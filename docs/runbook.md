# Runbook — Seven Levels, Round 2: Pink Cybertruck

Master procedure for running the experiment. The prompts live in `docs/prompts/level-{1..7}.md`; the assets Chi prepares are in `docs/asset-checklist.md`; progress is logged in `docs/run-log-template.md`.

## 1. The experiment contract

- **One product** (a pink Cybertruck selling site), **seven levels** of technique, **two models** — Claude **Fable 5** and **Opus 4.8** — given **identical prompts, assets, and skills**. The only variable between the two runs is the model.
- Every level's output is a named, immutable HTML file (`pink-cybertruck-web-level-N.html`). Post-run, outputs are copied byte-identical to `demos/` and SHA-256 checksummed. Verbatim evidence or it didn't happen.
- Levels 4 and 5 are intentionally **swapped vs Jack Roberts' source video** (4 = UI snipping, 5 = AI video) — carried over from round 1; keep this ordering everywhere.
- **Disclosed deviations from round 1:** (a) both models get identical L3/L4 inputs (round 1 was asymmetric); (b) "keep level N-1 untouched" is baked into every prompt up front; (c) new L7 target (teenage.engineering, was antigravity.google); (d) recommended sequencing is one-session-per-level (was batched); (e) the L4 component is swapped from balloons-js to a 21st.dev-style `DottedSurface` (three.js dot-wave, always on — no trigger); (f) levels may be driven headlessly by an orchestrator session via `claude -p` (round 1 was interactive) — see `docs/orchestrated-run.md`.

### The ladder

| Level | Technique | External tool |
|---|---|---|
| 1 | Grab and go | — |
| 2 | Screenshots & references | godly.website / Land-book / Awwwards |
| 3 | Design skills | power-design, ui-ux-pro-max |
| 4 | UI snipping | DottedSurface — three.js (21st.dev-style) |
| 5 | AI-generated video | OpenArt (Nano Banana → Kling/Seedance) |
| 6 | Data & brand research | Firecrawl → tesla.com |
| 7 | Design extraction blueprint | design-language skill → teenage.engineering |

## 2. Preflight (T-1 day)

1. Complete `docs/asset-checklist.md` — all 5 assets in `references/`.
2. Dry-read all 7 prompt files. Every prompt names its output file; every level ≥ 2 has the keep-untouched line; follow-ups ready at L2/L4/L7; L6 fallback present.
3. Re-verify the skill copy (must match round 1 byte-for-byte):

   ```sh
   cd skill/design-language && shasum -a 256 -c <<'EOF'
   ebbc30b6661a7b54068cb0615dab19c948dae7530c32f7189633599eadba80fa  SKILL.md
   75006f44317e1332724a41d74f06133d0556f0b1d7bc203583469971167a6dcc  README.md
   cf0474589a79ab134269aebce214767678a6e898fe0577d26166329d5e69326f  process.md
   4e3fbd2eee6a5c2adee1fec613a463a841d8bee6a2b2bca5c5ee1026420e3fec  brief-template.md
   EOF
   ```

