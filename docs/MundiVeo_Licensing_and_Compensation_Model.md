# MundiVeo — Licensing and Compensation Model

**What this file is:** a plain explanation of how MundiVeo's AGPL-3.0+ license interacts with running an actual business, and who gets paid, from where, and why. Written to be referred back to rather than re-explained every time the question comes up — for the founder, for contributors, and for anyone doing due diligence (a grant funder, a future team member, a legal advisor).

**Status:** conceptual/explanatory document, current as of September 2026. The one open decision this document flags (Contributor License Agreements, Section 4) has not yet been made and should be resolved before meaningful outside code contributions begin.

---

## 1. What the AGPL-3.0+ License Actually Does

MundiVeo is licensed under **AGPL-3.0, "or later" (the "+")**.

- **AGPL closes the "SaaS loophole"** that plain GPL leaves open: under GPL, someone can run a modified version of the code as a hosted network service without ever having to share their modifications, because they never "distribute" the software in the traditional sense — users just connect to it over a network. AGPL specifically requires that anyone running a modified version *as a network service* must also make their modified source available to the users of that service.
- **"Or later" (+)** means the project also accepts the terms of any future version of the AGPL that the Free Software Foundation publishes, rather than being locked to exactly version 3.0 forever.
- **What AGPL does *not* do:** it does not stop anyone — including MundiVeo itself — from charging money, running ads, or building a real business around the software. It also does not stop a competitor from taking the code, standing up their own hosted version, and competing directly, *as long as they also release their own modifications under AGPL*. This is a deliberate trade-off: maximum openness and community trust, at the cost of the code itself never being a defensible moat on its own.

---

## 2. Where Business Revenue Actually Comes From (Since the Code Itself Is Free)

Because anyone can legally take the AGPL code for free, MundiVeo's business model cannot be "sell access to the code." Revenue instead comes from layers built *on top of* the open code:

| Revenue source | How it works for MundiVeo | Precedent |
|---|---|---|
| **The hosted service itself** | People use mundiveo.com/eu for the existing community, creators, and convenience — not because they couldn't self-host the code | This is MundiVeo's primary model: ad revenue (platform + creator inventory, see `MundiVeo_Fair_Monetization_Model.md`) and any future subscription/donation revenue |
| **Dual licensing** | Offering a non-AGPL commercial license to businesses who don't want AGPL's copyleft obligations, for a fee | Only available later if the project holds sufficient rights over the full codebase — see Section 4 (CLAs) |
| **Trademark control** | The *code* is free to fork; the *name* "MundiVeo" and its brand identity are separate IP the project controls regardless of license | A fork cannot legally call itself "MundiVeo" — one of the few durable moats available under this license model |
| **Support/consulting** | Paid help for businesses running their own instance | Less central to MundiVeo's current model, but a standard option in this space |

This mirrors how other AGPL/GPL-licensed platforms with real businesses behind them operate (e.g., GitLab, Mattermost, Nextcloud) — the license governs the code, not the business built around running and operating it.

---

## 3. Who Gets Paid — the Distinct Groups

"Who gets paid" is not one pool of money — it's several separate relationships, each with its own logic:

| Group | Paid from | Notes |
|---|---|---|
| **Founder** | Business revenue, once an entity and revenue exist | Running an open-source project does not limit the founder's own right to earn from operating the platform — the license governs the code, not the founder's business |
| **Paid team members / contractors** | Salary or contract fee, on normal terms | Their code still goes into the AGPL repository, but compensation is a separate employment/contract relationship, same as at any company building on open-source code |
| **NLnet-funded contributors** | Milestone-based grant payments (see finance/hosting chat for mechanics) | A distinct funding pool tied to specific deliverables — entirely separate from business revenue and from whether the business is profitable at all |
| **Volunteer / community code contributors** | Not paid, by default | Standard for most open-source projects — people contribute for reputation, scratching their own itch, ideological alignment with the project's mission, or hope of a future paid role. MundiVeo's existing recruitment framing ("no delivery, no pay / NLnet bonus not a promise") is deliberately built to attract exactly this kind of mission-driven contributor rather than payment-motivated ones |
| **Creators (video uploaders)** | Platform monetization model — ad slots, yearly profit-sharing pool | See `MundiVeo_Fair_Monetization_Model.md`. This is a completely separate relationship from code contribution — a creator and a code contributor are different roles, even when the same individual happens to be both |

---

## 4. The Open Decision That Matters Most: Contributor License Agreements (CLAs)

**This is a "decide deliberately now, not by default later" issue.**

If an outside contributor submits code with no CLA in place, **they personally retain copyright** on their contribution, even though it's released under the project's AGPL license. This has real consequences:

- The project can **never relicense** away from AGPL — for example, to offer dual licensing (Section 2) or to satisfy a future investor who wants full IP clarity — without tracking down and obtaining consent from *every* contributor who never signed anything. This becomes practically impossible once a project has dozens of contributors scattered over time.
- If dual licensing is ever wanted as a real future revenue option, a **CLA (or a DCO with copyright assignment)** needs to be in place *before* meaningful outside contributions start landing. Retrofitting this after the fact — going back to ask past contributors to sign something — is a well-known, often-failed exercise in open-source projects that didn't think about this early.
- The alternative is to **not** require a CLA, let contributors keep their own copyright, and accept that dual licensing is permanently off the table. This is a simpler, more purely "open" stance, and a legitimate choice — it just needs to be made knowingly rather than defaulted into.

**Recommendation:** decide this before onboarding contributors beyond the founder, since it is far easier to set up front than to fix retroactively. This decision should sit alongside the AGPL-3.0 licensing confirmation already flagged as a prerequisite for the NLnet grant application.

---

## 5. Summary

- AGPL-3.0+ protects the *openness* of the code — it does not prevent MundiVeo from being a real, revenue-generating business.
- Revenue comes from operating the service (ads, creator monetization) and from controlling the brand, not from restricting access to the code itself.
- Different groups of people (founder, paid staff, grant-funded contributors, volunteers, creators) are paid through entirely separate mechanisms, and conflating them is a common source of confusion worth avoiding in any public-facing explanation of the project.
- The CLA decision is the one piece of unfinished business that actually forecloses future options if left too long — it belongs on the near-term decision list, not the "someday" list.
