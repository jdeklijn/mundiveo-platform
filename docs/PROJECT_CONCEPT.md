# MundiVeo — European Video Platform Concept

**Status:** Concept document (v2, English) — rewritten for public documentation via OBS recordings, in line with the project's transparency-from-day-1 principle.

---

## 1. Project Goal

Build a European, semi-open-source video platform as an alternative to YouTube, with:

- A modern, responsive interface that is genuinely user-friendly for viewers and creators (unlike PeerTube's rough edges or Dailymotion's dated UI).
- Governance inspired by open-source projects (Linux-style): the community can propose and vote on major platform decisions, while a core team retains operational responsibility.
- An honest, working recommendation system (not a marketing buzzword — see Section 6).
- Fair monetization for creators.
- GDPR compliance and adherence to EU law by design, not bolted on afterward.
- Full public documentation of the build process from day one, via video (OBS).

### Target audience
- **Creators:** European YouTubers frustrated with demonetization, opaque moderation, and AI-generated content flooding recommendations.
- **Viewers:** People who value privacy, local/regional content, and transparency about how the platform works.

### Naming
Working name: **MundiVeo**. An initial check suggests the name is likely available as a domain and project name, but a formal trademark search (EUIPO register, not just domain availability) is still needed before heavy investment in branding.

---

## 2. Functional Design (FD)

### 2.1 User Roles

| Role | Description | Functions |
|---|---|---|
| Visitor | Not logged in | Watch videos (limited), search |
| User | Logged in | Watch, like, comment, manage profile |
| Creator | Uploads content | Upload, set monetization, view analytics |
| Moderator | Manages content | Review videos, handle copyright claims |
| Admin | Manages platform | Manage users, platform settings |

*Note: an "18+ verified user" role is deliberately **not** part of the initial rollout — see Section 3.*

### 2.2 Core Features

| Feature | Description | Priority |
|---|---|---|
| Video upload | Creators upload video files (MP4, WebM, etc.) | ⭐⭐⭐ |
| Video playback | Responsive player with quality options (1080p, 720p, etc.) | ⭐⭐⭐ |
| Search | Search by title, tags, description | ⭐⭐⭐ |
| Recommendations | Behavior- and content-based suggestions (see Section 6) | ⭐⭐ |
| Monetization | Ad revenue share, donations, subscriptions | ⭐⭐ |
| Decentralized storage (opt-in, later phase) | Creators/users may optionally contribute NAS storage | ⭐ |
| Comments | Moderated comment section | ⭐ |
| Analytics | Creators see views, likes, revenue | ⭐ |
| User profile | Uploads, subscriptions, settings | ⭐ |
| Notifications | New videos, replies, etc. | ⭐ |

### 2.3 Unique Selling Points

- **European focus:** local languages, GDPR-compliant, not dependent on US Big Tech infrastructure.
- **Semi-open governance:** the community can influence roadmap and policy decisions, with a transparent decision-making process.
- **Honest recommendations:** a system that is explainable, not a black box.
- **Fair monetization:** clearer, more creator-favorable revenue terms than the current YouTube model.
- **Radical transparency:** the entire build process is documented on video from day one.

### 2.4 User Flows

**A. Watching a video (Visitor/User)**
1. Visitor lands on homepage → sees trending videos.
2. Clicks a video → player loads (with suggestions alongside).
3. User can like, comment, share.

**B. Uploading a video (Creator)**
1. Creator logs in → clicks "Upload."
2. Selects video file → transcoding runs (FFmpeg).
3. Adds title, description, tags.
4. Chooses monetization options (ads, donations, etc.).
5. Video is published to central storage.

*(Decentralized/NAS storage flow is intentionally deferred — see Section 3.2)*

---

## 3. Deliberate Scope Decisions (v2 changes from the original draft)

This section documents design decisions that were reconsidered during early concept review, and why — in the interest of the project's transparency principle.

### 3.1 No mandatory adult (18+) content at launch

The original draft included 18+ content with age verification as a core feature. This has been **moved out of the initial scope**, for practical rather than moralistic reasons:

- Platforms built specifically around adult content (e.g. OnlyFans-style services) already serve that need well, with purpose-built verification, payment, and moderation infrastructure.
- Combining a general-purpose video platform with adult content under one brand creates two very different user bases, two reputational risk profiles, and two regulatory regimes (this mirrors why platforms like Reddit handle NSFW content as a distinct, opt-in subsystem rather than folding it into the core product).
- Removing this from the MVP significantly reduces legal and technical complexity (no mandatory age-verification infrastructure, no immediate exposure to the strictest content-moderation obligations under EU platform regulation).

