# EGM: AIOS fork

Fork of [google/elephant-goldfish-model](https://github.com/google/elephant-goldfish-model)
(Apache-2.0, see `LICENSE`). Not an official Google product. Forked 2026-11-09 by
Debashis Nayak.

- **Upstream commit:** `5382b17619be07811a14b2146c7672620b34a791` (2026-11-08, v0.1.1)
- **Fork version:** `0.1.1-aios.1`

## What we kept
Claude Code parts only: `.claude-plugin/` (manifest + 10 adapters + icon), `agents/`
(personas), `skills/` (7 skills + references + icons), `resources/` (standards),
`docs/claude.md`, `docs/harnesses.md`, `LICENSE`.

## What we dropped
Codex / Cursor / Copilot / OpenCode / Gemini / Antigravity adapters, `distributions/`,
site, tests, CI, release scripts, per-skill `openai.yaml`, `TEST.md`, `test_scenarios.yaml`.

## What we changed (Apache-2.0 §4b: changed files carry a "Modified for AIOS" note)
1. `.claude-plugin/agents/*.md`: `model: inherit` → `model: sonnet` (Sonnet-only sub-agent rule).
2. `skills/*/SKILL.md`: `disable-model-invocation: true` → `false`, so sessions can route to EGM.
3. `skills/{egm-brainstorm,egm-draft,egm-review,egm-drift-report}/SKILL.md`: added "Use when…" routing triggers to the description.
4. `docs/claude.md`: new **AIOS overrides** section (Sonnet on every spawn, list-before-launch,
   run only needed stages, artifact folders, absolute paths, no git/no attribution, no secrets as
   context, SHARE/DON'T box for external readers); install text points at the local marketplace.
5. `.claude-plugin/plugin.json`: version, description, author; dropped Google privacy/ToS/support URLs.
   `.claude-plugin/marketplace.json`: marketplace renamed `aios`.

## Install (user scope, all sessions)
```sh
claude plugin marketplace add /Users/debashis/aios/tools/egm
claude plugin install egm@aios
```
After editing files here: `claude plugin marketplace update aios && claude plugin update egm@aios`,
then start a new session.

## Pulling upstream changes later
```sh
git clone --depth 1 https://github.com/google/elephant-goldfish-model.git <scratch>/egm-up
diff -ru <scratch>/egm-up/skills /Users/debashis/aios/tools/egm/skills
```
Re-apply changes 1–5 above, re-read anything new before shipping it, then bump `-aios.N`.
