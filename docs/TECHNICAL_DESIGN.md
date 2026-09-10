# MundiVeo — Technical Design (TD)

**Source of truth for technical scope.** This file is authoritative. `VIBE_CODE_BRIEF.md` is a generated summary of this document — if they ever disagree, this file wins.

For the narrative/motivation behind the project, see `../PROJECT_CONCEPT.md`. For functional scope, see `FUNCTIONAL_DESIGN.md`.

---

## 1. Technology Stack

| Component | Technology | Alternatives | Reason |
|---|---|---|---|
| Frontend | React + TypeScript | Vue.js, Svelte | Modular, popular, easy to extend |
| Styling | Tailwind CSS | Styled Components, CSS Modules | Fast, adaptable |
| Video player | Video.js | Plyr, HTML5 | Open-source, broadly compatible |
| Backend | Node.js + Express | Python (FastAPI), Go | Fast to build, scalable |
| Database | PostgreSQL | MongoDB, MySQL | Reliable, open-source, strong relational fit |
| Authentication | Firebase Auth / OAuth 2.0 | Passport.js | Easy to integrate, secure |
| Video storage | Central object storage (EU-based, e.g. OVH/Hetzner S3-compatible) | AWS S3 | GDPR-friendly, EU data residency |
| Transcoding | FFmpeg | CloudConvert | Open-source, powerful |
| Recommendations | Embedding/collaborative-filtering based system; LLM used for semantic search/tagging only (see Section 5) | — | Matches how recommendation systems actually work |
| Containerization | Docker | Kubernetes (later, at scale) | Easy to deploy |
| CI/CD | GitHub Actions | GitLab CI, Jenkins | Automated tests and deployments |
| Hosting | OVH / Hetzner (EU) | AWS, Google Cloud | Cheaper, GDPR-compliant, EU jurisdiction |

*Decentralized NAS/IPFS storage and age-verification integrations are intentionally excluded from the MVP stack — see `../PROJECT_CONCEPT.md` Section 3.*

## 2. Architecture Overview

```
┌───────────────────────────────────────────────────────┐
│                        Frontend                        │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐│
│  │   React     │    │  Video.js   │    │  Tailwind   ││
│  └─────────────┘    └─────────────┘    └─────────────┘│
└───────────────────────────────────────────────────────┘
                              │
                              ▼
┌───────────────────────────────────────────────────────┐
│                        Backend                         │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐│
│  │  Node.js    │    │  PostgreSQL │    │   FFmpeg    ││
│  └─────────────┘    └─────────────┘    └─────────────┘│
└───────────────────────────────────────────────────────┘
                              │
                              ▼
                  ┌─────────────────────┐
                  │  EU Object Storage  │
                  │   + CDN (OVH)       │
                  └─────────────────────┘
```

*(Decentralized NAS/P2P nodes to be added as an optional, later architectural branch — not part of the initial data flow.)*

## 3. Database Schema (Core Tables)

```sql
-- Users
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Videos
-- NOTE: storage_key / thumbnail_key hold internal object-storage keys/paths,
-- NOT public URLs. The backend constructs the actual playback/CDN URL at
-- request time from the key. This means switching storage providers or CDN
-- configuration later does not require a data migration.
CREATE TABLE videos (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    storage_key VARCHAR(255) NOT NULL,
    thumbnail_key VARCHAR(255),
    duration INTEGER, -- seconds
    views INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Tags
CREATE TABLE tags (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) UNIQUE NOT NULL
);

-- Video-Tags (many-to-many)
CREATE TABLE video_tags (
    video_id INTEGER REFERENCES videos(id),
    tag_id INTEGER REFERENCES tags(id),
    PRIMARY KEY (video_id, tag_id)
);

-- Comments
CREATE TABLE comments (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    video_id INTEGER REFERENCES videos(id),
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Likes
CREATE TABLE likes (
    user_id INTEGER REFERENCES users(id),
    video_id INTEGER REFERENCES videos(id),
    PRIMARY KEY (user_id, video_id)
);

-- Monetization
-- NOTE: schema is defined now to avoid a future migration; endpoints and
-- business logic are Phase 2 (see PROJECT_CONCEPT.md roadmap) and remain
-- stubbed until then. This is a deliberate sequencing choice, not a scope
-- disagreement with PROJECT_CONCEPT.md.
CREATE TABLE monetization (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    video_id INTEGER REFERENCES videos(id),
    ad_revenue_enabled BOOLEAN DEFAULT FALSE,
    donation_enabled BOOLEAN DEFAULT FALSE,
    subscription_enabled BOOLEAN DEFAULT FALSE
);
```

## 4. Key API Endpoints

| Endpoint | Method | Description | Auth |
|---|---|---|---|
| /api/videos | GET | List videos (with filtering) | No |
| /api/videos/:id | GET | Get a specific video | No |
| /api/videos | POST | Upload a new video | Yes (Creator) |
| /api/videos/:id/like | POST | Like a video | Yes |
| /api/videos/:id/comments | POST | Post a comment | Yes |
| /api/users/register | POST | Register a new user | No |
| /api/users/login | POST | Log in | No |
| /api/search | GET | Search videos | No |
| /api/recommendations | GET | Get recommended videos | Yes |

