# MundiVeo — Advertiser/Company System: FD & TD Additions (Draft)

**Purpose of this file:** a single staging document containing every proposed textual addition for the advertiser/company side of the platform — the missing piece identified today (how companies get access, create ads, and buy ad time). Organized by **target document and exact section**, so it can be reviewed once and then copy-pasted into `FUNCTIONAL_DESIGN.md` (FO) and `TECHNICAL_DESIGN.md` (TO) directly. Nothing in this file has been written into those documents yet.

This is the *mechanics* side only (roles, flows, schema, endpoints, rate-card/targeting logic) — consistent with the earlier decision to keep the fixed-price-vs-auction **marketing framing** out of the technical docs. It also complements, and should sit alongside, `MundiVeo_Fair_Monetization_Model.md` (the creator/platform ad-slot split) rather than duplicating it — that document defines *where the inventory comes from*; this one defines *who buys it and how*.

---

## FOR `FUNCTIONAL_DESIGN.md`

### Addition to Section 2 — User Roles

Add a new row to the roles table:

| Role | Description | Functions |
|---|---|---|
| Advertiser | Company/business buying ad inventory | Register company account, create and fund ad campaigns, upload ad creative, view campaign performance reports |

### Addition to Section 3 — Core Features

Add a new row to the features table:

| Feature | Description | Priority |
|---|---|---|
| Advertiser campaign management | Companies register, verify, create ad campaigns against fixed-rate platform inventory, and track delivery/spend | ⭐⭐ |

### New addition to Section 5 — User Flows

**C. Buying ad time (Advertiser)**
1. Company registers an advertiser account (company name, VAT/registration number, billing contact) — separate from Creator/User registration.
2. Account undergoes a basic verification step (know-your-business check) before any campaign can go live — a lighter check than payment processing itself requires, but enough to screen out obviously fraudulent or non-existent entities.
3. Advertiser creates a campaign: uploads ad creative (video file), sets a total budget, chooses contextual targeting (by video category/tag — not by individual user behavior), and sets a start/end date.
4. Campaign is reviewed before approval — reuses the same moderation pipeline already planned for creator content, since an unreviewed ad is a bigger reputational risk than an unreviewed user upload.
5. Once approved, the campaign runs against the platform's guaranteed ad inventory (see `MundiVeo_Fair_Monetization_Model.md` Section 1.1) until the budget is spent or the end date is reached.
6. Advertiser views a reporting dashboard showing impressions delivered, completion rate, and spend to date — the same transparency principle applied to recommendations extends here: advertisers can see exactly what their money bought.

---

## FOR `TECHNICAL_DESIGN.md`

### New Section — Advertiser / Platform Ad System

*(Suggested placement: after Section 5 "Recommendation System," renumbering Section 6 "Decentralized Storage" accordingly — or insert as a new Section 7, whichever fits the eventual document flow better.)*

**Scope:** this section covers how companies get access to the platform, create ad campaigns, and buy platform-inventory ad time. It does not cover creator-side ad slots or the yearly profit-sharing pool — see `MundiVeo_Fair_Monetization_Model.md` for those, and the existing `monetization` table (Section 3) for the per-video creator settings, which remains unchanged by this addition.

**1. Onboarding**
Advertisers register through a separate flow from Creator/User signup, providing company name, VAT/business registration number, and a billing contact. A basic verification step gates campaign creation — this is deliberately lighter than full KYC, but sufficient to filter out non-existent or obviously fraudulent entities before real money or ad slots are involved.

**2. Ad creation and review**
Ad creative (video file) is uploaded much like a creator video upload, using the same storage-key pattern already defined in Section 3 (`storage_key` referencing internal object storage, not a public URL). Before a campaign can run, its creative goes through the same moderation/review pipeline planned for creator content — this avoids building a second, parallel moderation system for a content type that arguably needs *more* scrutiny than user uploads, not less, since it's paid platform content.

**3. Fixed-rate pricing (rate card, not an auction)**
Each campaign is priced against a published, fixed rate per 1,000 impressions (CPM), set by the platform rather than determined through real-time bidding. This is the literal mechanism behind the "transparent alternative to opaque ad auctions" positioning — the rate card itself is what makes that claim concrete rather than aspirational.

