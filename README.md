# Seven Levels, Round 2: Pink Cybertruck

The same website built seven times, each level one technique better — and the whole ladder run **twice with identical prompts**: once on **Claude Fable 5**, once on **Claude Opus 4.8**. Round 1 sold a yellow iPhone ([repo](https://github.com/chizhangucb/claude-code-seven-levels-building-website)); this round sells a **pink Cybertruck**. Framework and prompts adapted from Jack Roberts' ["7 Levels of Building Claude Code Websites"](https://www.youtube.com/watch?v=AFRL9dtUHeI).

> **Status: run complete (2026-07-12/13 UTC).** All 14 level-sessions were driven headlessly by one orchestrator session (`claude -p`, one fresh session per level per model), verified, checksummed, and published.
>
> **Live site: https://claude-code-seven-levels-pink-cyber.vercel.app** — every level's writeup, the exact prompts, both models' demos side by side ([compare](https://claude-code-seven-levels-pink-cyber.vercel.app/compare)), per-run metrics, and the [talk deck](https://claude-code-seven-levels-pink-cyber.vercel.app/slides).
>
> Evidence: [demos/](demos/) (byte-identical copies, SHA-256s in [demos/CHECKSUMS](demos/CHECKSUMS)) · [docs/run-log.md](docs/run-log.md) (session IDs, timestamps, incidents) · [docs/prompt-log/](docs/prompt-log/) (what was sent and what happened, per level) · [process-artifacts/](process-artifacts/) (Firecrawl scrapes, both level-7 extraction kits, raw headless JSON results).
>
> Headline: both models independently titled their level-1 pages the identical "PINKTRUCK — The Cybertruck, but Pink"; by level 7 both had converged on ~22 KB lowercase teenage.engineering-voice pages. Fable 5: $37.41 / ~43 min machine time across the ladder; Opus 4.8: $24.62 / ~50 min.

## The ladder

| Level | Technique | External tool |
|---|---|---|
| 1 | Grab and go | — |
| 2 | Screenshots & references | godly.website / Land-book / Awwwards |
| 3 | Design skills | power-design + ui-ux-pro-max |
| 4 | UI snipping | DottedSurface — three.js (21st.dev-style) |
| 5 | AI-generated video | OpenArt (Nano Banana → Kling/Seedance) |
| 6 | Data & brand research | Firecrawl → tesla.com |
| 7 | Design extraction blueprint | design-language skill → teenage.engineering |

(Levels 4 and 5 are intentionally swapped vs the source video — carried over from round 1.)

## What's here

- [docs/runbook.md](docs/runbook.md) — the master run-day procedure: preflight, setup, session plan, per-level loop, post-run, failure recoveries.
- [docs/orchestrated-run.md](docs/orchestrated-run.md) — hands-off mode: one session drives all 14 level-sessions headlessly via `claude -p` (mechanics verified), then builds and deploys everything below.
- [docs/publish-plan.md](docs/publish-plan.md) — post-run tutorial site (adapted from round 1's proven code), metrics, and Vercel deployment plan.
- [docs/talk-skeleton.md](docs/talk-skeleton.md) — the 60-min talk structure with ⟨TBD⟩ slots the orchestrator fills from actual results.
- [docs/prompts/](docs/prompts/) — one file per level: the exact paste-ready prompt, scripted follow-ups, and what to watch for (with round-1 notes).
- [docs/asset-checklist.md](docs/asset-checklist.md) — the five assets to prepare before run day.
- [docs/run-log-template.md](docs/run-log-template.md) — filled in live during the run (session IDs, timestamps, checksums).
- [skill/design-language/](skill/design-language/) — the level-7 extraction skill, byte-identical to round 1 (checksums in the runbook).
- `references/` — where the run assets land (gitignored trial dirs `fable-trials/` and `opus-trials/` are created on run day).

## The framework

This round was produced by running a generalized, product-agnostic version of the experiment — the prompt pack, runbook, orchestration harness, and metrics methodology live at [seven-levels-framework](https://github.com/chizhangucb/seven-levels-framework). Pick a product, fill the placeholders, run your own round.

## Credit

Framework and original prompts from Jack Roberts' video. Skills: [power-design](https://github.com/ItsssssJack/power-design), [ui-ux-pro-max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill). Tools: [Firecrawl](https://firecrawl.dev), [OpenArt](https://openart.ai), [21st.dev](https://21st.dev), [three.js](https://threejs.org).
