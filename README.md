# Seven Levels, Round 2: Pink Cybertruck

The same website built seven times, each level one technique better — and the whole ladder run **twice with identical prompts**: once on **Claude Fable 5**, once on **Claude Opus 4.8**. Every level's output is preserved verbatim. Round 1 sold a [yellow iPhone](https://github.com/chizhangucb/seven-levels-round1-yellow-iphone); this round sells a **pink Cybertruck** — and new this round, **the experiment ran itself**: one orchestrator session drove all 14 level-sessions headlessly (`claude -p`), verified and checksummed each level, then built the site and deck below.

**Live site: https://seven-levels-round2-pink-cyber.vercel.app** — every level's writeup with the exact prompts, all 14 demos, a Fable-vs-Opus [compare view](https://seven-levels-round2-pink-cyber.vercel.app/compare) with synced scrolling and per-run metrics, and the [talk deck](https://seven-levels-round2-pink-cyber.vercel.app/slides).

## The ladder

| Level | Technique | Fable 5 | Opus 4.8 |
|---|---|---|---|
| 1 | Grab and go — a bare prompt | [demo](demos/fable/level-1.html) | [demo](demos/opus/level-1.html) |
| 2 | Screenshots & references | [demo](demos/fable/level-2.html) | [demo](demos/opus/level-2.html) |
| 3 | Design skills (power-design, ui-ux-pro-max) | [demo](demos/fable/level-3.html) | [demo](demos/opus/level-3.html) |
| 4 | UI snipping (DottedSurface — three.js) | [demo](demos/fable/level-4.html) | [demo](demos/opus/level-4.html) |
| 5 | AI-generated video (OpenArt) | [demo](demos/fable/level-5.html) | [demo](demos/opus/level-5.html) |
| 6 | Data & brand research (Firecrawl → tesla.com) | [demo](demos/fable/level-6.html) | [demo](demos/opus/level-6.html) |
| 7 | Design extraction (teenage.engineering) | [demo](demos/fable/level-7.html) | [demo](demos/opus/level-7.html) |

*(Levels 4 and 5 are swapped versus the source video — carried over from round 1.)*

## What's in the repo

- **`demos/`** — the 14 level HTML files, byte-for-byte as the sessions produced them (`demos/CHECKSUMS` has SHA-256 of each), plus the OpenArt-generated product film.
- **`levels/`, `compare/`, `index.html`** — the tutorial site: one page per level (technique, exact prompts, live demo with model toggle) and a side-by-side compare view with proportional sync-scroll and a full per-run metrics table.
- **`docs/prompts/`** — the fixed, paste-ready prompt pack (typos intentional). **`docs/prompt-log/`** — what was actually sent and what happened, per level. **`docs/run-log.md`** — session IDs, timestamps, checksums, incidents.
- **`docs/runbook.md`** + **`docs/orchestrated-run.md`** — the run-day procedure and the headless orchestration mode this round was executed in.
- **`docs/talk-skeleton.md`** + **`slides/`** — a 60-minute talk: filled speaker notes and a self-contained HTML deck.
- **`skill/design-language/`** — the level-7 design-extraction skill, byte-identical to round 1.
- **`process-artifacts/`** — working evidence: raw Firecrawl scrapes of tesla.com, both models' teenage.engineering extraction kits, and the raw headless-session JSON results.

## Run locally

```bash
python3 -m http.server 8765
# open http://localhost:8765
```

No build step — everything is static HTML.

## Highlights worth reading

- **Convergence at level 1** ([docs/prompt-log/level-1.md](docs/prompt-log/level-1.md)): both models independently titled their pages the identical "PINKTRUCK — The Cybertruck, but Pink".
- **Opus debugs the component it was handed** ([docs/prompt-log/level-4.md](docs/prompt-log/level-4.md)): it caught the source's 0–255 vertex-color bug — the round-2 twin of round 1's memory-leak find.
- **The overwrite didn't recur** ([docs/prompt-log/level-2.md](docs/prompt-log/level-2.md)): round 1's level-6 incident was designed out (keep-untouched up front + one session per dir), and the checksums prove it.
- **Brand persistence** ([docs/prompt-log/level-7.md](docs/prompt-log/level-7.md)): Opus reused the "CYBR" brand it invented at level 3; both models converged on ~22 KB lowercase teenage.engineering-voice pages.
- **The bill**: Fable 5 $37.41 / ~43 min machine time across the ladder; Opus 4.8 $24.62 / ~50 min — ~2.7× cheaper than round 1's interactive run.

## The framework

This round was produced by running a generalized, product-agnostic version of the experiment — the prompt pack, runbook, orchestration harness, and metrics methodology live at [seven-levels-framework](https://github.com/chizhangucb/seven-levels-framework). Pick a product, fill the placeholders, run your own round.

## Credit

The seven-level framework and original prompts are from [Jack Roberts' video](https://www.youtube.com/watch?v=AFRL9dtUHeI). Skills used: [power-design](https://github.com/ItsssssJack/power-design), [ui-ux-pro-max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill). Tools: [Firecrawl](https://firecrawl.dev), [OpenArt](https://openart.ai), [21st.dev](https://21st.dev), [three.js](https://threejs.org).
