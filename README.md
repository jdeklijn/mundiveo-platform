# MundiVeo

A European, community-governed video platform — built as an alternative to YouTube, under EU law by design, and documented in public from day one.

📺 This project is being built on camera. See the video series for the full story behind every decision.

## Status

🚧 **Concept / pre-MVP.** No production code yet. This repository exists to make the build process public from the start.

## What is this?

MundiVeo aims to combine:
- The ease of use people expect from YouTube
- EU-based hosting and GDPR-by-design data handling
- A semi-open governance model (community has real input, inspired by open-source projects like Linux)
- Transparent, explainable recommendations instead of a black-box algorithm
- Fairer monetization terms for creators

Full concept document: [`docs/PROJECT_CONCEPT.md`](docs/PROJECT_CONCEPT.md)

Build spec for AI-assisted development: [`VIBE_CODE_BRIEF.md`](VIBE_CODE_BRIEF.md)

## Repository structure

```
docs/           → Functional & technical design, roadmap, governance
frontend/       → React + TypeScript client
backend/        → Node.js + Express API
scripts/        → DB setup, transcoding helpers
```

## What's deliberately NOT in this version

To keep the first working version achievable and legally simple, the following are intentionally deferred — see `docs/PROJECT_CONCEPT.md` Section 3 for the reasoning:
- Adult (18+) content and age verification
- Decentralized/NAS-based storage
- Monetization payouts (schema exists, endpoints don't yet)

These may return via community governance decisions later — this isn't a permanent ban, just a sequencing choice.

## Contributing

This project doesn't have external contributors yet — it's just getting started. Once it does, contribution guidelines will live in `CONTRIBUTING.md`. Watching, starring, and opening discussion threads is welcome even now.

## License

See [`LICENSE`](LICENSE). *(Placeholder — license choice to be finalized; leaning toward a copyleft license such as AGPL-3.0 to keep the project and any derivatives open, but not yet finalized.)*
