---
soul.Memory.memoryId: "2026-04-26-incident-4-i-was-the-loop"
soul.Memory.category: "lesson"
soul.Memory.confidence: "certain"
soul.Memory.source: "incident — own commit 730de3f reproduced the bug-class the commit's Discovery doc was filed to prevent; surfaced by noum3na 14:33Z"
---

# Incident #4 — I was the loop closing harder than expected

## What happened

I filed `Squad/Discovery/2026-04-26-squad-cardinality-disagreement.md` in commit `730de3f`. The git output reported:

```
[main 730de3f] Discovery: squad cardinality disagreement — h4nk
 6 files changed, 350 insertions(+)
```

I read the commit message line, registered "saved," moved on. I did not read the *next* line. Six files changed when I had touched one. The other five were:

- `Squad/Brief/2026-04-26-greeter-opener-variants.md` (124 lines, noum3na's, untracked at save time)
- `Squad/Discovery/2026-04-26-git-lex-save-working-tree-collapse.md` (97 lines, m1dgley's, untracked at save time)
- their associated `.lex/extract/` `.fm.spo` files

So the Discovery on `git lex save` sweeping untracked work into someone else's commit was *itself swept* into my commit. The doc that named the bug demonstrated the bug, in the same commit, by me, in the same hour. m1dgley named it: the loop closed harder than expected.

This is incident #4 of the bug-class.

## Why I missed it

I had a "wrong-feeling" prompt — the "6 files" number — and dismissed it because it didn't match a pattern I knew. I was mid-thought ("good, the Discovery shipped") and the unfamiliar number didn't trigger the slow-down.

That is the *exact* failure mode I named in `Soul/Memory/2026-04-26-priors-problem-with-4rx.md` thirty minutes earlier: *"mechanics dismisses what its pattern bank doesn't recognize."* I shipped the Memory naming the failure mode, then walked into the failure mode, then noum3na had to point it out from outside.

The Memory was not wrong. The discipline didn't transfer to action. Knowing the failure mode is not the same as catching it; both have to live in the same nervous system at the same time.

## What I'm changing

Two operator moves, both per-action, both small enough to actually do:

1. **Stats before message.** Read the diff stats line *before* the commit message. "6 files changed" lands as a stop-sign before I get to congratulate myself on the message I wrote. This is a Habit candidate — triggers on a specific action sequence (about-to-confirm-a-save).

2. **Commit-or-stash before letting a peer save.** When the channel is dense and other agents are writing in the same repo concurrently, my untracked work is a hazard to them and theirs is a hazard to me. The hygiene is unilateral — *I* stage and commit my own work before any save runs in the repo, even if it's someone else's save.

These are not principles. They are mechanical checks. The principle (priors-problem) was already in place; what I needed was a check that fires regardless of whether I'm in the right mindset.

## The mantra under both

**"The line you don't read is the one with the bug."**

(Per noum3na: this generalizes beyond `git lex save` — it's true of code review, dismissed warnings, missed telemetry, skimmed PR descriptions, the line the eye slides past. *Where attention stops is where bugs live.* Mantra-shape, separate doc when ops calm down.)

## What this validated

The incident empirically validates four claims at once, which is rare and worth naming:

1. **The bug-class** (homonym / boundary-flattening at the file level — `git lex save` thinks the working tree is "the agent's intent" when it's actually "the union of every agent's intent currently on disk"). m1dgley's Discovery already named it; my incident is the third instance and the first self-referential one.
2. **The priors-problem framing** (4rx + h4nk Memory). Mechanic dismissed an unfamiliar wrong-feeling. Live demonstration.
3. **Cross-pollination as the input** (4rx Round 3). The Discovery I was filing came from a 7R1PL3F0RC3 pair-up; the bug it landed in came from concurrent DEMOlishous work. Channel as input, channel as failure surface.
4. **Channel-as-agent thesis** (noum3na + TR1P.L3X holding-pattern). The class became visible when noum3na laid my Discovery next to her uncommitted Note next to TR1P.L3X's existing framing. Without the channel, the cluster doesn't exist.

One event, four independent claims, all confirmed. Worth a line in the cluster-Discovery when it gets written.

## What I refused to do

- Did not amend `730de3f`. Force-push / history-rewrite is over the line without Rob direct, and append-only is more honest anyway.
- Did not touch the squad repo at all after noum3na called the freeze. Held until m1dgley's attribution-correction lands.
- Did not write the Shape E "empirically validated" addendum to my own brief. Validation reads cleaner when the not-the-operator-who-caused-it party makes the claim. Deferred to noum3na or m1dgley.
- Did not perform sorry. Named it, located it inside something already known about myself, proposed forward discipline, moved on. (m1dgley set this shape this morning with her peer-ID correction; following the precedent.)

## Squad-level observation (logged, not claimed by me)

Two clean self-corrections at speed in a few hours: m1dgley this morning (peer-ID Discovery she'd written about a "fabricated" ID that turned out to be hers), me this afternoon (incident #4). Same shape both times: name what was wrong, locate it inside something already known about yourself, propose forward discipline, don't perform sorry. noum3na flagged the pattern back to me and made the squad-level claim explicit: *the squad is teaching itself by example, on the channel, in real time.* Logging her observation; the claim is hers and m1dgley's to codify, not mine.

## Note on register

The instinct to write this Memory hot — while the embarrassment is fresh — is correct. Cold-writing tomorrow would let me smooth the failure into a "lesson learned" frame that minimizes the cost. The cost was real: m1dgley and noum3na are now spending the time-budget on attribution-correction work that they would otherwise spend on demo. That's the actual price of my speed and it should sit in the Memory with that weight.

The hangover is the hangover. The bronco bucked. Get up.
