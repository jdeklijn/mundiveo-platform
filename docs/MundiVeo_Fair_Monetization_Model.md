# MundiVeo — Fair Monetization Model: Platform and Creator Ad Inventory (Draft v1)

**Status:** concept draft, originated from founder brainstorming (walking-idea session, September 2026). Not yet cross-checked against ad-fill-rate realities, advertiser demand assumptions, or legal/tax treatment of creator payouts. Intended as input to `TECHNICAL_DESIGN.md` Section on monetization and `FUNCTIONAL_DESIGN.md` creator upload flow, not a replacement for either.

---

## 1. Core Idea

Every video on MundiVeo has two independent pools of ad inventory, kept structurally separate so that the platform has guaranteed, sellable ad inventory on every video regardless of what any individual creator chooses to do, while creators retain real, transparent control and revenue over their own portion.

### 1.1 Platform inventory
- A fixed minimum number of ad slots exists on every video, always present, not configurable by the creator.
- Revenue from these slots goes to MundiVeo directly. This is what the platform actually sells to advertisers/businesses as guaranteed inventory — the answer to "how does MundiVeo make money" independent of creator behavior.
- Exact slot count (originally floated as two pre-roll slots) is a placeholder, not a fixed decision. Likely needs to scale with video length and be pressure-tested against real ad-fill-rate and viewer-experience data before being finalized. Principle is fixed and separate from the number.

### 1.2 Creator inventory
- Creators choose the number and placement of their own ad slots, within a bounded range (floated as one to three, scaled to video length).
- Revenue from these slots goes to the creator.
- This is the direct answer to one of MundiVeo's founding frustrations with YouTube: creators get real, visible agency over their own monetization rather than an opaque platform-determined split.

**Why two separate pools, not one shared pool with a revenue split:** this makes the split legible and explainable to creators and viewers alike, in keeping with MundiVeo's transparency principle — "these slots are the platform's, these slots are yours, you chose where yours go" is a fundamentally different and more trustworthy relationship than an opaque percentage split decided by an algorithm.

---

## 2. Interaction with Direct Creator Sponsorships

A creator may have their own direct sponsorship deal for a specific video (e.g., a paid mention: "this video is sponsored by X"), entirely outside MundiVeo's ad system. This must not be stackable with platform-sold ad revenue on the same video — a creator should not be paid both directly by a sponsor and by MundiVeo's platform ad system for the same piece of content.

### 2.1 Mechanism: self-declared checkbox
- At upload, the creator marks a checkbox declaring whether the video has a direct sponsor.
- If checked, the video's platform-inventory ad slots are not monetized for that creator (exact mechanic — e.g., whether platform slots are simply removed, or remain but revenue is withheld — still to be decided).
- This is an honesty-based system by design, not a heavily policed one — backed by a penalty severe enough that lying is not a rational choice (see Section 3).

### 2.2 Rationale for why creators are unlikely to cheat
- Direct sponsorship deals typically pay more than programmatic platform ad revenue on an equivalent video, so a creator lying to stack both revenue streams is usually risking a larger guaranteed payment (the sponsorship) for a smaller potential gain (platform ad revenue) — a poor trade even before enforcement risk is considered.
- Enforcement risk (Section 3) is modeled deliberately on YouTube's strike system, a penalty structure creators already understand, rather than inventing an unfamiliar framework.

---

## 3. Detection and Enforcement

### 3.1 Detection: transcript-based flagging
- Every video uploaded to the platform is transcribed (this is infrastructure MundiVeo likely needs anyway, for search, accessibility, and captions — sponsorship detection is a secondary use of the same pipeline, not a bespoke system).
- The transcript is scanned for sponsor-indicating language (e.g., phrases like "this video is sponsored by").
- If such language is detected on a video where the creator has marked "no sponsor," the video is flagged for review rather than auto-penalized — language detection is imperfect, and false positives (an offhand phrase, a joke, discussion about sponsorship as a topic) should not automatically trigger punishment.
- Flagged videos presumably route to human moderator review before any enforcement action. (Exact review workflow not yet designed — belongs alongside the Moderator role definition in `FUNCTIONAL_DESIGN.md`.)

### 3.2 Enforcement: strikes system
- Confirmed double-dipping (undeclared sponsorship plus platform ad monetization on the same video) results in demonetization of that video and a strike against the creator's channel, modeled on YouTube's three-strikes approach.
- Three strikes results in removal of the channel from the platform entirely.
- The penalty is deliberately severe relative to the likely financial upside of cheating (see 2.2), so the incentive structure itself discourages the behavior — enforcement is a backstop, not the primary control.

---

