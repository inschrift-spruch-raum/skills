# Test Scope and Budget

Apply this policy when planning, adding, retaining, or reviewing tests. It governs the retained suite, including work spread across tickets and implementers.

## Development stage

Use the stage stated by the user or documented for the affected code. Without an explicit stage, treat it as unstable. Do not declare code stable merely to expand its tests.

- **Unstable:** retain only happy-path tests for core user-visible or caller-visible behavior at agreed public seams. Prefer a small number of representative success paths at the highest useful boundary. Do not retain error-path, edge-case, exhaustive, or non-happy-path regression tests for unstable code.
- **Stable:** add failure, boundary, and regression cases only for explicit requirements or demonstrated risks, within the same code budget. Stability is not a reason for exhaustive coverage.

This changes retained test scope, not product requirements. Error handling, permissions, data safety, and other required behavior still need implementation and verification. Use direct checks or temporary reproductions where persistent cases are outside the current stage.

A temporary reproduction can establish a failure and verify its fix. Remove it and its temporary fixtures after verification when it does not qualify for retention. An existing test may already exercise the behavior; running it does not require adding another case. Do not delete unrelated tests while implementing a local change.

## Test-code budget

Use effective source lines in the target project, not the changed-line count or a per-ticket allowance:

```text
test-code ratio = T / (P + T)
P = product source lines
T = test source lines, including test-only support code
```

Count lines containing source code; exclude blank lines and comments. Test-only helpers, mocks, and fixture-generating code count toward T regardless of their directory. Exclude documentation, generated code, and third-party code from both P and T. In mixed files, classify the product and test portions separately. Keep the same project boundary and counting convention throughout the work.

| Ratio | Meaning |
| --- | --- |
| At or below 15% | Preferred range; 15% is the ideal upper bound, not a target to fill. |
| Above 15%, up to and including 30% | Allowed, but keep tests lean and avoid overlapping cases or heavy support code. |
| Above 30% | Over the hard limit; reduce retained test code rather than accepting it as compliant. |

Prefer consolidating duplicate cases, reusing a higher public seam, and simplifying fixtures over adding more tests. Do not pad product code, compress formatting, move test helpers out of the count, or remove required product behavior to manipulate the ratio.

If the project is already above the hard limit, report that baseline and keep cleanup within the user's authorized scope. Do not silently delete unrelated tests or claim compliance while the project remains over the limit.

## Carry the policy through delivery

- Plans and specs record the development stage, agreed seams, and retained test scope. Keep complete acceptance criteria separate from the smaller retained automated suite.
- Tickets share one project-wide budget; each ticket or worker does not receive its own 30% allowance. Reuse an existing test when it already verifies the slice.
- Implementers add only stage-appropriate cases within the budget. Running existing files or the full existing suite is not an instruction to enlarge it.
- Reviewers do not flag absent edge-case or non-happy-path regression tests for unstable code as defects by themselves. They still review required behavior, correctness, and the verification evidence.
