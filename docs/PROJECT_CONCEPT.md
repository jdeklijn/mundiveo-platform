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

## 3. Revised Roadmap — Deadline-Anchored (Alpha: 1 February 2027)

### 3.1 Why this specific date

On 10 August 2026, YouTube announced that from **1 February 2027**, new applicants to the YouTube Partner Program will need **8,000 qualified watch hours** (up from 4,000) or **20 million qualified Shorts views per 90 days** (up from 10 million) to start monetizing. The 1,000-subscriber requirement is unchanged.

**Important nuance, for accuracy:** this change affects *new* applicants, not creators already in the program — existing monetized channels keep their status. The practical effect is a much wider pool of **not-yet-monetized and newly-starting European creators** who will find the bar to entry on YouTube has doubled overnight. That is the audience MundiVeo's alpha is timed to reach: not creators being kicked off YouTube, but creators for whom YouTube's door is about to get a lot harder to open. (Existing partners also get a new ongoing Shorts threshold — 10M qualified views per 90 days — which can affect Shorts-heavy active channels too.)

**1 February 2027** is therefore not an arbitrary target — it's the day this pool of frustrated/locked-out creators becomes largest and most receptive to a credible alternative. The roadmap below works backward from that date.

### 3.2 Month-by-month plan (~5 months, September 2026 → February 2027)

| Timeframe | Focus | Key deliverables | Runs in parallel? |
|---|---|---|---|
| **Sep 2026** (now) | Design finalized, MVP build kicks off | FD/TD/GD/Branding docs (done), wireframes, Vibe Code build starts (frontend + backend scaffolding) | Legal & Finance chats already spun up this month |
| **Oct 2026** | Core MVP build | Upload, playback, search working end-to-end on EU hosting | Legal: entity choice (ZZP vs BV), EUIPO trademark clearance search. Finance: hosting cost model, provider comparison |
| **Nov 2026** | Recommendations v1 + monetization scaffolding | Tag-based/trending cold-start recommendations; monetization schema + stubbed UI (no live ad sales yet) | Legal: ToS/Privacy Policy first draft (GDPR-reviewed). Finance: runway/budget model finalized |
| **Dec 2026** | Private/internal testing | Bug fixing, security review, small invite-only test group (former colleagues/students) | Legal: ToS/Privacy Policy finalized and published. Trademark filing decision made |
| **Jan 2027** | Alpha readiness | Final polish, first real creator onboarding (targeting creators approaching the old 4,000-hour bar who won't make 8,000), content moderation basics in place | Legal sign-off deadline. Marketing push timed to public awareness of the YPP change |
| **1 Feb 2027** | **Alpha launch** | Live, invite-limited alpha — not a full public launch, but real creators uploading real content on real EU infrastructure | Coincides deliberately with YouTube's new threshold taking effect |

**What "alpha" means here, to be explicit:** a working, legally-covered (ToS/Privacy live), EU-hosted platform open to an initial cohort of creators — not a marketing-scale public launch. Decentralized storage, full ad-sales tooling, and the 18+ governance question remain out of scope for this milestone, per Section 2 above.

### 3.3 What has to be true by 1 Feb 2027 for this to hold

- ToS + Privacy Policy must be live (not just drafted) before any real user/creator data is collected — this is the hardest legal dependency and the one most likely to slip.
- Trademark situation (the existing @MUNDIVEO conflict) needs resolution or a mitigation plan well before a public-facing push — a forced name change discovered *in* January would be the single biggest risk to this timeline.
- Hosting cost model needs to be validated with real numbers, not estimates, before onboarding real creators whose videos cost real money to store and serve.
- The technical build itself is the *least* likely part to slip, based on the 1–2 month AI-assisted estimate for a working MVP — legal and trademark resolution are the actual schedule risks.

**Key takeaway:** the 5-month window is workable if legal/trademark work starts now and runs fully in parallel with the build, exactly as already set up via the dedicated Legal and Finance chats — but it leaves little slack. Any slippage on the trademark or ToS/Privacy front is the most likely thing to push the 1 February date.

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