**Governance note:** the community-governance model can still put this to a vote in the future. If the community decides to pursue it, it should come with the dedicated technical and legal investment such a category requires (proper age-verification provider, audit trail for compliance purposes, separate content/payment handling) rather than being retrofitted onto the general platform.

### 3.2 Decentralized (NAS) storage — deferred, not abandoned

User-hosted NAS/P2P storage (via WebTorrent/IPFS) remains an interesting long-term idea for cost and community ownership, but it is deferred past the MVP because:

- The platform (not the individual NAS owner) remains legally responsible for notice-and-takedown obligations under EU platform regulation, regardless of where content is technically hosted.
- Reliability suffers when availability depends on individual users' home hardware and connections.

This is kept on the roadmap as a **Phase 2+ initiative**, to be revisited once the core platform and moderation processes are mature enough to support it responsibly.

### 3.3 Age verification method, if/when 18+ is ever introduced

The original draft referenced DigiD (a Dutch government authentication system) for age verification. DigiD is restricted to government and designated public-sector services and is **not available for a commercial video platform** — this was a factual correction, not a design opinion.

If the community ever votes to introduce adult content, the intended approach — a one-time verification (e.g. via a payment method or a dedicated age-verification provider) with minimal data retention — remains sound in principle, but would need:
- A NL-specific option such as iDIN (bank-based, unlike DigiD) or a general-purpose age-verification provider (e.g. Yoti, Veriff) for other markets.
- A GDPR-compliant retention approach: full data minimization while still keeping enough of an audit trail (verification method + date, not the underlying document/card data) to demonstrate compliance to a regulator if ever asked. "Verify once, then completely forget everything" and "be able to prove you verified" are in some tension and need a lawyer's input on the specific retention model.
- Awareness that a card transaction alone is an increasingly weak proxy for age in the eyes of EU regulators, and may not suffice on its own for explicit content going forward.

This entire topic is parked until/unless the community decides it's in scope.

---

## 4. Technical Design (TD)

### 4.1 Technology Stack

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
| Recommendations | Embedding/collaborative-filtering based system; LLM used for semantic search/tagging only (see Section 6) | — | Matches how recommendation systems actually work |
| Containerization | Docker | Kubernetes (later, at scale) | Easy to deploy |
| CI/CD | GitHub Actions | GitLab CI, Jenkins | Automated tests and deployments |
| Hosting | OVH / Hetzner (EU) | AWS, Google Cloud | Cheaper, GDPR-compliant, EU jurisdiction |

*Decentralized NAS/IPFS storage and DigiD/age-verification integrations are intentionally excluded from the MVP stack per Section 3.*

### 4.2 Architecture Overview

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

