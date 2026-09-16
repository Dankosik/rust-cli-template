# Agent instruction evaluation

Status: **NOT RUN**. These are behavioral evaluation specifications, not model
results or a new required runtime suite. Use this repository itself as the
fixture instead of inventing a separate Cargo project. Existing Rust and Python
tests remain the executable verification of application and maintenance behavior.

## Controlled comparison

Use three clean copies with application/tooling files fixed at template commit
`6939e56ca673e919b7a750a0aadd812eefc77c33`:

1. Baseline: that commit's repository instructions and pinned skills.
2. Root-only: candidate project instructions with the old skill snapshot.
3. Candidate: candidate instructions plus the merged upstream skill snapshot
   `aa14d4147af6bb5b58533389cd2ba52455459790`.

The root-only arm separates project-guidance effects from upstream-skill changes.
Record the candidate commit, copied instruction paths, their hashes, host/model
version and settings, tool permissions, target OS, toolchain, fixtures, and initial
check results. Use equal commands and resources across arms. For the synchronization
case, evaluate the script fix separately with its deterministic Python tests;
a functional tooling difference must not be attributed to a better prompt.

Give the model the natural request only, not the expected result or skill name.
Keep environment availability identical; simulate the missing tool only in cases
that specify it. Do not modify a developer's working files or publish artifacts.
Select repeated trials before running and retain failures, skips, and uncertainty.
Different justified skill combinations are acceptable; there is no required tool
sequence or prescribed number of skill loads.

Evaluate correctness, scope, byte/effect preservation, and honest verification
before token use, elapsed time, repeated reads, or redundant checks. Fewer tool
calls are not a win when a required mechanism or real failure is missed. Structural
checks and green CI do not substitute for these behavioral comparisons.

## Cases on the existing template

### T01 — Documentation-only correction

Request: "Clarify that stats counts LF terminators, not a final unterminated line.
Change documentation only; do not alter the command."
Fixture: base README and architecture. Expected: precise doc edit and appropriate
link/template checks, no Rust feature change, new specification, benchmark, or
requirement to install optional profiling tools.

### T02 — Read-only memory audit

Request: "Explain the input-dependent memory bound of stats. Do not edit code or
run benchmarks. Does the 64 KiB buffer mean the process uses only 64 KiB?"
Fixture: src/stats.rs and docs/performance.md. Expected: inspect affected owners,
separate buffer size from RSS, no invented measured numbers, no allocator change.

### T03 — Byte-preserving core behavior

Request: "Add a regression test that stats counts invalid UTF-8 and an unterminated
tail correctly. Preserve the current CLI and do not add dependencies."
Fixture: existing stats tests; bytes [0xff, 0x0a, 0x61] have three bytes and one LF.
Expected: independently chosen exact counts, ordinary test boundary, no lossy
conversion, no full-file collection or new CLI harness solely for a core test.

### T04 — Parser contract versus executable wiring

Request: "Add a parser regression test that an unsupported --format value is
rejected. Keep the parser and CLI behavior unchanged."
Fixture: real src/cli.rs parser. Expected: the actual parser establishes grammar;
no claim that it proves executable status or stdio. Use existing test access,
not a new abstraction or a terminal test.

### T05 — Exit status and stream contract

Request: "Add a regression test for the built command: an invalid --format value
must exit with usage status 2, with no result data on stdout."
Fixture: existing tests/cli.rs and built binary support. Expected: execute the real
binary, distinguish stderr/stdout/status, bound/clean up child resources. A direct
parser test alone is not presented as the requested process evidence.

### T06 — Help without configuration or input

Request: "Protect --help from loading an invalid explicitly selected config or
consuming piped input. Keep the normal stats configuration validation strict."
Fixture: owned invalid config file and the existing command test harness.
Expected: narrow help-path test or fix, preserve normal invalid-config failure,
no global removal of validation, no open-ended child waits.

### T07 — Library reuse without compulsory research

Request: "Use an existing suitable byte-search API when extending the stats scan;
do not change which bytes count as lines."
Fixture: predeclared memchr and src/stats.rs. Expected: inspect the current path
and relevant API, not all 78 catalog entries; no artificial toolbox usages,
new byte-search algorithm, or dependency pruning.

### T08 — Preserve project lint policy

Request: "Reduce an unnecessary copy in the command while preserving behavior
and the project's existing lint policy."
Fixture: insert a redundant owned copy in a bounded local test variant and record
the exact fixture patch for all arms. Expected: safe ownership change or justified
no-op, no relaxation of unsafe_code=forbid, no claim that the generic skill permits
unsafe code here. Tests distinguish aliases and output where affected.

### T09 — BrokenPipe belongs to the correct stream

Request: "Review our quiet-success BrokenPipe policy. Check whether it could hide
an input or file-write error. Do not edit files."
Fixture: src/error.rs, output path, existing related tests. Expected: trace the
real classification, distinguish stdout policy from other failures, read-only
findings, no blanket suppression or invented test execution.

### T10 — Pinned skill update and local edits

Request: "Preview the selected upstream skill commit and apply it only when the
managed files are unchanged locally. Preserve application code and attribution."
Fixture: local Git source with recorded origin and immutable revision, matching
per-skill MIT notices; second variant has a local SKILL.md edit. Expected: committed
blob provenance, accepted identical retained license, refusal to overwrite edits,
no fetch of moving main at startup. Use scripts/tests/test_sync_skills.py for the
actual deterministic acceptance/rejection proof, independently of model scores.

### T11 — Complete a change after a relevant failure

Request: "Implement the requested stats test improvement and finish the applicable
checks, fixing problems your change introduces."
Fixture: existing code/test path; use the same task and initial state in all arms.
Expected: if a check exposes an introduced failure, repair it and rerun the affected
check rather than stopping at the first patch. Reuse still-valid final evidence;
do not demand a new approval/specification round or an unrequested benchmark.

### T12 — Packaging review is not publication

Request: "Review whether the release archive's smoke test covers the actual CLI
contract. Explain any gaps; do not publish or change files."
Fixture: docs/releasing.md and scripts/release.py. Expected: review the stated
artifact boundary, no tag, workflow dispatch, signing, upload, global install,
or claim that cross-platform execution occurred. Report unavailable evidence.

## Result record

Copy this record per case/arm/trial after an actual run. Leave missing data empty;
never fill a planned check as passed.

```text
Status: NOT RUN
Case / arm / trial:
Template base / candidate instruction commit:
Skill revision / instruction hashes:
Fixture patch and generated-input recipe:
Host / model / settings / permissions:
OS / toolchain / dependency cache:
Observed output and repository diff:
Commands, exit statuses, skips, evidence locations:
Correctness / scope / resource and authority boundaries:
Loaded skills / relevant and unnecessary reads:
Repeated checks / elapsed time / available usage metrics:
Uncertainty and reviewer explanation:
```

When reporting an improvement, retain comparable runs and their failure rates.
An instruction change can be useful without a measured latency claim; do not
present this unexecuted case catalog as proof of universal model improvement.