## 5. Recommendation System

The original draft described "an LLM that generates recommendations." This isn't how recommendation systems are actually built, so the design is corrected here:

**Realistic architecture:**
- **Core recommendation engine:** collaborative filtering and/or embedding-based similarity (e.g. two-tower models), trained on watch history, likes, and engagement signals — this is the actual workhorse behind systems like YouTube's or Spotify's.
- **LLM's actual role:** semantic search (understanding a search query beyond exact keyword match) and automated content tagging/categorization from titles and descriptions — genuinely useful, but a supporting role, not the recommendation engine itself.
- **Transparency angle (a real differentiator):** unlike YouTube, MundiVeo can show users *why* something was recommended ("because you watched X" / "popular among viewers of Y"), which fits the project's transparency principle and is technically straightforward with this architecture.

For the v1/cold-start implementation, see `VIBE_CODE_BRIEF.md` Section 5.

## 6 Advertiser / Platform Ad System ##
Scope: this section covers how companies get access to the platform, create ad campaigns, and buy platform-inventory ad time. It does not cover creator-side ad slots or the yearly profit-sharing pool — see MundiVeo_Fair_Monetization_Model.md for those, and the existing monetization table (Section 3) for the per-video creator settings, which remains unchanged by this addition.

1. Onboarding Advertisers register through a separate flow from Creator/User signup, providing company name, VAT/business registration number, and a billing contact. A basic verification step gates campaign creation — this is deliberately lighter than full KYC, but sufficient to filter out non-existent or obviously fraudulent entities before real money or ad slots are involved.

2. Ad creation and review Ad creative (video file) is uploaded much like a creator video upload, using the same storage-key pattern already defined in Section 3 (storage_key referencing internal object storage, not a public URL). Before a campaign can run, its creative goes through the same moderation/review pipeline planned for creator content — this avoids building a second, parallel moderation system for a content type that arguably needs more scrutiny than user uploads, not less, since it's paid platform content.

3. Fixed-rate pricing (rate card, not an auction) Each campaign is priced against a published, fixed rate per 1,000 impressions (CPM), set by the platform rather than determined through real-time bidding. This is the literal mechanism behind the "transparent alternative to opaque ad auctions" positioning — the rate card itself is what makes that claim concrete rather than aspirational.