4. Build + test the zip (asset-checklist #5): `unzip -l references/design-language-skill.zip` → 4 files.
5. Open a throwaway Claude Code session: confirm **Firecrawl MCP** is connected (`/mcp`), and record the globally installed skills/plugins into the run log (environment snapshot).
6. Re-probe the L7 target — the CSS filename hash rotates:

   ```sh
   curl -sL https://teenage.engineering/ | grep -o 'assets/root[^"]*\.css'
   ```

   If the root stylesheet is gone, switch L7 to the fallback (`https://linear.app/` — see `docs/prompts/level-7.md`).
7. Confirm the MP4 autoplays muted in a browser tab.

## 3. Setup (run morning)

```sh
cd /Users/chizhang/experimental/design-pink-cybertruck-web
mkdir -p fable-trials opus-trials
cp references/reference-for-pink-cybertruck-web.png \
   references/reference-real-cybertruck.png \
   references/pink-cybertruck-video-by-openart.mp4 \
   references/design-language-skill.zip \
   fable-trials/
cp references/reference-for-pink-cybertruck-web.png \
   references/reference-real-cybertruck.png \
   references/pink-cybertruck-video-by-openart.mp4 \
   references/design-language-skill.zip \
   opus-trials/
git check-ignore fable-trials opus-trials   # must print both
```

All assets go into **both** trial dirs **before the first session** — prompts use bare `@filename` mentions, so files must sit at trial-dir root, and identical folder contents keep the "model noticed the extra files" variable controlled (Fable did, in round 1's level 1).

Keep `references/dotted-surface-component.tsx.txt` open in an editor — it gets pasted into the L4 message, not `@`-mentioned.

## 4. Session plan

**Recommended — Option C: orchestrated headless run.** Open ONE fresh session at the repo root and let it drive all 14 level-sessions via `claude -p` (each level still gets its own real session, transcript, and model). Full mechanics, kickoff prompt, and extra preflight in `docs/orchestrated-run.md`. Same guarantees as Option A below, minus the manual labor.

**Option A (manual): one fresh session per level, strictly sequential within a model.** 7 sessions × 2 models.

- Levels chain through **files on disk**, not session context — nothing is lost by closing sessions.
- Per-level metrics come free (one transcript = one level).
- The round-1 overwrite class of bug becomes impossible.
- Skills installed at L3 persist on disk into L4–6, matching the ladder's cumulative nature.

**Model interleaving:** the Fable and Opus runs may proceed **in parallel** — separate trial dirs mean separate cwds, separate transcript project dirs, zero shared files. Within one trial dir, **never more than one live session**.

**Option B (round-1 fidelity):** batch L1–2 / L3–6 / L7 per model. Cheaper on session overhead, but per-level metrics require timestamp-bucketing within transcripts, and it reinstates the L6/L7 concurrency temptation. If you use it: **L7 must wait until the L3–6 session is closed.** Not recommended.

## 5. Per-level procedure

For each level, in order:

1. Open `docs/prompts/level-N.md`; do its **Pre-prompt setup**.
2. Start a fresh Claude Code session **in the trial dir** (`cd fable-trials && claude` with the right model selected).
3. Paste the prompt exactly. **Immediately** log session ID + UTC start timestamp in the run log.
4. Use follow-ups **only** from the file's "Ready follow-ups" list. Log any you send.
5. Verify the output file exists with the exact expected name, and level N-1 is untouched.
6. `shasum -a 256 pink-cybertruck-web-level-N.html` → run log. Log end timestamp.
7. **Close the session**, then move on.

Resist the urge to polish beyond the scripted follow-ups — extra prompts contaminate the level's metrics and the A/B comparison.

## 6. Post-run

1. Copy outputs to canonical dirs:

   ```sh
   mkdir -p demos/fable demos/opus
   cp fable-trials/pink-cybertruck-web-level-*.html demos/fable/
   cp opus-trials/pink-cybertruck-web-level-*.html demos/opus/
   cp fable-trials/pink-cybertruck-video-by-openart.mp4 demos/fable/
   cp opus-trials/pink-cybertruck-video-by-openart.mp4 demos/opus/
   ```

   (Rename to `level-N.html` inside `demos/` if following round 1's naming. Copy any poster/extra assets a model generated alongside.)
2. Generate checksums: `cd demos && shasum -a 256 fable/* opus/* > CHECKSUMS`.
3. Preserve process evidence worth keeping (Firecrawl scrapes, L7 extraction kit) into `process-artifacts/`.
4. Mine metrics from the trial-dir transcript project dirs (during the run: `~/.claude/projects/-...-web-fable-trials/` and `...-web-opus-trials/`; after post-run cleanup these were merged into `~/.claude/projects/-Users-chizhang-experimental-design-pink-cybertruck-web/`) per the methodology in `CLAUDE.md`. The run log's session IDs make the session→level mapping trivial this time.
5. **Cleanup (kills the duplication):** once `demos/CHECKSUMS` is generated and every copied file is verified byte-identical to its trial-dir original (`diff -r` or re-checksum both sides), the trial dirs are safe to delete — `demos/` + the transcripts under `~/.claude/projects/` are the durable evidence. (Round 1 did exactly this.) Keep them until after metrics mining if you want a belt-and-suspenders backup.
6. Only then: build the tutorial materials and deploy per `docs/publish-plan.md` (Phase 2–3 of the orchestrated kickoff), filling `docs/talk-skeleton.md` from actual results.

## 7. Known failure modes & recoveries

- **The overwrite incident (round 1):** Fable's L7 session ran concurrently with L6 and silently wrote its output over `level-6.html`. Recovery required mining the other session's transcript to rebuild. Prevented this round by sequential sessions + immediate post-level checksums. If it somehow happens: the transcript JSONL under `~/.claude/projects/` contains every file write — the level can be faithfully rebuilt, but must be disclosed as a rebuild in CHECKSUMS notes.
- **tesla.com scrape blocked/thin:** use the fallback follow-up (`tesla.com/cybertruck`) or retry with longer `waitFor`. Firecrawl caches under `.firecrawl/` (gitignored).
- **teenage.engineering CSS moved:** preflight step 6 catches it; fall back to linear.app and note the swap in the run log.
- **L7 delivers a kit, not a page:** expected, not a failure — the two scripted follow-ups in `docs/prompts/level-7.md` push blueprint → deliverable.
- **Model asks about unrelated files in the folder (L1):** answer minimally ("ignore them for now") and log the exchange — it's a fun observation, not a contamination, as long as both models had the same folder.
