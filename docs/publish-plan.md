# Publish plan — tutorial materials & deployment

Phase 2–3 of the orchestrated run: after the experiment completes and `demos/` is checksummed, build the tutorial package and deploy it. **Round 1's site is the template** — adapt, don't reinvent: `/Users/chizhang/experimental/design-yellow-iphone-web` has proven, static, no-build code for every piece below.

## 1. Tutorial site (adapt from round 1)

| Piece | Source in round-1 repo | Round-2 changes |
|---|---|---|
| `index.html` | `index.html` | Rebrand (pink Cybertruck, pink accent — check WCAG contrast), ladder links, round-1 cross-link |
| `levels/1-7.html` | `levels/*.html` | Per-level technique writeup, the exact prompts (from `docs/prompts/`), Fable/Opus demo toggle, metrics strip |
| `compare/index.html` | `compare/index.html` + sync-scroll logic in `assets/site.js` | Content only — keep the proportional sync-scroll code as-is |
| `slides/index.html` | `slides/index.html` | Rebuild content per `docs/talk-skeleton.md`; keep the deck engine (sections, `#/N` deep links, overview) |
| `assets/site.css/js` | `assets/` | Palette swap; logic mostly untouched |
| `assets/metrics.js` | `assets/metrics.js` | New data, same shape (below) |
| `assets/thumbs/` | — | Regenerate: 1280×800 Playwright screenshots, `{fable,opus}-{1..7}.png` |
| `docs/prompt-log/` | `docs/prompt-log/` | Round 2's is easy: prompts were fixed upfront; log = prompt + follow-ups used + outcome per level, from `docs/run-log.md` |

## 2. Metrics

Same single-source-of-truth shape as round 1:

```js
window.SEVEN_LEVELS_METRICS = {
  1: { fable: { prompts, calls, cost, mins, time }, opus: { ... } }, // ... 2–7
};
```

- **Primary source:** the orchestrator's captured JSON results (`total_cost_usd`, `duration_ms`, session IDs) in `docs/run-log.md` — no transcript reverse-engineering needed this round.
- **`calls` (API calls) and machine-time refinement:** mine transcripts under `~/.claude/projects/-Users-chizhang-experimental-design-pink-cybertruck-web/` (consolidated post-run from the `-...-fable-trials`/`-...-opus-trials` project dirs) per the methodology in `CLAUDE.md` (dedupe by `message.id`; queue-operation timestamp gotcha; re-verify pricing via the claude-api skill).
- **Note:** `total_cost_usd` from the CLI and the transcript-mined API-equivalent may differ slightly — compute the transcript-mined number for comparability with round 1, and say which is which.
- **No total rows** in per-level tables.
- Deck conventions from round 1: metrics data is **duplicated inline** in the slide chart script — update both places when numbers change; adding a slide renumbers `#/N` deep links; a 4-col table once overflowed small viewports (`table-layout:fixed` fixed it); bust local preview cache with `?v=N`.

## 3. Verification before deploy

- Local preview: `python3 -m http.server 8899` from repo root (the Browser-pane preview tool rejects `file://` — use `preview_start {url}` once, then `navigate`).
- Check: every level page renders both demos; compare sync-scroll works at L1/L5/L7 (the three talk pivots); deck renders at small viewport; L5 video autoplays muted; metrics strips match `metrics.js`; no 404s (the video file must be in `demos/fable/` AND `demos/opus/`).

## 4. Deployment (Vercel)

- Static site, **no build step**. Deploy from repo root: `vercel deploy --yes` (preview) → verify → `vercel deploy --prod --yes` (production) **only with Chi's explicit go-ahead**.
- CLI is authed as `chizhangucb`. Suggested project name: `claude-code-seven-levels-pink-cybertruck` (production domain `<name>.vercel.app`).
- Round-1 gotchas that WILL bite otherwise:
  - **Preview URLs are SSO-protected** — `curl -L` follows to a vercel.com login page that returns 200, so curl "success" on a preview URL is meaningless. Verify content against the production domain, or grep response bodies for expected strings. Chi can view previews in his browser (he's authed).
  - Git push and prod deploy are **independent** — pushing does not deploy.
  - Project rename ≠ domain move; a manually `vercel alias`-ed domain is treated as a preview alias (SSO-gated). Only a project production domain is public.
  - `/skill/` has no `index.html` — link to the GitHub tree, not the bare path.

## 5. GitHub (optional, Chi's call)

If publishing the repo like round 1: `git init`, commit with the noreply email `8743300+chizhangucb@users.noreply.github.com` (GitHub rejects pushes exposing private emails; round 1 had to rewrite history over this), suggested repo name matching the Vercel project. Trial dirs stay gitignored; `demos/` + `CHECKSUMS` are the published evidence. **Ask Chi before creating/pushing anything public.**

## 6. Order of operations (Phase 2–3 of the kickoff)

1. Experiment done, `demos/CHECKSUMS` verified (Phase 1 exit criteria).
2. Compute metrics → `assets/metrics.js` (+ inline deck copy).
3. Build site pages (adapt round-1 code) → thumbs → prompt-log.
4. Fill every `⟨TBD⟩` in `docs/talk-skeleton.md` from actual results; build `slides/index.html` from the filled skeleton.
5. Local preview + full verification pass (§3).
6. Vercel **preview** deploy → hand Chi the URL → **stop and ask** before production / GitHub.
