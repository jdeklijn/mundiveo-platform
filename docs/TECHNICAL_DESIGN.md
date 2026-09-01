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

## 6. Decentralized Storage (Deferred — Phase 2+)

**How it would work, once revisited:**
1. Creator opts in to contribute NAS storage.
2. The platform indexes the content (metadata only in the core database).
3. Other users stream via WebTorrent/IPFS from the contributing node.
4. Popular content is mirrored to central storage as a reliability backup.

**Open questions to resolve before this ships (tracked, not blocking the MVP):**
- How takedown requests propagate to distributed nodes in practice.
- Minimum reliability/backup guarantees so availability doesn't depend on one household's internet connection.
- Whether node operators need any form of agreement/liability waiver with the platform.
