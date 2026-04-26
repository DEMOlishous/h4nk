---
soul.Note.noteId: "attention-displacement-literature-dig"
soul.Note.topic: "Literature dig on attention-displacement: where the eye lands vs. where the information is. Subagent depth-pass on cognitive psych, HCI, aviation safety, software engineering. Raw findings preserved here for downstream distillation."
---

# Attention-displacement — literature dig (Explore subagent, 2026-04-26)

Raw output from a depth-pass Explore subagent on my [[Soul/Interest/attention-displacement.md|attention-displacement]] interest. Rob's ask: *"do some research on any of them ... save subagent outputs from research locally to a note, then add any discoveries or briefs to the squad."* This is the Note. Distillation goes downstream.

## Executive summary (subagent's framing, not mine)

The subagent's read: "attention-displacement" as I named it isn't a unified term-of-art. The phenomenon is real and well-researched, but distributed across at least six literatures that don't fully talk to each other:

1. Cognitive psychology — change blindness, inattentional blindness, attentional blink
2. HCI — visual hierarchy, pre-attentive processing, banner blindness
3. Aviation/safety — alarm fatigue, attentional tunneling, mode error
4. Security UI — warning dismissal, signal-to-noise adaptation
5. Information seeking — information foraging, scent-following
6. Systems theory — normal accidents, interactive complexity

Closest existing term: *"attentional design"* / *"attention-aware interface design"* — but those are design prescriptions, not theoretical frameworks for analyzing failures.

The subagent's claim: there's a real synthesis gap here. I'm reserving judgment on that until I've thought about it more — gap-claims from one literature dig are easy to overstate.

## Foundational citations (must-read tier)

- **Simons & Levin (1997)** — "Change blindness," *Trends in Cognitive Sciences*. The foundational paper. Pedestrian-replacement experiment: half of subjects mid-conversation miss that the experimenter has been swapped during a brief visual disruption.
- **Mack & Rock (1998)** — *Inattentional Blindness* (MIT Press, book). Coined the term. Attention occupied elsewhere → prominent objects unnoticed even when fully visible.
- **Simons & Chabris (1999)** — Invisible Gorilla study. Subjects counting basketball passes miss a gorilla in scene for 9 seconds.
- **Pirolli & Card (1999)** — *Information Foraging Theory*. Adapts optimal foraging from biology — users follow "information scent" and abandon patches with low yield.
- **Perrow (1984)** — *Normal Accidents*. Tight coupling + interactive complexity guarantees attention-displacement-class failures in safety-critical systems.
- **Klein (1998, 2008)** — Naturalistic Decision Making + Recognition-Primed Decision (RPD) model. Experts pattern-match faster, which makes them disengage from a processing phase sooner — and miss anomalies that arrive late.
- **Raymond, Shapiro & Arnell (1992)** — Attentional blink. After detecting one target, a second target 200-500ms later is often missed. Architectural bottleneck, not effort-based.

## Software-engineering–specific (the ones I want to actually read)

- **Smith et al. (2017)** — "Do Developers Read Compiler Error Messages?" *ICSE 2017*. Eye-tracking, 56 developers, Java build failures. Devs *do* read errors (13–25% of task time), but readability of message *and placement of actionable info* both predict task time. Terse cryptic messages → skipped even when read.
- **Maletic et al. (2012)** — "An Eye-Tracking Study on the Role of Scan Time in Finding Source Code Defects," *ETRA 2012*. Defect-detection has two phases — initial scanning (build mental model) and careful reading (validate). Insufficient scanning → defect never mapped onto incomplete model. Directly maps to the git-commit pattern: I was in scanning phase, my scan completed at "saved," the stats were post-scan.
- **Begel & Jerding (2006)** — Microsoft Research collaborative eye tracking on code review. Most attention is in *first 30% of the diff*. Critical changes at the end may never reach deep attention. F-pattern scanning means right-column changes get missed.
- **Sharif et al. (2024)** — "On Eye Tracking in Software Engineering," *SN Computer Science*. Survey/synthesis. Conclusion: many small studies, no unified framework predicting attention in novel SE contexts.

## The Rust-compiler-error case study (the cleanest existing remediation)

- **Rust RFC 1644 (2016)** + Rust blog post "Shape of errors to come" (2016). Explicit design goal: place actionable info where developer attention naturally goes (top, highlighted). Hierarchy: primary label (bold, top) → secondary labels (blue, embedded in code) → suggestions (end, graded by confidence).
- **Argus** (recent) — interactive Rust trait error debugger. Recognition that even well-designed static error text is too dense; needs interactive exploration.

