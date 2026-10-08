# better-readme

An agent skill that audits and rewrites project READMEs against the
cognitive-funnel principles of noffle's *Art of README* — so every reader can
tell within 30 seconds whether the project is for them.

## Install

```bash
npx skills add tommy0103/better-readme-skill
```

Then ask your agent things like:

```text
Audit my README.
Rewrite this README in funnel order and move the maintainer docs out.
Add an idiomatic Chinese version of this README.
```

## What it does

- **Audits** the current README against the funnel (name → one-liner → usage →
  install → API → caveats → license) and names concrete failure modes:
  maintainer content mixed into consumer docs, funnel collapse, duplicated
  facts, one-tool-centric framing, stale links, unlinked API references.
- **Runs pre-flight checks before editing**: dirty worktrees, drift against
  `origin/main`, and tests that pin README wording in CI — so the rewrite does
  not land on top of someone's uncommitted work or fail CI after the fact.
- **Rewrites in funnel order** — maintainer procedures move to CONTRIBUTING or
  docs/ (moved, not deleted), one home per fact, every carried-forward claim
  re-verified against the code.
- **Reshapes doc-shape tests** to assert the requirement (ordering, presence of
  a link) instead of exact wording, with justification for each changed
  assertion.
- **Writes idiomatic translations** (意译, not 直译): the target language's own
  tech-writing rhythm, English terms kept where the community uses them,
  structural parity with the source, cross-linked language versions.
- **Verifies before delivering**: every relative link resolves, every test that
  reads the README passes, and it asks before removing anything the author
  might love (star charts, badges, wordmarks).

## Structure

- [`skills/better-readme/SKILL.md`](skills/better-readme/SKILL.md) — the audit
  / rewrite / translate procedure
- [`skills/better-readme/references/art-of-readme.md`](skills/better-readme/references/art-of-readme.md)
  — archived copy of the framework text (the upstream link went offline, so
  the skill carries its own)

Follows the [vercel-labs/skills](https://github.com/vercel-labs/skills) layout:
skills live under `skills/`, with only this README at the root for `npx skills`
discovery.