### 4.3 Database Schema (Core Tables)

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
CREATE TABLE videos (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    url VARCHAR(255) NOT NULL,
    thumbnail_url VARCHAR(255),
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
CREATE TABLE monetization (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    video_id INTEGER REFERENCES videos(id),
    ad_revenue_enabled BOOLEAN DEFAULT FALSE,
    donation_enabled BOOLEAN DEFAULT FALSE,
    subscription_enabled BOOLEAN DEFAULT FALSE
);
```

### 4.4 Key API Endpoints

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

---

## 5. Decentralized Storage (Deferred — Phase 2+)

**How it would work, once revisited:**
1. Creator opts in to contribute NAS storage.
2. The platform indexes the content (metadata only in the core database).
3. Other users stream via WebTorrent/IPFS from the contributing node.
4. Popular content is mirrored to central storage as a reliability backup.

**Open questions to resolve before this ships (tracked, not blocking the MVP):**
- How takedown requests propagate to distributed nodes in practice.
- Minimum reliability/backup guarantees so availability doesn't depend on one household's internet connection.
- Whether node operators need any form of agreement/liability waiver with the platform.

---

## 6. Recommendation System (Corrected Design)

The original draft described "an LLM that generates recommendations." This isn't how recommendation systems are actually built, so the design is corrected here:

**Realistic architecture:**
- **Core recommendation engine:** collaborative filtering and/or embedding-based similarity (e.g. two-tower models), trained on watch history, likes, and engagement signals — this is the actual workhorse behind systems like YouTube's or Spotify's.
- **LLM's actual role:** semantic search (understanding a search query beyond exact keyword match) and automated content tagging/categorization from titles and descriptions — genuinely useful, but a supporting role, not the recommendation engine itself.
- **Transparency angle (a real differentiator):** unlike YouTube, MundiVeo can show users *why* something was recommended ("because you watched X" / "popular among viewers of Y"), which fits the project's transparency principle and is technically straightforward with this architecture.

This should be treated as a proper ML engineering task once the MVP has enough usage data to make recommendations meaningful at all — a cold-start plan (e.g. trending + tag-based suggestions) is needed for the early days regardless.

---

## 7. Monetization

| Method | Implementation | Notes |
|---|---|---|
| Ad revenue | Ad network integration (EU-based where possible) | Exact revenue split to be modeled against real hosting/CDN costs before publishing a promised percentage |
| Donations | Stripe/PayPal | Direct viewer support |
| Subscriptions | Monthly, ad-free tier | Recurring revenue |
| Sponsorships | Creator-brand deals | Additional creator income |

**Open item:** cost-per-GB-streamed and cost-per-active-user need to be modeled with real infrastructure quotes before any revenue-split percentage is publicly promised. Bandwidth and storage costs scale directly with growth and are historically what makes video platforms expensive to run at scale.

---

## 8. Revised Roadmap (AI-accelerated development)

Technical development can move fast with AI-assisted coding tools. The real bottlenecks are legal groundwork, fundraising, and community-building — the roadmap reflects that.

| Phase | Timeframe | Goals | Deliverables |
|---|---|---|---|
| 1. Design & Validation | 1–2 days | Functional/technical design, name check, informal validation with creators | FD, TD, wireframes, GitHub repo |
| 2. MVP (AI-assisted build) | 1–2 weeks | Core frontend + backend + upload/playback/search | Working demo: video grid, player, upload |
| 3. Recommendations v1 | 1–2 weeks | Tag-based + trending as cold start; collaborative filtering once data exists | Working "suggested videos" |
| 4. Monetization | 1 week | Ad integration, donations, subscriptions | Creators can earn |
| 5. Legal Compliance | 4–8 weeks (parallel) | GDPR review, terms of service, moderation policy, DSA obligations assessment | Legally reviewed platform |
| 6. Fundraising | 4–12 weeks (parallel) | Pitch deck, investor conversations, possible crowdfunding | Funding secured / runway extended |
| 7. Community Building | Ongoing | Recruit early creators and viewers, set up governance process | Active early community |
| 8. Launch | 2–4 weeks | Public launch, marketing push | Live platform |
| 9. Scale & Iterate | Ongoing | Decentralized storage (Phase 2), governance votes on scope (e.g. 18+), optimization | Growing platform |

**Key takeaway:** technical build-out for a real MVP is realistically 1–2 months with AI tooling; legal, fundraising, and community work run in parallel and take 3–6 months and are the actual critical path.

---

## 9. GitHub Repository Structure

```
mundiveo-platform/
│
├── docs/
│   ├── FUNCTIONAL_DESIGN.md
│   ├── TECHNICAL_DESIGN.md
│   ├── API_DOCUMENTATION.md
│   └── SETUP_GUIDE.md
│
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   │   ├── VideoPlayer.jsx
│   │   │   ├── VideoGrid.jsx
│   │   │   └── UploadForm.jsx
│   │   ├── pages/
│   │   │   ├── Home.jsx
│   │   │   ├── VideoPage.jsx
│   │   │   └── Upload.jsx
│   │   ├── styles/
│   │   ├── App.jsx
│   │   └── index.js
│   ├── package.json
│   └── Dockerfile
│
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── services/
│   │   └── app.js
│   ├── package.json
│   └── Dockerfile
│
├── scripts/
│   ├── setup_database.sql
│   └── transcoding_script.sh
│
├── docker-compose.yml
└── README.md
```

---

## 10. Summary — Why This Can Work

- **Real market gap:** creators are actively frustrated with YouTube's demonetization and algorithm opacity; there is demand for a credible European alternative.
- **European focus:** GDPR-by-design, EU hosting, not dependent on US Big Tech.
- **Realistic technical scope:** MVP focuses on what actually matters first (upload, playback, search, basic recommendations), deferring higher-risk/higher-complexity features (18+, decentralized storage) until the community and infrastructure are ready for them.
- **Honest recommendation design:** transparency about *why* something is recommended, built on a technically sound foundation rather than a buzzword.
- **AI-accelerated build:** technical development timelines are compressed significantly compared to a traditional build, freeing up time and focus for the legal and community work that actually determines success.

---

## 11. Next Steps

1. Set up the GitHub repository with the structure in Section 9.
2. Produce wireframes (homepage, video page, upload page) — first OBS-documented milestone.
3. Build MVP frontend (video grid + player + search, dummy data).
4. Build MVP backend (auth, video upload endpoint, PostgreSQL schema).
5. Engage a lawyer for GDPR/ToS/moderation-policy review — start in parallel, not after building.
6. Draft pitch materials for early investor/crowdfunding conversations.
7. Set up community channels and publish the governance model draft.
8. Begin EUIPO trademark search for "MundiVeo" alongside the domain check already done.