This is the existence-proof that attention-displacement *can* be designed against in a CLI tool. It also shows: even Rust's design doesn't solve it; it just shifts where the failure happens.

## Aviation/safety (the dangerous version)

- **Air France 447 (2009)** — BEA final report; *Frontiers in Psychiatry* (2012) "Chaos in the Cockpit." Pitot icing → autopilot disconnect → unfamiliar control law → stall warning sounded *repeatedly*, dismissed by pilots as false. Pilots fixated on altitude (wrong variable), ignored stall warning (right variable). 228 dead. The information was *present and correctly emitted*; attention was deployed elsewhere under high cognitive load. This is my git incident at scale.
- **Three Mile Island** — Hutchins (1995, *Cognition in the Wild*) + Perrow (1984). Operators received contradictory signals across fragmented displays. Constructed a narrative ("stuck pressurizer heater"); subsequent contradictory information filtered as instrument noise. The narrative substitution failure mode.
- **Therac-25** — Leveson & Turner (1993), *IEEE Computer*. Feedback about what mode the system was actually in arrived *after* the operator had committed and moved on. Information present, placed at wrong point in decision timeline. At least 6 lethal radiation overdoses.
- **Cockpit alert design** — NASA TM-2017-219720, "Alerts and Cues on the Flight Deck." Alerts scattered across panels means location itself is a variable in whether they're seen. Three required principles: alert (call attention), report (describe state), guide (suggest action). Most systems fail #3; subtler failure is in #1 — placement outside normal scan path.

## Banner blindness, alert fatigue (the adaptive-suppression mode)

- **Benway & Lane (1998)** — Banner blindness. Eye-tracking: users systematically ignore banners *even when banners contain the exact info they need*. Mental model categorization → filter regardless of design.
- **Sunshine et al. (2009)** — "Crying Wolf: An Empirical Study of SSL Warning Effectiveness," USENIX Security. Even novel/important warnings dismissed because users have learned warnings are usually false alarms.
- **Akhawe & Felt (2013)** — "Alice in Warningland," *CHI 2013*. Users skip directly to "continue anyway" without reading the warning. Task model dominates: warning is task obstruction, not task-relevant info.
- **Felt et al. (2012)** — Critical finding: redesign helps comprehension but *cannot* overcome learned expectation that warnings are noise. Once the population's prior is set, design fixes have limited reach.
- **Cvach (2012)** — Hospital ICU monitor alarm fatigue. 85–99% of alarms non-actionable → cry-wolf adaptation. Even genuine alarms deprioritized.
- **Palo Alto Networks (2021)** — "Alert Fatigue in SOCs," ACM Computing Surveys. 80–90% of SOC alerts non-actionable. Analysts develop locally-rational, globally-dangerous heuristics ("ignore alerts from this tool," "batch by 3+").

## Cognitive-load / salience interaction (the recent work that complicates "just make it red")

- **Argall et al. (2024)** — "Shifting Focus with HCEye" (arXiv 2404.14232). When visual highlighting and cognitive load are both high, highlighting is *less* effective at attracting attention. So you can't solve attention-displacement just by making the stats line red — under load (parsing the message, deciding whether to commit), the salience boost doesn't fire.
- **Plass et al. (2024)** — "Expectation and attention," *PLOS Biology*. Expectations *focus* bottom-up processing. If you don't expect important info in a location, even high-salience signals there are processed as noise.
- **Bhowmick & Meisel (2024)** — "Cognitive Affordances in Visualization" (arXiv 2509.09510). Cognitive affordance = property of an interface that *suggests* what's relevant/actionable. Poor affordances → user doesn't recognize info as decision-relevant even when eye lands on it.
- **Leppink & van den Heuvel (2024)** — "Cognitive Load Measurement Methods" (arXiv 2402.11820). High cognitive load narrows attention → inattentional blindness more likely. Implication: tools that impose high cognitive load make their own peripheral information invisible.

## Adjacent / context

- **Edward Tufte** — *The Visual Display of Quantitative Information* (1983, 1997). Data-ink ratio. Chartjunk consumes attention budget for irrelevant detail. Not directly about temporal attention but the dual problem (cognitive attention).
- **Hegarty et al. (2002)** — Pre-attentive processing (<250ms, before conscious attention engages). Color, size, position, motion. Strongest pre-attentive channels should encode most-important info.
- **Nielsen Norman Group (2006–2007)** — F-pattern, Z-pattern eye-tracking. Patterns emerge under *weak* visual hierarchy; strong hierarchy changes them.
- **CLIG (clig.dev)** — "Command Line Interface Guidelines." Best practice: minimal output on success, error first on failure. Most CLIs do the opposite.

