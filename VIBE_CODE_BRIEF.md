# MundiVeo — Build Brief for Mistral Vibe Code

**Purpose of this file:** this is the working spec for AI-assisted code generation. Read this before generating any code. It reflects deliberate, already-made scope decisions — don't re-introduce items marked "excluded from MVP" without a human decision first.

---

## 0. One-paragraph context

MundiVeo is a European, community-governed video platform (YouTube-alternative). MVP goal: a working, EU-hosted platform with upload, playback, search, and basic recommendations — nothing more. The full rationale and long-form design doc lives in `docs/FUNCTIONAL_DESIGN.md` and `docs/TECHNICAL_DESIGN.md`. This brief is the condensed, code-generation-ready version.

---

## 1. Explicitly OUT of scope for MVP — do not implement these yet

If a prompt or ticket asks for any of these, flag it back to the human rather than building it:

- ❌ Any 18+ / adult content flag, age-gating modal, or age-verification integration (DigiD, iDIN, Stripe-based age check, etc.). Not until a separate governance decision authorizes it.
- ❌ Decentralized NAS / WebTorrent / IPFS storage. Central EU object storage only for now.
- ❌ "LLM generates recommendations" — do not wire an LLM call directly into the recommendation endpoint. See Section 5 for the correct approach.
- ❌ Ad-network integration, payment processing, subscriptions — monetization is Phase 2. Build the `monetization` table/schema but leave endpoints as stubs.

## 2. Tech stack (use exactly this unless a human changes it)

| Layer | Choice |
|---|---|
| Frontend | React + TypeScript, Tailwind CSS |
| Video player | Video.js |
| Backend | Node.js + Express |
| Database | PostgreSQL |
| Auth | OAuth 2.0 / Passport.js (email+password to start; social login later) |
| Transcoding | FFmpeg (server-side, invoked via child process or a queue worker) |
| Storage | S3-compatible EU object storage (OVH/Hetzner) — use env-configurable bucket, don't hardcode a provider SDK if avoidable |
| Containerization | Docker + docker-compose for local dev |

## 3. MVP feature list, in build order

1. **Auth** — register, login, JWT session, basic profile.
2. **Video upload** — file upload → FFmpeg transcode (at minimum 720p output) → store to object storage → save metadata to Postgres.
3. **Video playback** — Video.js player page, quality selector if multiple renditions exist.
4. **Video grid / homepage** — paginated list, sorted by recency to start (trending logic comes later).
5. **Search** — Postgres full-text search on title/description/tags to start. No LLM dependency required for v1 search.
6. **Comments & likes** — basic CRUD, one like per user per video.
7. **Creator dashboard (minimal)** — list of own uploads, view counts.

## 4. Database schema (authoritative — use this, don't redesign)

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE videos (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    url VARCHAR(255) NOT NULL,
    thumbnail_url VARCHAR(255),
    duration INTEGER,
    views INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE tags (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE video_tags (
    video_id INTEGER REFERENCES videos(id),
    tag_id INTEGER REFERENCES tags(id),
    PRIMARY KEY (video_id, tag_id)
);

CREATE TABLE comments (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    video_id INTEGER REFERENCES videos(id),
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE likes (
    user_id INTEGER REFERENCES users(id),
    video_id INTEGER REFERENCES videos(id),
    PRIMARY KEY (user_id, video_id)
);

-- Stub for Phase 2 — create the table now, leave endpoints unimplemented
CREATE TABLE monetization (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    video_id INTEGER REFERENCES videos(id),
    ad_revenue_enabled BOOLEAN DEFAULT FALSE,
    donation_enabled BOOLEAN DEFAULT FALSE,
    subscription_enabled BOOLEAN DEFAULT FALSE
);
```

## 5. Recommendations — correct approach for v1

Do NOT call an LLM per-request to "generate" recommendations. For v1:
- Cold start: return trending (most-viewed in last 7 days) + same-tag videos.
- Once there's watch-history data: simple item-based collaborative filtering (co-occurrence of videos watched in the same session is enough for v1 — no need for a full two-tower model yet).
- LLM's only legitimate v1 role: improving full-text search relevance (e.g. query expansion) and auto-suggesting tags at upload time. Keep this isolated in `backend/src/services/` behind a clean interface so it can be swapped or removed without touching core logic.

## 6. API endpoints for v1

| Endpoint | Method | Auth |
|---|---|---|
| /api/users/register | POST | No |
| /api/users/login | POST | No |
| /api/videos | GET | No |
| /api/videos/:id | GET | No |
| /api/videos | POST | Yes |
| /api/videos/:id/like | POST | Yes |
| /api/videos/:id/comments | POST | Yes |
| /api/search | GET | No |
| /api/recommendations | GET | Yes |

## 7. Non-negotiable constraints

- **EU data residency**: no US-based storage or database hosting for user/video data. Config should make the region explicit and hard to misconfigure by accident.
- **GDPR basics baked in now**: password hashing (bcrypt/argon2), no storing plaintext secrets, a `deleted_at` soft-delete pattern for user accounts (supports right-to-erasure workflows later without a schema migration).
- **No dark patterns**: no auto-play-into-next-video without a clear, easy-to-disable setting; no infinite scroll without a visible "you're all caught up" style boundary — this is a stated project value (transparency-first), not just a nice-to-have.

## 8. What "done" looks like for the MVP

A user can register, upload a video, see it transcode and appear in the grid, another user can find it via search, watch it, like it, and comment — all running locally via `docker-compose up`, and all data in EU-region-configured storage. That's the bar. Everything else is Phase 2+.
