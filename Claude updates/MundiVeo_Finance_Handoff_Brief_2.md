# MundiVeo — Finance & Hosting Cost Handoff Brief

**Purpose of this document:** you (Claude, in this new chat) are being briefed to take over a specific role: figuring out the real-world costs of building and running MundiVeo — especially server/hosting/infrastructure costs — and the broader financial picture. This is a handoff from a "main" chat that covers the broader project (concept docs, GitHub repos, branding, domains, legal, marketing). That work continues elsewhere — your job here is the numbers.

---

## 1. What MundiVeo is (one paragraph)

MundiVeo is a European, community-governed video platform being built as an alternative to YouTube — user-friendly like YouTube, but EU-hosted, GDPR-by-design, with community governance inspired by open-source projects like Linux, and an explainable (non-black-box) recommendation system. Video platforms are unusually expensive to run compared to most web apps (storage + bandwidth + transcoding at scale), so getting a realistic cost picture early matters both for planning and for talking to investors credibly.

## 2. Where the project currently stands (context for cost estimates)

- **Stage:** concept + early scaffolding, first YouTube videos public, no code shipped yet, no users yet. Two GitHub repos exist (public code/docs, private legal/financials).
- **NLnet Foundation grant application submitted**: €30,000 requested (100 days at €300/day), including an "Anti AI code" line item alongside core build tasks. Note: an earlier draft breakdown circulated at €24,000/80 days without that line item — €30,000/100 days is the correct, current figure; if a more detailed task breakdown is needed, confirm the final version with the user rather than assuming either draft. Outcome not yet known; treat this as pending, not secured funding.
- **OVHcloud Startup Program identified as a strong hosting option**: "Start" tier offers €10,000 in free cloud credits (12 months) + 6 hours of free 1-on-1 engineer time; credits apply to Public Cloud/Private Cloud infrastructure, which matches the project's planned object-storage + compute stack. The "Scale" tier (up to €100,000) is not realistic yet — it targets startups with existing Series-A-level traction and cloud spend. **Important dependency: this almost certainly requires a registered business entity to apply**, which is currently being worked out in the legal chat — factor this into any cost timeline (credits can't be assumed until the entity exists).
- **Confirmed architecture decisions with cost implications:**
  - **EU hosting required from day one** — not just for branding reasons but because decentralized/home-hardware storage (NAS/IPFS) was explicitly deferred to a later phase. This means real cloud/hosting provider costs (storage, bandwidth/CDN, compute for transcoding) apply from the MVP stage, not later.
  - **Video-specific cost drivers to account for:** raw video storage (per GB), video transcoding (multiple resolutions per upload — CPU/GPU cost), CDN/bandwidth for video delivery (this is usually the dominant cost for video platforms, not storage), database/backend hosting, and potentially costs tied to the recommendation system (embeddings/vector storage, compute for collaborative filtering).
  - **Monetization is a core planned feature** (ad revenue is the main creator-payout mechanism, matching YouTube's model but reportedly with a different sales approach being discussed in the marketing chat — details of that are intentionally not final yet, don't assume a specific number here). This means the financial model eventually needs a revenue side, not just a cost side, but cost/runway is the more urgent piece right now.
  - **Domains** cost €12,09 total (`mundiveo.com` + `mundiveo.eu`) — already logged publicly in the repo's `EXPENSES.md` as a transparency example; this is the only real spend logged so far.

## 2.1 HARD DEADLINE — this changes your priorities

**Alpha launch target: 1 February 2027** (~5 months from now). This date was chosen deliberately: YouTube's Partner Program monetization threshold doubles (4,000 → 8,000 watch hours) for new applicants that day, creating a wave of frustrated, not-yet-monetized creators who are the platform's target audience at launch — missing the date weakens that positioning, not just the schedule.

**What this means for your work specifically:** the hosting cost model needs to be validated with real numbers well before real creators are onboarded (planned for January 2027), not left as an estimate. The OVHcloud credit application should happen as soon as the entity exists (see the dependency above) rather than being left until later, since it directly affects the runway/budget picture you're building. If the NLnet grant outcome becomes known, that materially changes the runway calculation — worth checking in on its status periodically rather than assuming either outcome.

## 3. Your role in this new chat

You are the **finance & hosting cost guide** for this project. Concretely, that likely means helping the user:
- Build a realistic **per-GB-stored and per-active-user cost model** for video hosting, storage, bandwidth/CDN, and transcoding — comparing a few realistic EU-based hosting/cloud options (the project deliberately favors non-US-Big-Tech tooling elsewhere, e.g. Mistral Vibe Code for coding, so EU-based hosting providers may be worth prioritizing in comparisons, but cost-effectiveness should still be weighed honestly against US providers if there's a large gap).
- Estimate costs at different growth stages (e.g., small pilot/beta vs. thousands of active users vs. a real YouTube-scale ambition) so the user understands what the cost curve looks like, not just a single snapshot number.
- Think through **runway/burn-rate basics**: given no revenue yet, what does a monthly budget look like, and how long could early funding realistically last.
- Once a monetization structure is more final (from the marketing/technical chats), help sanity-check whether an ad-revenue model at various stages could plausibly cover hosting costs and creator payouts — but don't invent numbers for the ad model itself; that detail belongs to the other chats and isn't finalized.
- Flag anything that needs a real accountant, financial advisor, or EU grant/subsidy specialist rather than general estimation — European tech/startup grants and subsidies may genuinely be relevant here and worth researching.
- Keep the internal repo's `finance/README.md` checklist in mind as a place this work may eventually get logged (currently an empty placeholder, intentionally not pre-filled with fake content).

## 4. What's explicitly NOT resolved yet

- No hosting provider has been chosen.
- No formal budget or funding round has been set.
- The ad-revenue model (fixed-price vs. auction-style) is still being worked out in the marketing/technical chats — treat it as "a model is coming, not finalized" rather than guessing at numbers.
- No company entity/legal structure confirmed yet (relevant for things like grant eligibility) — that's being handled in the legal chat.

## 5. Tone notes

- The user wants concrete, realistic numbers where possible — cite real provider pricing/estimates rather than vague ranges when you can, and be upfront about assumptions when precise numbers aren't available.
- The project has a public transparency ethos (costs are logged in a public `EXPENSES.md`), so framing cost estimates in a way that could eventually be shared publicly (clear, honest, not inflated or downplayed) fits the project's character.

---

**If you need details beyond this brief** (full technical spec, exact roadmap, legal specifics, marketing positioning), those live in the public/internal GitHub repos and the other project chats — this document is scoped to get you up to speed on the finance/hosting angle specifically, not to duplicate the full project picture.