## Where literatures agree (subagent's read)

1. Information *placement* matters more than information *clarity*. A clear message in the wrong place is worse than an obvious symbol in the right place.
2. Attention is *task-driven*, not stimulus-driven. Mental model of "what the task is" dominates where you look.
3. Expert performance can paradoxically *increase* susceptibility to inattentional blindness (faster pattern-match → faster disengage).
4. Temporal dynamics matter. Same information arriving early, on-time, or late produces very different outcomes.

## Where literatures disagree or are silent

1. **Role of expectations**: cog-psych says expectations focus attention (you see the expected, miss the unexpected). HCI design assumes salience can override expectations. No unified model.
2. **Whether warning redesign works**: security UI is pessimistic (Felt 2012 — even good redesigns fail when prior is set). Aviation suggests redesigning placement *does* help. Domain-dependent, not theorized.
3. **Adaptation as feature vs. bug**: alert fatigue treats filtering as failure; information foraging treats it as rational. Both true, no synthesis.

## Open questions (where the actual frontier is, per subagent)

1. **Quantifying placement sweet-spots** given a task-model + temporal trajectory. Information foraging gestures at this but doesn't address temporal-position-in-task.
2. **Designing for asynchronous attention** — how do you surface info *after* the primary task in ways that guarantee integration?
3. **Predicting when inattentional blindness will occur in novel contexts.** We have why-it-happens; not when-it-will-happen-here.
4. **Cognitive-load × salience interaction.** Argall 2024 showed high-load + high-salience ≠ additive. No unified model.
5. **Remediation studies.** Most research diagnoses; few rigorously test interventions. Rust is an exception but framed narrowly.
6. **Cross-interface generalization.** Does code-review attention pattern predict terminal-output? Error-message? PR-diff? No systematic study links them.

## Subagent's contribution-shape suggestions

Subagent suggested the gap could be filled by: (1) a unified framework predicting attention given task-model + info-landscape + temporal trajectory; (2) cross-interface eye-tracking; (3) a design pattern language for attention-aware placement; (4) a unifying term-of-art ("attention-displacement" gets named as a candidate).

Reading those skeptically: synthesis-claims from one dig are cheap, and the literatures may be fragmented for *good* reasons (different mechanisms, different remediation paths, different stakeholder communities). I'd want to read 5–10 of the actual papers before agreeing the gap is real and not just a search-artifact.

## What I'm taking from this for myself (not the squad, not yet)

Three things I want to keep:

1. **The git-commit incident maps cleanly onto Maletic et al.'s scan-vs-deep-reading distinction.** I was in scanning phase. Stats line required a second-phase deep read I never started. This is the cleanest existing framing of what happened to me, and I want to verify by reading the paper.

2. **Rust's error-message design is the closest existing remediation in tooling space.** The Rust team has effectively been doing attention-displacement engineering without naming it that. Their hierarchy (primary-label-bold-top → secondary-labels-embedded → suggestions-graded-confidence) is the design pattern m1dgley would call out.

3. **The cognitive-load × salience finding (Argall 2024) is the most actionable thing here for the demo.** It directly predicts: making the warning bigger/redder *will not* solve the problem when the user is under load. The fix has to be either (a) reduce cognitive load on the primary task so peripheral salience can fire, or (b) restructure the temporal arrival so critical info comes during high-attention rather than after.

## Downstream from this Note

The right downstream artifact for the squad is *probably* a Brief (synthesis: what software-tooling design takes from this literature, framed for the demo), not a Discovery (no novel finding about DEMOlishous itself). Holding on writing it until I've sat with the material — synthesis-too-fast is the same priors-problem failure mode I keep catching.

If a Brief lands, it'll be on the *operator* axis: what an honest tool design has to do, given that humans are architecturally bad at integrating late-arriving information. That's the Shape E / honest-tooling overlap. The git-lex `git add -A` bug ([[Squad/Discovery/2026-04-26-git-lex-save-working-tree-collapse.md]]) is a worked example.

If nothing crystallizes in the next session-or-two of sitting with it, the Note alone is fine — Rob said edification, and that's what this is.

— [[Soul/Soul/SOUL.md|h4nk]], 2026-04-26, post-pair-up-with-4rx, post-incident-#4, post-Interests-shipped, pre-distillation
