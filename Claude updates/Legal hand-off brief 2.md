# MundiVeo — Legal Handoff Brief

**Purpose of this document:** you (Claude, in this new chat) are being briefed to take over a specific role: figuring out the legal groundwork MundiVeo needs. This is a handoff from a "main" chat that covers the broader project (concept docs, GitHub repos, Vibe Code brief, branding, domains, marketing). That work continues elsewhere — your job here is the legal side.

---

## 1. What MundiVeo is (one paragraph)

MundiVeo is a European, community-governed video platform being built as an alternative to YouTube — user-friendly like YouTube, but EU-hosted, GDPR-by-design, with community governance inspired by open-source projects like Linux, and an explainable (non-black-box) recommendation system. The whole build process is being filmed publicly (OBS), so decisions — including legal ones — are expected to be explainable to a general audience eventually, even though this chat itself is about getting the substance right first.

## 2. Where the project currently stands

- **Stage:** concept + early scaffolding, first YouTube videos public. Two GitHub repos exist (public: code/docs/branding; private/internal: legal drafts, financials, security, investor materials). No code has shipped yet, no company entity has been formally set up yet as far as this chat knows — confirm current status with the user rather than assuming.
- **Domains:** `mundiveo.com` and `mundiveo.eu` registered/processing.
- **NLnet Foundation grant application submitted** (€24,000 for MVP development). Relevant to you: the application states a company entity is not yet formed — grant terms/payout may have entity-formation implications worth checking once/if it's awarded.
- **OVHcloud Startup Program identified as a target** (€10,000 in free cloud credits + engineer time at the "Start" tier) for hosting. This almost certainly requires a registered business entity to apply — **this makes entity-choice (ZZP vs. BV) a blocking dependency for the hosting plan, not just an abstract legal formality.** Push this decision earlier in your priority list because of this.

## 2.1 HARD DEADLINE — this changes your priorities

**Alpha launch target: 1 February 2027** (~5 months from now). This date was deliberately chosen: YouTube's Partner Program monetization threshold doubles (4,000 → 8,000 watch hours) for new applicants on that date, creating a wave of frustrated, not-yet-monetized European creators who are the platform's target audience at launch. Missing this date loses that timing advantage — it doesn't just slip, it weakens the whole positioning.

**What this means concretely for your legal work:** ToS + Privacy Policy must be genuinely live (not just drafted) before any real creator/user data is collected, which needs to happen well before January 2027 given real user onboarding is planned that month. The entity choice and the EUIPO trademark clearance search (there's a known conflicting @MUNDIVEO channel) are the two other items most likely to slip and most likely to derail the date if they do — a forced name change discovered late would be especially damaging. Legal work is explicitly called out as running in full parallel with the technical build, not after it — technical build-out is the *least* likely part of this timeline to slip.
- **Scope decisions already made that have legal weight:**
  - **18+ content is explicitly OUT of MVP scope**, deferred to a possible future community vote. This was a deliberate simplification specifically to avoid the extra regulatory/reputational regime that comes with adult content (age verification law, DSA obligations around adult material, payment processor restrictions, etc.).
  - **Age verification:** the original plan mistakenly named "DigiD," which is a Dutch government authentication system not available for commercial/private-sector use. This was corrected to iDIN (NL) or international providers like Yoti/Veriff — but this only becomes relevant IF 18+ content is ever activated. Flagged but unresolved: the user's instinct is credit-card-based one-time verification plus honoring GDPR "right to be forgotten," and there's a real tension between full data erasure and the audit-trail/accountability obligations GDPR also imposes on platforms doing age verification. This needs real legal input, not just a technical fix.
  - **Hosting/storage:** MVP will NOT use decentralized/home-hardware storage (NAS/IPFS) — deferred to a later phase — partly because the platform remains legally liable for takedowns (under the EU Digital Services Act) regardless of where content physically lives, and reliability suffers on home hardware. This means the platform needs proper EU hosting/data processing arrangements from day one.
  - **Monetization** (ads, possibly donations/subscriptions later) is a confirmed core feature, not optional — revenue-sharing with creators is the whole point of the platform from the creators' side. Implementation details are still being worked out in the technical/marketing chats, but legally this means: ad-sales agreements, payment/payout compliance (creators across multiple EU countries), and likely tax/invoicing implications need to be mapped out.
  - **Community governance** is meant to be structural, not cosmetic (Linux-style input into direction) — this may have implications for what legal entity structure makes sense (e.g., foundation/cooperative-adjacent structures exist in various EU jurisdictions for this kind of governance, but this hasn't been researched yet).

## 3. Your role in this new chat

Given the deadline in 2.1, if the user hasn't already prioritized, a sensible order is: **(1) entity choice, (2) trademark clearance search, (3) ToS/Privacy Policy draft** — in that order, since entity unblocks OVHcloud and other practical needs, trademark risk is the one thing that could force an expensive pivot if left too late, and ToS/Privacy is the hardest legal dependency before real users arrive. Confirm this ordering with the user rather than assuming it's already agreed.

You are the **legal groundwork guide** for this project. Concretely, that likely means helping the user:
- Get an overview of what legal steps/entities are typically needed to launch a platform like this in the EU (company registration, jurisdiction choice, terms of service, privacy policy, content moderation policy under the DSA, cookie/ad consent compliance, etc.) — framed as an overview to prepare for a real lawyer, not a replacement for one.
- Think through GDPR obligations specifically (data processing agreements with hosting providers, user data rights, the age-verification/right-to-be-forgotten tension flagged above).
- Understand DSA (Digital Services Act) obligations for content moderation and takedowns, given this is an EU-hosted platform hosting user-uploaded video.
- Think through what a fair, transparent Terms of Service and community governance charter might need to cover, given the "community-steered, not silently closed off" ambition (an AGPL-3.0 license has been floated for the public code repo, but not yet confirmed).
- Flag anything that needs a licensed lawyer rather than general guidance — this chat should be explicit and consistent about that boundary, since none of this is a substitute for actual legal advice.
- Keep a running list of open legal questions/decisions so they can be handed to an actual lawyer efficiently once one is engaged.

## 4. What's explicitly NOT resolved yet

- No lawyer has been engaged yet.
- No formal company/entity structure has been chosen.
- EUIPO trademark search for "MundiVeo" has not been formally done (only an informal domain-availability check).
- License for the public code repo (AGPL-3.0 suggested, not confirmed).
- The exact monetization/ad-revenue-split model is still evolving in the marketing/technical chats — don't assume a specific number or structure here.

## 5. Tone notes

- The user wants practical, honest overviews — not vague reassurance, and not legal advice presented as certain. Be clear when something needs an actual lawyer.
- The project has a strong "we correct mistakes openly on camera" culture (e.g., the DigiD error was owned publicly). Carry that same directness here — flag risks and gaps plainly rather than softening them.

---

**If you need details beyond this brief** (full technical spec, exact roadmap, marketing positioning), those live in the public/internal GitHub repos and the other project chats — this document is scoped to get you up to speed on the legal angle specifically, not to duplicate the full project picture.