**4. Targeting — contextual only, not behavioral**
Targeting is scoped to **content context** (video category/tag, or a specific creator's content) rather than individual user tracking across sessions. This is a deliberate boundary, not an oversight: behavioral targeting would require building a user-tracking/profiling layer that directly conflicts with the platform's GDPR-native, privacy-respecting positioning. If this boundary is ever revisited, it should be treated as a governance decision (per `GOVERNANCE.md` Principle 2), not a quiet technical addition.

**5. Budget and scheduling**
An advertiser sets a total budget and a date range at campaign creation. The campaign draws down against the platform-inventory ad pool (defined in `MundiVeo_Fair_Monetization_Model.md` Section 1.1) until either the budget is exhausted or the end date is reached, whichever comes first.

**6. Reporting**
Advertisers can view impressions delivered, completion rate, and spend-to-date for each campaign — mirroring the transparency principle already applied to the recommendation system (Section 5): advertisers see *what* was delivered and roughly *why* (which targeting rule matched), not a black-box performance number.

**7. Regulatory note — DSA ad-transparency**
The EU Digital Services Act includes advertising-transparency obligations (clearly identifying who paid for an ad; larger platforms must maintain a public ad repository). MundiVeo is not yet at the scale where the strictest obligations apply, but designing the ad system to make "who is behind this ad" visible by default from the start is significantly cheaper than retrofitting it later, and it's consistent with the platform's existing transparency principle. Full compliance scoping belongs with the broader legal review already flagged in `PROJECT_CONCEPT.md` Phase 5.

**8. Proposed database schema additions**

```sql
-- Advertisers (companies/businesses buying ad inventory)
CREATE TABLE advertisers (
    id SERIAL PRIMARY KEY,
    company_name VARCHAR(255) NOT NULL,
    vat_number VARCHAR(50),
    billing_email VARCHAR(100) NOT NULL,
    contact_name VARCHAR(100),
    status VARCHAR(20) DEFAULT 'pending_verification', -- pending_verification, verified, suspended
    created_at TIMESTAMP DEFAULT NOW()
);

-- Ad Campaigns
CREATE TABLE ad_campaigns (
    id SERIAL PRIMARY KEY,
    advertiser_id INTEGER REFERENCES advertisers(id),
    name VARCHAR(255) NOT NULL,
    creative_storage_key VARCHAR(255) NOT NULL, -- same storage-key pattern as videos.storage_key
    budget_total NUMERIC(10,2) NOT NULL,
    budget_spent NUMERIC(10,2) DEFAULT 0,
    rate_cpm NUMERIC(6,2) NOT NULL, -- fixed price per 1,000 impressions, locked in at campaign creation
    start_date DATE,
    end_date DATE,
    status VARCHAR(20) DEFAULT 'pending_review', -- pending_review, approved, rejected, running, paused, completed
    created_at TIMESTAMP DEFAULT NOW()
);

-- Ad Campaign Targeting (contextual only — reuses existing tags table, no user-level tracking)
CREATE TABLE ad_campaign_tags (
    campaign_id INTEGER REFERENCES ad_campaigns(id),
    tag_id INTEGER REFERENCES tags(id),
    PRIMARY KEY (campaign_id, tag_id)
);

-- Ad Impressions (backs the advertiser reporting dashboard)
CREATE TABLE ad_impressions (
    id SERIAL PRIMARY KEY,
    campaign_id INTEGER REFERENCES ad_campaigns(id),
    video_id INTEGER REFERENCES videos(id),
    served_at TIMESTAMP DEFAULT NOW(),
    completed BOOLEAN DEFAULT FALSE -- whether the viewer watched through the ad slot
);
```

**9. Proposed API endpoint additions**

| Endpoint | Method | Description | Auth |
|---|---|---|---|
| /api/advertisers/register | POST | Register a new advertiser/company account | No |
| /api/advertisers/login | POST | Advertiser login | No |
| /api/campaigns | POST | Create a new ad campaign (creative, budget, targeting, dates) | Yes (Advertiser) |
| /api/campaigns/:id | GET | Get a campaign's status and details | Yes (Advertiser) |
| /api/campaigns/:id | PATCH | Pause, update, or cancel a campaign | Yes (Advertiser) |
| /api/campaigns/:id/report | GET | Impressions, completion rate, and spend for a campaign | Yes (Advertiser) |

---

## Open questions (not resolved by this draft — flag for later discussion, not blocking)

- Exact verification depth for advertiser onboarding (self-declared VAT number vs. actual KYB check against a business registry).
- Whether ad review is fully human, fully automated, or hybrid (mirrors the same open question already logged for creator sponsorship-detection review in `MundiVeo_Fair_Monetization_Model.md` Section 3.1).
- Minimum/maximum campaign budget, and whether small businesses need a lower minimum spend than the rate card would otherwise imply.
- How `ad_campaign_tags` targeting interacts with the platform-inventory slot count once that number is finalized (still a placeholder in the monetization model).
- Payment collection mechanism for advertisers (Stripe/Mollie, matching the payment infrastructure already noted for creator payouts in the cost-timeline doc) — sequencing likely depends on entity formation, same as creator payouts.
