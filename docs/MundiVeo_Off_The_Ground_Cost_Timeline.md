# MundiVeo — "Off The Ground" Cost Timeline (Draft v1)

*Scope: everything from today through a working private beta. Ad-revenue modeling, creator payouts, and Stage 2+ growth costs are intentionally out of scope here — see the separate hosting cost model for the growth-stage numbers. Figures below are sourced from published 2026 pricing where noted; ranges reflect real variation in how much you do yourself vs. pay a professional for.*

---

## Already spent

| Item | Cost | Status |
|---|---|---|
| `mundiveo.com` + `mundiveo.eu` domain registration | €12.09 | ✅ Paid, logged in public `EXPENSES.md` |

---

## Phase 1 — Legal & entity foundation (before or during beta build)

This is the phase your instinct correctly flagged as "not obviously necessary yet" — and for a pure hobby project, it wouldn't be. But three things about MundiVeo specifically make it worth doing earlier rather than later: it will handle other people's data (GDPR-by-design is a stated pillar), it will eventually move real money (ad revenue → creator payouts), and there's already a live trademark conflict with an existing @MUNDIVEO channel.

| Item | Low estimate | Higher/safer estimate | Notes |
|---|---|---|---|
| **Business entity — ZZP (sole proprietorship)** | €85 (KVK registration fee) | — | Fastest, cheapest route for a solo founder pre-revenue. No notary needed. Downside: no liability separation between you and the business. |
| **Business entity — BV (private limited)**, *if chosen instead* | €600 (online notary, budget package) | €2,500–3,500 (full-service formation, notary + KVK + tax registration) | Gives limited liability — relevant once ad money and creator payouts flow through the platform. This is a real decision point, not a formality — worth a real conversation with a Dutch accountant/notary about ZZP-now-BV-later vs. BV-from-day-one, since switching later has its own costs. |
| **Trademark clearance search** (before filing — non-negotiable given the @MUNDIVEO conflict already found) | €0 (DIY search via free EUIPO TMview + national registers) | €300–600 (professional search covering phonetic/visual similarity) | Given you already know of one conflicting channel, I'd treat the low-end DIY option as higher-risk than usual here. |
| **EUIPO trademark filing (EUTM, 1 class)** | €850 (official fee only, self-filed) | €1,450–2,350 (fee + attorney to draft/file, €600–1,500) | Covers all 27 EU member states with one filing. If the clearance search turns up a real conflict, budget an extra €320 opposition-response fee plus attorney time — impossible to size until the search is done. |
| **Terms of Service + Privacy Policy + Data Processing basics** | €0–300 (template-based, e.g. generator services, light self-drafting) | €1,500–4,000 (proper GDPR-savvy legal drafting/review) | This is the line item I'd be most cautious about underspending on, given "GDPR-by-design" is a named pillar of the project — a templated privacy policy written for a generic SaaS product won't actually cover video hosting, EU minors potentially using the platform, or ad-tech data flows correctly. |
| **Accounting setup (if BV)** | €0 (DIY bookkeeping pre-revenue) | €300–800 (first-year accountant for annual filing) | Only relevant once the BV exists; a ZZP has much lighter admin. |

**Phase 1 subtotal:** roughly **€935 (bare-minimum, ZZP + DIY legal + self-filed trademark)** to **€10,000+ (BV + full legal review + attorney-assisted trademark)**. Most realistic middle path for a solo founder who wants real protection without full-service legal fees: **€2,500–4,000**.

---

## Phase 2 — Building & privately testing the MVP

| Item | Estimate | Notes |
|---|---|---|
| Hosting (pilot-scale, per the earlier model) | €10–150/month | Genuinely trivial at this stage — see the hosting cost model for the per-provider breakdown. Budget for maybe 3–6 months before public beta: **€30–900 total**. |
| Dev tooling (Mistral Vibe Code usage, any paid CI/testing tools) | Unknown — needs checking with the dev-focused chat | Flagging rather than guessing: I don't have Vibe Code's pricing model in front of me, and it may be free/subscription/usage-based. Worth a quick check before this budget line is finalized. |
| Payment infrastructure setup (Stripe/Mollie/Wise for future creator payouts, business bank account) | €0 upfront typically, but a business bank account usually requires the entity (KVK number) to exist first | Sequencing matters: entity → bank account → payment processor, in that order. |
| Misc buffer (testing devices, minor SaaS subscriptions, form tools beyond Tally's free tier) | €100–500 | Rough contingency, not itemized. |

**Phase 2 subtotal:** roughly **€130–1,900**, heavily dependent on beta length and whether Vibe Code has a paid tier.

---

## What it takes to get MundiVeo "off the ground" (through a working private beta)

| Scenario | Total (excl. already-spent €12) |
|---|---|
| **Bare minimum** — ZZP, DIY legal templates, self-filed trademark, short beta | **~€1,100–2,500** |
| **Realistic middle path** — ZZP or lean BV, real legal review for ToS/Privacy, professional trademark search + self-filed | **~€3,500–5,500** |
| **Fully de-risked** — BV from day one, attorney-drafted legal docs, attorney-assisted trademark filing | **~€10,000–14,000** |

I'd treat **€3,000–5,000** as the realistic number to actually plan around for a solo founder taking this seriously but not over-lawyering it — it buys a defensible entity, GDPR-appropriate legal documents (which matter a lot given the project's core positioning), and a trademark filing that's been properly cleared first rather than filed blind into a known conflict.

---

## What I'm explicitly not able to size yet

- **Whether the trademark conflict forces a name change** — if the clearance search finds a blocking prior right (not just the @MUNDIVEO channel, which may or may not itself hold a registered mark), the honest range above could be moot and you'd be re-costing branding, domains, and the YouTube channel identity instead. Worth doing the EUIPO/TMview check *before* committing money to Phase 1's trademark line.
- **Mistral Vibe Code's actual pricing** — flagged above, needs a quick check.
- **Any marketing spend** — deliberately excluded; no ad-budget decision has been made yet, and organic (the YouTube channel) may cover creator recruitment without a paid budget at this stage.
- **Legal costs specific to the ad-revenue/creator-payout structure** (e.g., contracts for creators, tax withholding logistics) — these belong once that model is finalized in the marketing/technical chat, not before.

---

*This is a planning estimate, not a quote from any lawyer, notary, or accountant. Given this document may eventually inform real spending decisions (and per the project's transparency ethos, possibly a public EXPENSES.md entry), I'd treat every "professional service" line above as worth a real quote before committing, not just this estimate.*
