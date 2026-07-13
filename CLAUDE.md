# CLAUDE.md — Handoff notes for the Pink Cybertruck seven-levels experiment

Context for future Claude Code sessions in this repo.

## What this repo is

Round 2 of the "7 Levels of Building Claude Code Websites" experiment (round 1: yellow iPhone, `~/experimental/design-yellow-iphone-web`, published at https://github.com/chizhangucb/design-seven-levels-round1-yellow-iphone). Same 7-level ladder, same Fable 5 vs Opus 4.8 A/B with identical prompts, new product: a **pink Cybertruck** selling site. Powers a 60-minute talk on leveling up website quality in Claude Code.

**Stage: designed, not yet run** (design completed 2026-07-12). The full procedure is in `docs/runbook.md`; the recommended hands-off mode (one orchestrator session driving all levels via headless `claude -p` — mechanics verified 2026-07-12) is in `docs/orchestrated-run.md`, which then chains into `docs/publish-plan.md` (tutorial site + metrics + Vercel deploy, adapting round 1's code) and `docs/talk-skeleton.md` (60-min talk, ⟨TBD⟩ slots filled from results). Run sessions must follow them. Production deploys and GitHub publishing require Chi's explicit go-ahead — preview deploys only by default. Do not "improve" the prompts in `docs/prompts/` — their casual phrasing (typos included) is verbatim-adapted from round 1 and is part of the experiment.

## Hard rules for run sessions

1. Name the output file **in the prompt** at every level (`pink-cybertruck-web-level-N.html`). Levels are immutable once produced.
2. "keep pink-cybertruck-web-level-(N-1).html untouched" goes **up front** in every level ≥ 2 prompt (round 1: Opus overwrote L1 at L2).
3. **Never two live sessions in the same trial dir** (round 1: a concurrent L7 session silently clobbered `level-6.html`). Fable and Opus run in parallel only because `fable-trials/` and `opus-trials/` are separate dirs.
4. Fresh session per level; log session ID + UTC start timestamp in `docs/run-log-template.md` **as you go** (round 1 had to reverse-engineer this from transcripts).
5. Control the skill variable: L3's prompt excludes the superpowers plugin; record globally installed skills/plugins at preflight.
6. Only scripted follow-ups (each level's "Ready follow-ups") — extra prompts contaminate metrics and the A/B.
7. `fable-trials/` and `opus-trials/` are gitignored raw evidence. Post-run, outputs are copied byte-identical to `demos/fable|opus/` with SHA-256s in `demos/CHECKSUMS`.

## Key design decisions

- Level order keeps round 1's **4/5 swap vs the source video**: 4 = UI snipping (`DottedSurface`, a 21st.dev-style three.js dot-wave, always on — swapped from round 1's balloons-js, disclosed deviation), 5 = AI video (OpenArt).
- L6 Firecrawl target: **tesla.com** (fallback `tesla.com/cybertruck`). L7 extraction target: **teenage.engineering** (fallback linear.app — see `docs/prompts/level-7.md` for the probe-verified reasoning).
- Both models get **identical L3/L4 inputs** this round (round 1 was asymmetric) — disclosed deviation, listed in runbook §1.
- `skill/design-language/` is a **verbatim copy** from round 1, checksums in runbook §2 — don't edit it; it must stay byte-identical for cross-round comparability.

## Metrics methodology (for post-run analysis)

Mine from `~/.claude/projects/-Users-chizhang-experimental-design-pink-cybertruck-web/*.jsonl` (post-run consolidation: the former `...-web-fable-trials` and `...-web-opus-trials` transcript dirs were merged into this one on 2026-07-13); session→level map comes from the run log.

- **Tokens:** dedupe assistant entries by `message.id` (streaming writes one JSONL line per content block, each repeating the same usage — summing raw lines double-counts).
- **Pricing:** API-equivalent at list price — round-1 values were Fable 5 $10/$50 per MTok in/out, Opus 4.8 $5/$25; cache read 0.1×, cache write 2× (1-hour TTL — check `usage.cache_creation.ephemeral_1h_input_tokens`, do NOT assume the 1.25× 5-minute rate). **Re-verify all pricing via the claude-api skill at analysis time; never from memory.**
- **Time = machine time only:** sum of (prompt → last assistant/tool_result event before next prompt); human think-time excluded. Gotcha: `queue-operation` entries carry the *next* prompt's timestamp — end intervals at the last assistant/tool_result event or times inflate past wall-clock.
- **Prompt counting:** exclude `isMeta`, `[Request interrupted...]` markers, and tool_result-only user entries; a re-sent prompt after an interrupt counts.
- **No total rows** in per-level metrics tables (Chi's preference).

## Chi's preferences

Concise but warm; no unrequested scope creep; flat repo layout (docs in `docs/`, no codename nesting); results over methods (always offer alternatives).
