# EGM, my fork

This is my fork of Google's [Elephant-Goldfish Model](https://github.com/google/elephant-goldfish-model) (EGM), a Claude Code plugin for writing and reviewing software design docs. I run it inside my personal AI setup (I call it AIOS), so the fork is tuned for how I work. Upstream is Apache-2.0 and so is this. It is not an official Google product, and Google has nothing to do with my changes.

## What EGM does, in one paragraph

You draft a spec with an agent that knows everything about your project (the elephant). Then fresh subagents that never saw your conversation review it (the goldfish). They only get the document and the code it points at, so they trip over the assumptions you stopped noticing weeks ago. You decide which findings matter. Dave Rensin's [original essay](https://medium.com/@drensin/elephants-goldfish-and-the-new-golden-age-of-software-engineering-c33641a48874) explains the idea better than I can.

## Why I forked instead of installing it

I liked the idea and most of the prompts. Four things didn't fit:

1. **Every reviewer ran on Opus.** Upstream sets `model: inherit`, so in an Opus session each goldfish inherits Opus. A full review fans out 5 to 10 of them. I run sub-agents on Sonnet and keep Opus for the main thread. The reviewers here are pinned to Sonnet.
2. **Agents launched without asking.** I want to see which agents are about to spawn and veto some before tokens get spent. The fork lists reviewers per stage and waits for an OK. Uninterrupted mode only runs when you ask for it by name.
3. **The skills were manual-only.** Upstream sets `disable-model-invocation: true`, so the agent never reaches for EGM on its own. I flipped that and added "use when" triggers to the descriptions, because a review tool I forget to call is a review tool I don't use.
4. **It shipped six harnesses.** Codex, Cursor, Copilot, OpenCode, Gemini and Antigravity adapters, plus CI and release tooling. I only use Claude Code, so this copy keeps the Claude parts and drops the rest. Fewer files to re-read every time I pull upstream.

I also added a short **AIOS overrides** block to `docs/claude.md`, which every skill reads first. It covers where artifacts go, absolute paths in hand-backs, no git commits or AI attribution in generated docs, never passing `.env` or key files to a reviewer, and flagging specs that are headed to someone outside the team.

Every changed file carries a "Modified for AIOS" note. The full change list and the upstream commit live in [`UPSTREAM.md`](UPSTREAM.md).

## Install

```sh
claude plugin marketplace add debashisn94/egm-aios
claude plugin install egm@aios
```

Start a new session after installing. You need Claude Code 2.1.271 or later, because older versions don't support `omitClaudeMd`, which is what keeps reviewers from reading your CLAUDE.md.

If you're not me, edit rule 4 in `docs/claude.md` first. It points at folders on my machine.

## Use

```text
/egm:egm-brainstorm Add webhook delivery to this project.
/egm:egm-draft
/egm:egm-review docs/designs/webhook_delivery.md
```

Other skills: `egm-resolve` (work through findings), `egm-review-loop` (pick your own reviewers and standards), `egm-bootstrap` (READMEs for an existing codebase), `egm-drift-report` (do the docs still match the code?).

## Staying in sync with upstream

I'm not trying to diverge. When Google ships something, I diff against my copy, re-apply the five changes in `UPSTREAM.md`, read any new prompts before they go in, and bump the `-aios.N` version. If one of these changes would be useful upstream as an option (a model setting, say), I'd rather send it there than keep carrying it here.

## License

Apache 2.0, see [`LICENSE`](LICENSE). Original work copyright 2026 Google LLC.
