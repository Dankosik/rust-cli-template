# Working with agents

[AGENTS.md](../AGENTS.md) owns repository rules. This is optional navigation for
work that needs more context, not a second mandatory workflow. Small tasks can
proceed from the request and affected implementation directly to a checked change.

## Find the relevant context

| Current decision | Read when needed |
| --- | --- |
| Add or replace a command | [First command](first-command.md), affected parser/dispatch/tests |
| Understand existing ownership or public behavior | [Architecture](architecture.md) and the affected callers |
| Reuse a technical mechanism or select a dependency | Matching [library-guide](library-guide.md) row; selected API documentation only |
| Measure a resource claim | [Performance](performance.md); an audit without measurements can instead report supported bounds and hypotheses |
| Packaging or authorized release | [Releasing](releasing.md); preparation does not itself authorize publication |
| Coordinate work across sessions or owners | [Task notes](../specs/README.md), only when shared decisions need persistence |
| Change agent instructions or the skill snapshot | Source pin below and [behavioral evaluation](agent-evaluation.md) |

Do not read every document in the table. Research catalogs explain past choices;
they are not a prerequisite for ordinary implementation or a reason to upgrade
libraries. Use the current Cargo graph and selected API when a version matters.

## Select a technical skill

Descriptions in `.agents/skills/` are the activation surface. The following map
is only a fallback when the host does not expose them. Load a specialist for an
actual decision, not as a mandatory implementation stage; useful combinations
are allowed without requiring every neighbor.

| Decision | Skill |
| --- | --- |
| Implement requested behavior | [rust-implement](../.agents/skills/rust-implement/SKILL.md) |
| Representation, borrowing, traits, transformations | [rust-idiomatic](../.agents/skills/rust-idiomatic/SKILL.md) |
| Responsibilities and useful module boundaries | [rust-design](../.agents/skills/rust-design/SKILL.md) |
| Grammar, configuration precedence, terminal/output interface | [rust-cli-interface](../.agents/skills/rust-cli-interface/SKILL.md) |
| Error distinctions, diagnostics, partial success, exit status | [rust-errors](../.agents/skills/rust-errors/SKILL.md) |
| Byte flow, buffering, record boundaries, fallible I/O | [rust-io](../.agents/skills/rust-io/SKILL.md) |
| File identity, traversal, replacement, preservation | [rust-filesystem](../.agents/skills/rust-filesystem/SKILL.md) |
| Child arguments, pipes, termination, reaping | [rust-processes](../.agents/skills/rust-processes/SKILL.md) |
| Admission, task ownership, cancellation, joining | [rust-concurrency](../.agents/skills/rust-concurrency/SKILL.md) |
| Retained owners, growing buffers, memory bounds | [rust-memory](../.agents/skills/rust-memory/SKILL.md) |
| Workload timing, profiling, benchmark comparisons | [rust-performance](../.agents/skills/rust-performance/SKILL.md) |
| An uncertain defect or causal explanation | [rust-debugging](../.agents/skills/rust-debugging/SKILL.md) |
| Function/parser/I/O-double behavior | [rust-testing](../.agents/skills/rust-testing/SKILL.md) |
| Executable wiring, status, real pipes or terminals | [rust-cli-testing](../.agents/skills/rust-cli-testing/SKILL.md) |
| Cargo resolution, features, toolchain, build profile | [rust-build](../.agents/skills/rust-build/SKILL.md) |
| Artifact compatibility and delivery | [rust-distribution](../.agents/skills/rust-distribution/SKILL.md) |

Project contracts constrain reusable advice. In particular, preserve Cargo's
unsafe-code prohibition and the predeclared toolbox. Do not edit vendored text
to encode template-specific policy; keep that policy in maintained repository
instructions. An explicit request to change a project decision may revise it,
but does not reopen unrelated choices.

## Implement, review, or diagnose the requested result

For a command change, identify input, success output, expected errors, status,
and file/process effects on the affected path. A refactor preserves those
contracts. Review and diagnosis need not modify files; benchmarking need not
change an implementation. Do not make missing optional profiler data a blocker
for a code-only audit or for implementing an agreed logical resource bound.

Use the verification boundary from AGENTS.md and the commands in the Makefile.
A real parser call can establish grammar, but not executable wiring or status.
A Read/Write double can establish error propagation, but not OS pipe behavior.
Existing process tests should remain real where their contract crosses that
boundary. Unavailable evidence stays explicitly unverified.

For a straightforward implementation, repair failures it introduces and finish
without a new review-approval loop. Reuse relevant checks already completed for
the same revision and environment; a handoff is not a reason to run them again.

## Larger work and delegation

Use a short note under `specs/<topic>/` only when decisions must survive sessions
or multiple owners need the same contract. Record outcome, accepted choices,
open questions, dependencies, and completion evidence. Do not create a spec just
to edit a command. Respect an explicitly limited research/design/planning task.

Delegate concrete independent work when tools are available and it saves time.
Give each worker the outcome, relevant files, settled decisions, writable scope,
constraints, and expected evidence. Keep writers on disjoint files; sequence
shared edits and build resources. Read-only reviewers may share the checkout.
Do not simulate delegation when no subagent facility is available.

The coordinator integrates results, checks assembled behavior, and resolves
conflicting findings against the code. A worker's report is evidence to inspect,
not proof of integration. Run only final checks made necessary by the assembled
change; no mandatory review round at every boundary.

A useful handoff records outcome, decisions, files changed, commands/results,
and next action or blocker. Distinguish local verification, CI, and publication.

## Update the pinned skills deliberately

[skills-source.json](../.agents/skills-source.json) owns the upstream URL, immutable
commit, and SHA-256 inventory. The skills are available offline; startup must not
fetch a moving branch. Update from a reviewed local checkout with a full commit:

```sh
python3 scripts/sync_skills.py --source ../rust-cli-skills --revision FULL_COMMIT_SHA --check
python3 scripts/sync_skills.py --source ../rust-cli-skills --revision FULL_COMMIT_SHA --apply
make template-check
```

Preview exits 1 for drift, 0 for no change, and 2 for invalid input. It writes
nothing. Apply reads committed blobs, refuses locally changed skills or inventory,
and updates the skill contents and pin together. It does not modify application
code. Review activation/scope changes as behavior changes, not cosmetic edits.

The local inventory intentionally contains SKILL.md only. A per-skill upstream
LICENSE is omitted only when its exact bytes are already preserved in the root
LICENSE. A different or missing retained notice requires explicit reconciliation;
unknown resources, non-regular files, and orphan license directories are rejected.
Keep [third-party notices](../THIRD_PARTY_NOTICES.md) accurate.

Skill-only updates need template checks, not an invented Rust workload. Run the
additional applicable checks when code, build inputs, or initialization changes.
CI gates remain unchanged. Structural checks do not establish model behavior;
use selected evaluation cases when assessing instruction effectiveness.