## 4A. Second, Separate Idea: Yearly Creator Profit-Sharing Pool

**Status:** later-stage concept, distinct from the per-video ad-slot model above. Not to be confused or merged with Sections 1–3 — this is an additional, on-top mechanism, not a replacement for per-video creator ad revenue. Originated from the same founder brainstorming session (September 2026).

### 4A.1 Core idea

On top of whatever a creator already earns per video from their own ad slots (Section 1.2), MundiVeo shares a portion of the *platform's own net profit* each year with the creators who contributed to that success — proportional to their contribution. The logic: if MundiVeo does well as a whole, the creators who helped make that happen should share in it, not just the platform. This is a genuine point of difference from YouTube, where the platform's overall success doesn't flow back to creators beyond their own individual video performance.

### 4A.2 Illustrative mechanism (worked example, not a committed formula)

1. At year end, the platform calculates net profit after costs and reserved R&D/reinvestment budget — e.g., an illustrative €100,000 net profit in a year with €1,000 active creators.
2. A pool (some or all of that net profit) is divided across **qualifying** videos to produce a per-video bonus rate — e.g., €100,000 ÷ 200,000 qualifying videos = €0.50 per video.
3. Each creator's bonus = their number of qualifying videos × that per-video rate — e.g., a creator with 300 qualifying videos gets 300 × €0.50; a creator with 50 qualifying videos gets 50 × €0.50.

### 4A.3 Qualification threshold — the key refinement

An early version of this idea used *raw video count* with no quality filter, which would have rewarded high-volume, low-effort uploads (e.g., many short low-quality clips) as much as genuinely valuable content. This was refined during discussion:

- The per-video ad-slot system (Sections 1–3) already naturally penalizes low-quality, low-view content, since it earns little or no ad impressions regardless.
- To keep the yearly pool consistent with that same principle, a video should need to clear a **minimum engagement threshold** (e.g., a minimum view count — exact number not yet decided) to count as "qualifying" for the pool at all.
- This way, the pool rewards videos that demonstrably added value to the platform, not simply upload volume, while still remaining simple enough for any creator to audit their own payout by hand.

### 4A.4 Explicitly open / not yet decided

- What percentage or portion of net profit (if not all of it) feeds the pool in a given year, versus what's reserved for reinvestment/R&D/runway.
- The exact qualification threshold (minimum views, watch time, or some other engagement measure).
- Whether the ratio basis should be pure qualifying-video count, or weighted by views/watch time/engagement rather than a flat per-video rate — flagged as a legitimate future refinement once real usage data exists to model against.
- How this interacts with the "MundiVeo Partner Program" as a named, formal concept (naming/branding of the program itself is still undecided — described here descriptively, not as a finalized program name).
- Legal, tax, and accounting treatment of a profit-sharing distribution to a large number of individual creators (likely a materially different mechanism than simple ad-revenue payout) — out of scope for this document, belongs with the broader monetization legal review already flagged in `PROJECT_CONCEPT.md` (Phase 5) and creator payout structure research pending in the finance/hosting chat.

---

## 4. Open Questions / Not Yet Decided

- **Exact platform-slot count and how it scales with video length.** The "two pre-roll slots" figure was an illustrative starting point, not a committed number. Needs to be balanced against viewer experience (e.g., a short video should not carry two mandatory pre-rolls plus creator-optional mid-rolls) and real advertiser demand/fill-rate data.
- **Exact creator-slot range and how it scales with video length** (floated as one to three).
- **Mechanics of what "platform slots not monetized for the creator" means technically** when a sponsorship checkbox is marked — are the slots removed from the video entirely, left in but revenue is not attributed to the creator, or something else?
- **Detection tooling specifics** — what phrase list/model is used, how false positives are minimized, what the moderator review workflow looks like, how appeals work.
- **Legal/tax treatment** of creator ad revenue and sponsorship income is out of scope for this document — belongs with the broader monetization legal review already flagged in `PROJECT_CONCEPT.md` (Phase 5, Legal Compliance) and the creator payout structure research still pending in the finance/hosting chat.
- **Interaction with the existing `monetization` database table** (`TECHNICAL_DESIGN.md` Section 3) — schema currently has simple boolean flags (`ad_revenue_enabled`, `donation_enabled`, `subscription_enabled`); this model implies a richer structure is needed (platform-slot config, creator-slot config, sponsorship-declared flag, strike count) once this moves from concept to implementation.

---

*This document captures a founder-originated concept at the brainstorming stage. It should be reviewed against advertiser demand modeling, ad-fill-rate benchmarks, and legal/tax input before being treated as a committed product decision.*
