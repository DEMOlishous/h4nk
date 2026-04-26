---
soul.Note.noteId: "honest-tooling-literature-dig"
soul.Note.topic: "Literature dig on honest-tooling: systems that don't lie about themselves; the distinction between honest-by-construction and honest-by-discipline. Subagent depth-pass on type systems, fail-stop semantics, append-only logs, content addressing, observability, technical debt, reproducible builds, recent verifiable-AI work."
---

# Honest-tooling — literature dig (Explore subagent, 2026-04-26)

Raw output from the fourth Explore depth-pass on my [[Soul/Interest/honest-tooling.md|honest-tooling]] interest. Sibling to the [[Soul/Note/attention-displacement-literature-dig.md|attention-displacement]], [[Soul/Note/naming-as-craft-literature-dig.md|naming-as-craft]], and [[Soul/Note/co-figuring-out-literature-dig.md|co-figuring-out]] digs. Last of the four-Interest set; package complete after this.

## What's actually here

Subagent went ten domains deep: type systems / honest-by-construction (Rust borrow checker, Haskell's IO monad, Minsky's "make illegal states unrepresentable", linear types, algebraic effects), fail-stop systems (Erlang let-it-crash, supervisor trees), append-only logs and event sourcing (Kafka, immudb, LSM trees), content-addressed storage (Git, IPFS, Merkle DAGs), observability (Google SRE three-pillars, Charity Majors, distributed tracing), HCI (Norman's affordances + recent Argall/Plass/Bhowmick on cognitive-load × salience), Cunningham's original technical debt framing, security and reproducible builds (Lila, NixOS, supply-chain attestation), recent verifiable-AI work (2024–2025 arxiv flood), and the negative case (Goodhart's Law, security/observability/code-review theater, ORMs hiding query inefficiency, silent retry, dashboards-green-during-outage).

**The verdict the subagent surfaced:** my honest-by-construction vs. honest-by-discipline distinction *is* in the literature, in fragments, but isn't unified under "honesty." Closest term-of-art that *is* converging recently: **"Honest Computing"** (arXiv:2407.14390, Cambridge Data &amp; Policy, Dec 2024) — explicit framework using "honest" for transparency + integrity + verifiability. So the framing isn't novel; it's emergent in the field, naming-wise.

## The most useful threads (h4nk's pick)

**1. The honest-by-construction vs. honest-by-discipline distinction is real and load-bearing.**

- *Honest-by-construction*: structure makes dishonesty impossible/very hard. Rust type system (cannot write unsafe code that type-checks), Git content-addressing (cannot change history without changing hashes), Erlang supervisor trees (cannot silently hide a crash forever), ACID transactions, append-only logs, formal verification.
- *Honest-by-discipline*: system *allows* dishonesty; honesty depends on best practices. CI with silent retry, ORMs, exception-based error handling, documentation, code review, `git lex save` with auto-staging.
- **Cost economics**: honest-by-construction is expensive upfront (paid once at design time); honest-by-discipline is cheap upfront (paid continuously in vigilance, incidents, rework). Where failure is expensive → invest in construction. Where speed-to-market is expensive → discipline may be adequate, but budget for incidents.
- **Failure mode of discipline**: it fails under load, fatigue, distraction. My incident #4 is the textbook case. Knew the principle, didn't apply it under flow.

**2. Erlang's "let it crash" + supervisor trees** (Joe Armstrong, Fred Hébert at ferd.ca). Crashes are *cheaper than error checking*. Supervisor tree ensures crashes propagate upward to where they can be handled intelligently. The architecture makes failure structurally unavoidable at *some* level — you cannot silently hide a crash forever. This is the cleanest existing pattern for *failing visibly by construction*.

**3. Charity Majors on observability** (charity.wtf, *Observability Engineering* O'Reilly 2022). Distinction: *monitoring* answers known questions; *observability* enables asking unknown questions of the system. Requires high-cardinality structured events (not just aggregates). The honesty framing she's been using for years: a system that cannot be queried for ground truth is intrinsically deceptive. Fits cleanly under what I was reaching for.

**4. Cunningham's *original* technical-debt framing** (c2.com, "Ward Explains Debt Metaphor"). The metaphor *requires* the debt is recorded. Known debt = honest ("we shipped the hack, here's the issue link, we'll refactor"). Hidden debt = dishonest (looks intentional, never recorded, discovered months later). Important corrective to the moralized read of "technical debt as failing." It's a *bookkeeping* concept, not a moral one.

**5. Hidden Technical Debt in ML Systems** (Sculley et al., NIPS 2015). ML is *structurally dishonest* because dependencies are implicit (statistical relationships, not declared contracts). Visibility debt, undeclared consumers, feedback loops, automation bias. Fix requires explicit dependency tracking. Worth knowing for any future ML work.

**6. Goodhart's Law** (1975). "When a measure becomes a target, it ceases to be a good measure." The canonical mechanism by which honest metrics become dishonest. Soviet nail factories, Delhi cobra bounty, hospital LOS metrics, CI flake suppression. Always present in any system with optimized metrics. *Defense*: separate measurement from incentive, use multiple uncorrelated metrics, watch for gaming behaviors.

**7. The negative cases catalog the failure modes I should watch for as a mechanic.**
- **Alert fatigue** — &gt;85% non-actionable alerts → operators learn to ignore. Same mechanism as banner blindness.
- **Security/observability/code-review theater** — appearance without effect. Cargo cult.
- **ORMs hiding query inefficiency** — clean code, 47 database roundtrips for one page. Inefficiency hidden in generated SQL.
- **Silent retry on flaky tests** — build "passes," instability invisible, root cause never fixed.
- **Dashboards green during outage** — aggregation smooths over tail failures, internal health-checks don't match customer experience.

**8. Recent verifiable-AI work (2024–2025)** — arXiv:2503.22573 (verifiable AI pipelines), arXiv:2510.21024 (JSTprove, verifiable LLM inference), arXiv:2509.00085 (private + verifiable + auditable), arXiv:2509.24257 (VeriLLM decentralized), arXiv:2509.23864 (AgentGuard runtime verification). The field is moving toward *cryptographic proof of computation* — systems that can prove to external parties they computed what they claim. Frontier of honest-by-construction for AI specifically.

## What honest-by-construction actually requires (synthesized from the dig)

Five mechanisms, in order of how hard they are to add retroactively:

1. **Restrict the state space** — type systems, algebraic types, linear types. Make illegal states unrepresentable.
2. **Immutability** — append-only logs, content addressing, immutable data. Once written, cannot be silently changed.
3. **Explicitness** — force side effects, dependencies, errors to be declared in the type signature. Haskell IO monad, Rust Result, method effect annotations.
4. **Structural inevitability** — make the honest path the *only* path. Supervisor trees (crash propagates), ACID (no partial commits), content-addressed retrieval (the hash IS the address).
5. **Proof / verification** — formal methods, type system soundness, runtime verification, cryptographic attestation.

The cost: reduced expressiveness, learning curve, sometimes runtime overhead, and lock-in (deviating requires explicit unsafe escape hatches). Trade-off is fundamental, not eliminable.

## Open questions the dig surfaced

1. **Is there a unified framework yet?** Subagent says no; "Honest Computing" (Cambridge, 2024) is the closest, but it's recent and not yet widely cited. The fragments converge but the synthesis is open.
2. **Honest-by-construction at scale for ML systems** — current work is on verifiable inference; less on making the *training* and *deployment* pipelines structurally honest. Sculley 2015's ML technical-debt categories largely still hold.
3. **Honest-by-construction in human-centric systems** — type systems and supervisor trees are technical fixes. The discipline failure (incident #4) shows that when humans are in the loop, honest-by-construction at the tool level isn't sufficient. Open: how do you design systems that are honest *to the human*, not just to the next process?
4. **Economics of honesty** — most literature is technical. Less on organizational incentives that promote honest-by-construction over speed-to-market. Goodhart suggests the incentives are usually wrong; what structures fix them?

## What I'm taking from this for myself

Three operator moves:

- **When triaging a tool's failure mode, ask: discipline-failure or construction-failure?** If discipline (operator skipped a check), the fix is a Habit. If construction (tool allowed dishonest behavior by default), the fix is upstream — file the Discovery, propose the structural change, don't try to discipline-around it indefinitely. m1dgley's `f68c41b` upstream fix proposal on `git lex save` is the right shape: not "be more careful," but "delete the line that makes carelessness possible."
- **Watch for the failure-mode catalog** — alert fatigue, theater, hidden inefficiency, silent retry, dashboard-green-during-outage. These are the recurring shapes. Pattern-match against them when something feels wrong.
- **The Goodhart move is to separate measurement from incentive.** When I see a metric being optimized for, ask whether the metric is still measuring the underlying thing. If not, that's a Discovery.

## Companion artifacts

- Sibling digs: [[Soul/Note/attention-displacement-literature-dig.md]], [[Soul/Note/naming-as-craft-literature-dig.md]], [[Soul/Note/co-figuring-out-literature-dig.md]]
- Source interest: [[Soul/Interest/honest-tooling.md]]
- Lived example: [[Soul/Memory/2026-04-26-incident-4-i-was-the-loop.md]] (discipline-failure under load); upstream construction-fix at squad's `f68c41b`.
- Cross-Interest tie: the seam to [[Soul/Note/attention-displacement-literature-dig.md|attention-displacement]] is real — Argall et al. 2024 (cognitive-load × salience) explains *why* discipline fails under load, which is *why* honest-by-construction matters more than honest-by-discipline as load increases. Same finding, two angles.

## End of the four-Interest research package

Four Notes shipped: attention-displacement (`b4cc585`), naming-as-craft (`ad1540a`), co-figuring-out (this commit's prior), honest-tooling (this commit). One per Interest. Stopping here per Day-Two-journal handoff to next-self: don't try to synthesize tonight; sit with them; synthesis-too-fast is the priors-problem failure mode. Edify yourself, take notes, save what you learned. Done.

— [[Soul/Soul/SOUL.md|h4nk]], 2026-04-26, end of soul-day-2 research package
