# MundiVeo — Functional Design (FD)

**Source of truth for functional scope.** This file is authoritative. `VIBE_CODE_BRIEF.md` is a generated summary of this document — if they ever disagree, this file wins.

For the narrative/motivation behind the project, see `../PROJECT_CONCEPT.md`. For the technical implementation, see `TECHNICAL_DESIGN.md`.

---

## 1. Target Audience

- **Creators:** European YouTubers frustrated with demonetization, opaque moderation, and AI-generated content flooding recommendations.
- **Viewers:** People who value privacy, local/regional content, and transparency about how the platform works.

## 2. User Roles

| Role | Description | Functions |
|---|---|---|
| Visitor | Not logged in | Watch videos (limited), search |
| User | Logged in | Watch, like, comment, manage profile |
| Creator | Uploads content | Upload, set monetization, view analytics |
| Advertiser | Company/business buying ad inventory | Register company account, create and fund ad campaigns, upload ad creative, view campaign performance reports
| Moderator | Manages content | Review videos, handle copyright claims |
| Admin | Manages platform | Manage users, platform settings |


*Note: an "18+ verified user" role is deliberately **not** part of the initial rollout — see `PROJECT_CONCEPT.md` Section 3.1.*

## 3. Core Features

| Feature | Description | Priority |
|---|---|---|
| Video upload | Creators upload video files (MP4, WebM, etc.) | ⭐⭐⭐ |
| Video playback | Responsive player with quality options (1080p, 720p, etc.) | ⭐⭐⭐ |
| Search | Search by title, tags, description | ⭐⭐⭐ |
| Recommendations | Behavior- and content-based suggestions (see `TECHNICAL_DESIGN.md` Section 5) | ⭐⭐ |
| Monetization | Ad revenue share, donations, subscriptions — schema now, functionality Phase 2 (see `TECHNICAL_DESIGN.md` monetization note) | ⭐⭐ |
| Advertiser campaign management |	Companies register, verify, create ad campaigns against fixed-rate platform inventory, and track delivery/spend |	⭐⭐ |
| Decentralized storage (opt-in, later phase) | Creators/users may optionally contribute NAS storage | ⭐ |
| Comments | Moderated comment section | ⭐ |
| Analytics | Creators see views, likes, revenue | ⭐ |
| User profile | Uploads, subscriptions, settings | ⭐ |
| Notifications | New videos, replies, etc. | ⭐ |

## 4. Unique Selling Points

- **European focus:** local languages, GDPR-compliant, not dependent on US Big Tech infrastructure.
- **Semi-open governance:** the community can influence roadmap and policy decisions, with a transparent decision-making process.
- **Honest recommendations:** a system that is explainable, not a black box.
- **Fair monetization:** clearer, more creator-favorable revenue terms than the current YouTube model.
- **Radical transparency:** the entire build process is documented on video from day one.

## 5. User Flows

**A. Watching a video (Visitor/User)**
1. Visitor lands on homepage → sees trending videos.
2. Clicks a video → player loads (with suggestions alongside).
3. User can like, comment, share.

**B. Uploading a video (Creator)**
1. Creator logs in → clicks "Upload."
2. Selects video file → transcoding runs (FFmpeg).
3. Adds title, description, tags.
4. Chooses monetization options (ads, donations, etc.) — UI can be present, backend stays stubbed until Phase 2.

**C. Buying ad time (Advertiser)**
1. Company registers an advertiser account (company name, VAT/registration number, billing contact) — separate from Creator/User registration.
2. Account undergoes a basic verification step (know-your-business check) before any campaign can go live — a lighter check than payment processing itself requires, but enough to screen out obviously fraudulent or non-existent entities.
3. Advertiser creates a campaign: uploads ad creative (video file), sets a total budget, chooses contextual targeting (by video category/tag — not by individual user behavior), and sets a start/end date.
4. Campaign is reviewed before approval — reuses the same moderation pipeline already planned for creator content, since an unreviewed ad is a bigger reputational risk than an unreviewed user upload.
5. Once approved, the campaign runs against the platform's guaranteed ad inventory (see MundiVeo_Fair_Monetization_Model.md Section 1.1) until the budget is spent or the end date is reached.
6. Advertiser views a reporting dashboard showing impressions delivered, completion rate, and spend to date — the same transparency principle applied to recommendations extends here: advertisers can see exactly what their money bought.

*(Decentralized/NAS storage flow is intentionally deferred — see `PROJECT_CONCEPT.md` Section 3.2)*
