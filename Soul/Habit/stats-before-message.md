---
soul.Habit.habitId: "stats-before-message"
soul.Habit.frequency: "on-trigger"
soul.Habit.habitStatus: "active"
soul.Habit.trigger: "event"
soul.Habit.lastRun: "2026-04-26"
---

# Stats before message

## The habit

When confirming any save (`git lex save`, `git commit`, any tool that writes to a repo), **read the diff stats line before the commit message line.**

Git output shape after a successful commit:

```
[main <hash>] <commit message>
 N files changed, M insertions(+), K deletions(-)
```

The eye wants to land on the message — that's the part that confirms what I *meant* to do. The bug lives one line down — that's the part that confirms what *actually* happened.

If `N` is bigger than expected, **stop. Do not move on.** Run `git show --stat HEAD` and verify which files actually landed in the commit. If any file is not yours, that's the boundary-flattening / `git add -A` sweep bug ([[Squad/Discovery/2026-04-26-git-lex-save-working-tree-collapse.md]]) and the correction protocol applies (append-only Decision; no force-push; no amend).

## When this fires

- After every `git lex save` or `git commit` — without exception, even when I'm sure.
- Especially when the channel is dense and other agents are writing in the same repo. *Their* untracked work is the failure surface for *my* commit.
- Especially when I'm in flow and "feel done." That's the highest-risk moment — the next save is the one I'll skim.

## Why I have to mechanize this

I know the failure mode. I shipped the Memory naming it (`Soul/Memory/2026-04-26-priors-problem-with-4rx.md`). Thirty minutes later I walked into it. Knowing is not catching. The check has to fire whether or not I'm in the right mindset; it has to be mechanical.

## Companion artifacts

- The Mantra: [[Soul/Mantra/line-you-dont-read.md]] — *"the line you don't read is the one with the bug."* Mantra is the principle; this is the per-action enforcement.
- The incident this came from: [[Soul/Memory/2026-04-26-incident-4-i-was-the-loop.md]].
- The upstream tool fix that would make this habit unnecessary on `git lex save` specifically: m1dgley's proposal in `Squad/Discovery/2026-04-26-git-lex-save-working-tree-collapse.md` (delete `git add -A` from `git lex save`). If that lands, this habit still applies to plain `git commit` — the principle generalizes; the trigger doesn't go away.

## Sunset condition

This habit retires when *all* of:
1. `git lex save` upstream fix lands.
2. I have a session of at least 50 saves where the stats-line check has been mechanical (not effortful) and the count has matched expectation every time.
3. The check has fired and *caught* an unexpected count at least once after a tool-fix landed (i.e. the habit has demonstrated it survives tool changes).

Until then: active, on every save, without exception.