4. Targeting — contextual only, not behavioral Targeting is scoped to content context (video category/tag, or a specific creator's content) rather than individual user tracking across sessions. This is a deliberate boundary, not an oversight: behavioral targeting would require building a user-tracking/profiling layer that directly conflicts with the platform's GDPR-native, privacy-respecting positioning. If this boundary is ever revisited, it should be treated as a governance decision (per GOVERNANCE.md Principle 2), not a quiet technical addition.

5. Budget and scheduling An advertiser sets a total budget and a date range at campaign creation. The campaign draws down against the platform-inventory ad pool (defined in MundiVeo_Fair_Monetization_Model.md Section 1.1) until either the budget is exhausted or the end date is reached, whichever comes first.

6. Reporting Advertisers can view impressions delivered, completion rate, and spend-to-date for each campaign — mirroring the transparency principle already applied to the recommendation system (Section 5): advertisers see what was delivered and roughly why (which targeting rule matched), not a black-box performance number.

7. Regulatory note — DSA ad-transparency The EU Digital Services Act includes advertising-transparency obligations (clearly identifying who paid for an ad; larger platforms must maintain a public ad repository). MundiVeo is not yet at the scale where the strictest obligations apply, but designing the ad system to make "who is behind this ad" visible by default from the start is significantly cheaper than retrofitting it later, and it's consistent with the platform's existing transparency principle. Full compliance scoping belongs with the broader legal review already flagged in PROJECT_CONCEPT.md Phase 5.

8. Proposed database schema additions

sql
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

9. Proposed API endpoint additions

Endpoint	Method	Description	Auth
/api/advertisers/register	POST	Register a new advertiser/company account	No
/api/advertisers/login	POST	Advertiser login	No
/api/campaigns	POST	Create a new ad campaign (creative, budget, targeting, dates)	Yes (Advertiser)
/api/campaigns/:id	GET	Get a campaign's status and details	Yes (Advertiser)
/api/campaigns/:id	PATCH	Pause, update, or cancel a campaign	Yes (Advertiser)
/api/campaigns/:id/report	GET	Impressions, completion rate, and spend for a campaign	Yes (Advertiser)
Open questions (not resolved by this draft — flag for later discussion, not blocking)
Exact verification depth for advertiser onboarding (self-declared VAT number vs. actual KYB check against a business registry).
Whether ad review is fully human, fully automated, or hybrid (mirrors the same open question already logged for creator sponsorship-detection review in MundiVeo_Fair_Monetization_Model.md Section 3.1).
Minimum/maximum campaign budget, and whether small businesses need a lower minimum spend than the rate card would otherwise imply.
How ad_campaign_tags targeting interacts with the platform-inventory slot count once that number is finalized (still a placeholder in the monetization model).
Payment collection mechanism for advertisers (Stripe/Mollie, matching the payment infrastructure already noted for creator payouts in the cost-timeline doc) — sequencing likely depends on entity formation, same as creator payouts.

## 7. Decentralized Storage (Deferred — Phase 2+)

**How it would work, once revisited:**
1. Creator opts in to contribute NAS storage.
2. The platform indexes the content (metadata only in the core database).
3. Other users stream via WebTorrent/IPFS from the contributing node.
4. Popular content is mirrored to central storage as a reliability backup.

**Open questions to resolve before this ships (tracked, not blocking the MVP):**
- How takedown requests propagate to distributed nodes in practice.
- Minimum reliability/backup guarantees so availability doesn't depend on one household's internet connection.
- Whether node operators need any form of agreement/liability waiver with the platform.

## 7. — AI-Assisted Development Policy


## 7.1 Why this section exists

MundiVeo's NLnet grant carries a Generative AI policy that draws a hard line between two things that look similar but are treated very differently:

- **AI-assisted development** — a human writes, reviews, and takes responsibility for the code; AI tools (autocomplete, pair-programming suggestions, refactor help) may speed up that human's work. This is compliant and payable.
- **AI-generated development** — code is produced wholesale by an AI tool and merged without a human genuinely authoring and understanding it. NLnet does not consider this eligible for payment as grant-funded work, and some NLnet-funded projects (e.g. NLnet Labs) reject AI-generated contributions outright.

This distinction is not a formality. It determines whether the hours spent building MundiVeo can actually be claimed against the NLnet grant, and it protects the project from a funder relationship problem later if this isn't decided up front — this is why it's being written into the TD now, before any contributor starts writing code, rather than after.

## 7.2 What counts as compliant AI assistance

Permitted, without special process beyond disclosure (7.3):

- Inline autocomplete / "ghost text" suggestions (e.g. Copilot-style, single lines or short blocks) that the author reviews and edits as their own.
- Using an AI assistant to explain, debug, or suggest a fix for code the author already wrote.
- Using an AI assistant as a rubber-duck / architecture sounding board, where the resulting design is written up and implemented by the human.
- AI-assisted refactoring or test-writing, provided the author reviews every change and can explain what it does and why.

**Not permitted as grant-payable work:**

- Prompting an AI tool to generate a feature, endpoint, or component wholesale and merging it with only superficial review ("vibe coding").
- Submitting AI output that the contributor cannot explain line-by-line if asked.
- Any AI-assisted output that has not been checked for FLOS-license compatibility (see 7.4).

**The test that decides which side of the line something is on:** *could the author sit down and explain, unaided, exactly what this code does and why it was written this way?* If not, it doesn't meet the human-authorship bar regardless of how it was produced.

## 7.3 Disclosure & prompt provenance logging

For any commit or pull request where AI assistance materially contributed to the change (i.e. beyond trivial autocomplete):

- The commit message or PR description must include a short disclosure line, e.g.:
  `AI-assisted: [tool name] used for [scope — e.g. "initial draft of validation logic, human-reviewed and modified"]`
- A prompt provenance log must be kept for that contribution, recording: the tool/model used, the date, and the substance of the prompt(s) that produced material output. This can live in a `docs/ai-provenance/` folder in the repo, one file per PR, rather than inline in commit messages, to keep history clean.
- Trivial autocomplete-level assistance does not need a full log entry, but should still be covered by the general disclosure line above where reasonably practical.

This logging exists to make MundiVeo's NLnet reporting straightforward and to give any future code reviewer or auditor a clear trail — not as a bureaucratic hurdle.

## 7.4 License and IP compatibility

Any AI-assisted output must be checked, before merge, for compatibility with MundiVeo's AGPL-3.0 licensing:

- Verify the AI tool's terms of use permit commercial/FLOS use of its output.
- Watch for output that closely resembles known copyrighted or non-FLOS-compatible source material (a risk with larger generated blocks — another reason to keep AI contributions small and human-reviewed rather than large and wholesale).

## 7.5 Contributor & reviewer responsibilities

- **Contributors** are responsible for disclosure and provenance logging on their own submissions, and must be able to explain their code in review if asked.
- **Code reviewers** (once in place — see open recruitment gap) are responsible for checking that disclosure requirements were followed and, where something reads as "too generated," asking the contributor to walk through it before approval.
- This section should be cross-referenced from `CONTRIBUTING.md` once that file exists, and factored into the CLA decision — a CLA can require this disclosure as a condition of contribution.

## 7.6 What this means for the roadmap

Practically: MundiVeo's coding phases (`PROJECT_CONCEPT.md` Section 3, Phases 2–4) should be planned around AI-*assisted*, human-authored timelines, not AI-*generated* ones. The original "1–2 weeks per phase" estimates assumed the latter and should be treated as superseded — see the separate timeline recalculation for current estimates.