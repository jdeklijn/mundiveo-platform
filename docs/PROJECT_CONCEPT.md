# MundiVeo — Project Concept & Narrative

**What this file is:** the "why" behind the project — motivation, naming, deliberate scope decisions, roadmap, and next steps. For the actual specs, see:
- [`docs/FUNCTIONAL_DESIGN.md`](docs/FUNCTIONAL_DESIGN.md) — user roles, features, user flows
- [`docs/TECHNICAL_DESIGN.md`](docs/TECHNICAL_DESIGN.md) — stack, architecture, database schema, API, recommendations

---

## 1. Project Goal

Build a European, semi-open-source video platform as an alternative to YouTube, with:

- A modern, responsive interface that is genuinely user-friendly for viewers and creators (unlike PeerTube's rough edges or Dailymotion's dated UI).
- Governance inspired by open-source projects (Linux-style): the community can propose and vote on major platform decisions, while a core team retains operational responsibility.
- An honest, working recommendation system (not a marketing buzzword).
- Fair monetization for creators.
- GDPR compliance and adherence to EU law by design, not bolted on afterward.
- Full public documentation of the build process from day one, via video (OBS).

### Target audience
- **Creators:** European YouTubers frustrated with demonetization, opaque moderation, and AI-generated content flooding recommendations.
- **Viewers:** People who value privacy, local/regional content, and transparency about how the platform works.

### Naming
Working name: **MundiVeo**. Domains `mundiveo.com` and `mundiveo.eu` have been registered (see `EXPENSES.md`). A formal EUIPO trademark search is still needed before heavy investment in branding — tracked in the internal repo's `legal/` folder.

---

## 2. Deliberate Scope Decisions (v2 changes from the original draft)

This section documents design decisions that were reconsidered during early concept review, and why — in the interest of the project's transparency principle.

### 2.1 No mandatory adult (18+) content at launch

The original draft included 18+ content with age verification as a core feature. This has been **moved out of the initial scope**, for practical rather than moralistic reasons:

- Platforms built specifically around adult content (e.g. OnlyFans-style services) already serve that need well, with purpose-built verification, payment, and moderation infrastructure.
- Combining a general-purpose video platform with adult content under one brand creates two very different user bases, two reputational risk profiles, and two regulatory regimes (this mirrors why platforms like Reddit handle NSFW content as a distinct, opt-in subsystem rather than folding it into the core product).
- Removing this from the MVP significantly reduces legal and technical complexity (no mandatory age-verification infrastructure, no immediate exposure to the strictest content-moderation obligations under EU platform regulation).

**Governance note:** the community-governance model can still put this to a vote in the future. If the community decides to pursue it, it should come with the dedicated technical and legal investment such a category requires (proper age-verification provider, audit trail for compliance purposes, separate content/payment handling) rather than being retrofitted onto the general platform.

### 2.2 Decentralized (NAS) storage — deferred, not abandoned

User-hosted NAS/P2P storage (via WebTorrent/IPFS) remains an interesting long-term idea for cost and community ownership, but it is deferred past the MVP because:

- The platform (not the individual NAS owner) remains legally responsible for notice-and-takedown obligations under EU platform regulation, regardless of where content is technically hosted.
- Reliability suffers when availability depends on individual users' home hardware and connections.

This is kept on the roadmap as a **Phase 2+ initiative**. Technical details in `docs/TECHNICAL_DESIGN.md` Section 6.

### 2.3 Age verification method, if/when 18+ is ever introduced

The original draft referenced DigiD (a Dutch government authentication system) for age verification. DigiD is restricted to government and designated public-sector services and is **not available for a commercial video platform** — this was a factual correction, not a design opinion.

If the community ever votes to introduce adult content, the intended approach — a one-time verification (e.g. via a payment method or a dedicated age-verification provider) with minimal data retention — remains sound in principle, but would need:
- A NL-specific option such as iDIN (bank-based, unlike DigiD) or a general-purpose age-verification provider (e.g. Yoti, Veriff) for other markets.
- A GDPR-compliant retention approach: full data minimization while still keeping enough of an audit trail (verification method + date, not the underlying document/card data) to demonstrate compliance to a regulator if ever asked. "Verify once, then completely forget everything" and "be able to prove you verified" are in some tension and need a lawyer's input on the specific retention model.
- Awareness that a card transaction alone is an increasingly weak proxy for age in the eyes of EU regulators, and may not suffice on its own for explicit content going forward.

This entire topic is parked until/unless the community decides it's in scope.

---

## 3. Revised Roadmap (AI-accelerated development)

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

## 4. Summary — Why This Can Work

- **Real market gap:** creators are actively frustrated with YouTube's demonetization and algorithm opacity; there is demand for a credible European alternative.
- **European focus:** GDPR-by-design, EU hosting, not dependent on US Big Tech.
- **Realistic technical scope:** MVP focuses on what actually matters first (upload, playback, search, basic recommendations), deferring higher-risk/higher-complexity features (18+, decentralized storage) until the community and infrastructure are ready for them.
- **Honest recommendation design:** transparency about *why* something is recommended, built on a technically sound foundation rather than a buzzword.
- **AI-accelerated build:** technical development timelines are compressed significantly compared to a traditional build, freeing up time and focus for the legal and community work that actually determines success.

---

## 5. Next Steps

1. ~~Set up the GitHub repository~~ — done (public + internal repos).
2. ~~Register domains~~ — `mundiveo.com` and `mundiveo.eu` registered, see `EXPENSES.md`.
3. Produce wireframes (homepage, video page, upload page) — first OBS-documented milestone.
4. Build MVP frontend (video grid + player + search, dummy data) — see `VIBE_CODE_BRIEF.md`.
5. Build MVP backend (auth, video upload endpoint, PostgreSQL schema) — see `VIBE_CODE_BRIEF.md`.
6. Engage a lawyer for GDPR/ToS/moderation-policy review — start in parallel, not after building.
7. Draft pitch materials for early investor/crowdfunding conversations.
8. Set up community channels and publish the governance model draft.
9. Begin EUIPO trademark search for "MundiVeo" alongside the domains already secured.
