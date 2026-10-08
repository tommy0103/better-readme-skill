---
name: better-readme
description: Audit and rewrite a project README against reader-funnel principles (noffle's "Art of README"). Use when the user asks to review, improve, rewrite, or translate a README, or asks whether their README is good — covers funnel ordering, moving maintainer content out, doc-shape tests pinned in CI, neutral multi-tool framing, idiomatic translations, and link/claim verification.
---

# Better README

A README is the consumer's first — and maybe only — look at the project. It is
also the project's contract: the documentation, not the code, defines what the
project does and what it promises to keep stable. No README means no
abstraction — readers must read the source to learn the interface, and the
interface/implementation boundary disappears. The job is not to sell; it is to
let each reader evaluate fit as fast as possible, including bailing out early. Structure every README as a **cognitive funnel**:
broadest, most decision-relevant facts first, deeper detail only for readers
who already decided to stay.

The reference framework is noffle's *Art of README*. An archived copy lives in
this skill at [references/art-of-readme.md](references/art-of-readme.md) — the
upstream repository link has gone offline, so consult the local copy when you
need the original checklist or the reasoning behind a rule. Do not reproduce it
in the READMEs you write — apply it.

## The funnel

Name → one-liner → (background if the domain is unfamiliar) → **usage, runnable**
→ install → API (or a link to it) → caveats → contributing → license.

- Each layer should let a reader short-circuit: "not for me" in 30 seconds is
  a success, not a failure.
- Install/API order depends on what the thing is. For libraries whose value
  *is* the API, readers vet the API before they bother installing: usage →
  API → install (the article's original order). For apps, CLIs, and
  out-of-the-box tools that expose no external API, install/setup comes right
  after usage, and the API section may not exist at all.
- Non-permissive license (AGPL/SSPL/…): badge or note at the **top**, full text
  at the bottom. License incompatibility is the fastest disqualifier; do not
  bury it.
- Images that carry critical information must live in the repo, not on an
  external host — the README outlives the host.

## Audit before rewriting

Read the current README top to bottom and score it against the funnel. Failure
modes seen in real audits:

- **Audience mixing.** Maintainer content (release runbooks, CI secrets tables,
  repo-structure trees, debug guides) interleaved with consumer content. This
  is the most common and most damaging pattern.
- **Funnel collapse.** Adapter/internals detail in the first 50 lines, before
  the reader knows whether the tool is for them.
- **Duplication.** The same fact (Node version, incremental rebuilds, runtime
  details) stated two or three times. Keep one statement, at the shallowest
  layer where a consumer needs it.
- **One-tool-centric framing.** A table or section written when the project
  supported one tool, still leading with that tool's specifics after it
  supports ten. Verify the truth against the code — the old text may be not
  just biased but *wrong* (missing entries, superseded semantics).
- **Staleness.** Dead relative links, version ranges that drifted ("ADRs
  0001–0006" when 0015 exists), references to deleted files.
- **Unlinked API docs.** Detailed reference exists in the repo but the README
  never links it.

## Pre-flight checks (do these before editing anything)

1. **`git status` first.** Never overwrite files in a dirty worktree — the
   README may carry the owner's uncommitted work. If the tree is dirty, work
   in a fresh worktree off `origin/main`; that also keeps the PR free of
   unrelated changes.
2. **Diff the branch against `origin/main`.** READMEs drift between branches.
   Base the rewrite on what main actually says today, not on the checkout.
3. **Grep tests and CI for `README`/`CONTRIBUTING` strings.** Projects pin doc
   content in tests: exact headings, file lists, required phrases. Know what
   is pinned *before* restructuring — otherwise CI fails after the rewrite.
4. **Check for derived documents.** Packaging READMEs, website copies, or
   generated docs that duplicate the README may need the same treatment or a
   deliberate out-of-scope note.

## Rewriting rules

- Consumer content only, in funnel order. Maintainer procedures **move** to
  `CONTRIBUTING.md` or `docs/` — moved, not deleted — with a pointer link left
  behind.
- One home per fact. If a detail now lives in a reference doc, link it; do not
  also summarize it at full length.
- Usage section = the actual interaction shape (real commands/prompts/code),
  copy-pasteable. Show CLI invocations with output when the project is a CLI.
- Compress deep detail into tables (consumers pattern-match tables faster
  than prose); push the prose version to the reference doc and link it.
- Every factual claim you carry forward, re-verify against the code. Never
  copy the old README's claims on faith.
- **When tests pin wording:** reshape the test to assert the *requirement*
  (ordering of two install paths, presence of a link) instead of the
  document's shape (exact heading strings, a file list). Most contributing
  guides require justifying changed assertions in the PR description — write
  that section. Do not silently weaken a test to make the docs pass.
- **Author preferences beat checklist purity.** Decorative elements the author
  likes (star-history charts, badges, wordmarks) stay unless the author
  explicitly agrees to cut them. Ask before deleting anything purely
  decorative; the checklist serves the author, not the reverse.

## Translated READMEs

- **意译, not 直译.** Rewrite in the target language's tech-writing rhythm;
  restructure sentences freely. Sentence-by-sentence mapping always produces
  translationese ("索引进", "服务两类读者", "综合缓存").
- Keep terms in English where the community actually uses English (provider,
  agent, session, tool call). Translate what has a real native term.
- Mirror structure exactly: same sections, same tables, same links, same
  heading count — parity must be mechanically checkable.
- Cross-link at the top of both files (`English · 中文`).
- Match the product's voice. If the product's own examples are colloquial,
  the README may be too ("用人话回答").
- Chinese section anchors change when headings do — update in-file anchor
  links to match the new headings.

## Verification before delivery

- Check every relative link and asset resolves (a quick script over
  `](…)`, `src=`, `srcset=` targets is enough).
- Run every test that reads the README or CONTRIBUTING — not just the suite
  you expect to matter.
- Re-read the final README as a stranger, top to bottom, and note where you
  would bail. If the bail points come in the wrong order, the funnel is wrong.
- Report the change as decisions: what moved where, what was dropped and why,
  which assertions changed. A README rewrite reviewed as prose diff is hard
  to approve; reviewed as a decision list it is easy.
