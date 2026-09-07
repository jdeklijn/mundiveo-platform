# MundiVeo — Collaboration & Tooling (Draft)

**Status: draft, pre-community**, same status as `GOVERNANCE.md`. This describes the tools intended to let a distributed, volunteer-based team — potentially spread across many countries — actually work together once contributors join. Nothing here is final; several items are explicitly flagged as open discussion points for when a real community forms, consistent with the project's principle of not over-deciding things before there's anyone to decide them with.

For the "why" behind tool choices in general, see the sovereignty/GDPR-by-design principles in `../PROJECT_CONCEPT.md`. For code hosting specifically, see `README.md` and `CONTRIBUTING.md`.

---

## 1. Code Hosting & Review Flow

**Decided:**
- **GitHub** — used as a staging/draft area. New work happens in a local working folder first, then goes to GitHub for outside visibility and review.
- **Codeberg** (https://codeberg.org/Mundiveo-platform/Mundiveo_platform) — the official, released repository. Only human-reviewed code is pushed here. This is the canonical source for anything considered "official" MundiVeo code.
- License: **AGPL-3.0**, chosen to close the network-service loophole that plain GPLv3 leaves open (see `README.md`).

**Open / not yet decided** (parked for actual community discussion, not to be forced now):
- What exactly counts as "reviewed" before something moves from GitHub to Codeberg — self-review, outside reviewers, or a more formal process once contributors exist.
- Whether the local working folder pushes to GitHub only (with a separate manual push to Codeberg after review), or to both remotes in some other arrangement.
- A known honest tension worth keeping in mind: this flow means GitHub (Microsoft) sees the earliest, most AI-assisted draft of any code — potentially before Codeberg does. That's an accepted trade-off for now, not an oversight.
- AI-assistance/code-review policy for contributors (relevant given Codeberg's July 2026 Terms of Use update on heavily-LLM-generated projects) — deferred to a future SSDLC/SCRUM process document once real coding and contributors begin.

---

## 2. Scrum / Project Management

**Starting point (lowest friction, nothing new to set up):**
- Codeberg's built-in Issues + Project Boards (Forgejo), for basic Kanban-style tracking. Likely sufficient for the earliest stage, before task ownership needs anything more structured.

**Planned once the team outgrows the basics:**
- **Taiga** — open-source, AGPL-3.0-or-later (same license philosophy as MundiVeo itself), self-hostable, purpose-built for Scrum and Kanban (backlog, sprints, burndown charts, epics). Chosen over generic task boards because it's an actual Scrum tool, not one adapted to look like one.
- **OpenProject** noted as a heavier, more enterprise-grade alternative (Gantt charts, resource planning) if the project ever scales past what Taiga comfortably handles — not needed at this stage.

**Open:** exact trigger point for standing up Taiga (self-hosted, presumably once OVHcloud hosting is active) instead of relying on Codeberg's boards. No fixed headcount or date attached to this yet.

---

## 3. Communication

**Decided:**
- **Proton Mail** — already in use for project email, replacing Gmail.
- **Jitsi Meet** — open-source, self-hostable video calls, no account required to join. Intended for community/team calls without pulling contributors into Zoom or Google Meet.

**Being evaluated:**
- **Proton Meet** — Proton's own end-to-end encrypted video calling product, launched in 2026. Philosophically consistent with the rest of the Proton-based stack; not yet committed to as the primary call tool pending a closer look at meeting-size limits and browser support.
- **Matrix / Element** — open-source, federated, self-hostable chat. The sovereignty-consistent choice for day-to-day team/community chat.
- **Discord** — flagged honestly as the tool most open-source volunteer communities actually gather on, due to low signup friction. US-owned, not GDPR-native, so a real values trade-off rather than a technical one. Not decided either way — noted here so the choice is made consciously rather than by default.

**Open:** whether the project uses Matrix/Element exclusively for a sovereignty-consistent story, or accepts the Discord trade-off for lower-friction community growth. This is a values decision as much as a tooling one, and worth surfacing to early contributors rather than deciding unilaterally.

---

## 4. Documents, Files & Shared Work

**Decided:**
- **Proton Drive** (Docs + Sheets) — replacing Google Drive/Docs/Sheets for internal working documents, in line with the project's move away from Google infrastructure. End-to-end encrypted, GDPR/NIS2/ISO 27001-aligned by default, which is a reasonable internal-tooling echo of the platform's own "GDPR by design" positioning.
- **Proton Workspace** — noted as Proton's bundled team offering (Mail, Drive, Docs, Sheets, Meet together); worth standardizing on this as the team plan rather than assembling individual Proton products piecemeal, if/when a paid team tier is needed.

**Being evaluated:**
- **HedgeDoc** — open-source, self-hostable collaborative markdown editor. Possible fit for fast-moving shared brainstorm notes, as an alternative or complement to Proton Docs.

**Explicitly not a fit:**
- Proton's suite is good for documents, storage, and calls — it is **not** a Scrum/sprint-tracking tool (no backlog, story points, or burndown charts). It sits alongside Taiga/Codeberg boards, not in place of them.

---

## 5. Summary Table

| Need | Decided | Being evaluated | Not decided |
|---|---|---|---|
| Code hosting | GitHub (staging) → Codeberg (official) | — | Review criteria; push workflow details |
| Sprint/task tracking | Codeberg boards (early stage) | Taiga (self-hosted, later) | Exact trigger to switch |
| Email | Proton Mail | — | — |
| Video calls | Jitsi Meet | Proton Meet | Primary tool long-term |
| Chat | — | Matrix/Element vs. Discord | Sovereignty vs. friction trade-off |
| Docs/files | Proton Drive/Docs/Sheets | HedgeDoc | — |

---

*This document will grow and firm up as real contributors join — consistent with `GOVERNANCE.md`'s approach of not writing rules before there's a community to apply them to.*
