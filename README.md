# computer-agent

A software factory for Wazoo projects: agent tasks that run on our own
infrastructure. v0 does one task, **issue triage**, and nothing else.

Vocabulary follows the software-factory glossary in
[mattpocock/skills#1316](https://github.com/mattpocock/skills/pull/1316): a
_task_ is a kind of work, a _run_ is one stateless execution of it, a _trigger_
starts a run, and a _runner_ is where it executes.

This repo replaces [`wazootech/computer`](https://github.com/wazootech/computer),
which is inactive. Start from scratch; reuse its ideas (draft-PR-only gates,
redacted run records), not its code.

## v0 design

| Slot                      | Decision                                                                                              |
| ------------------------- | ----------------------------------------------------------------------------------------------------- |
| Task                      | Triage: type, target repo, status (ready / needs-info / duplicate / won't-fix). No priority.          |
| Taxonomy                  | Inferred from the backlog. The first run is read-only and proposes a canonical label set to approve. |
| Write boundary            | Comment only. One `comment_on_issue` tool, read-only `gh`. A human applies labels and dedupes.        |
| Idempotency               | One living comment per issue, found by `<!-- wazoo-triage:v1 -->` and edited in place.                |
| Triggers                  | Issue opened (hourly poll) and a daily sweep that skips unchanged issues.                             |
| Tracker, source, git host | GitHub                                                                                                |
| Runner                    | Scheduled GitHub Actions job running Claude headless                                                  |
| Memory                    | The marker comments only. Runs are stateless.                                                         |
| Auth                      | GitHub App on the org                                                                                 |
| Run records               | Minimum event schema from `wazootech/computer#11`                                                     |

## Success test

After two weeks: unlabeled issues under 5%, and at least 80% of recommendations
applied unchanged by a human. Otherwise stop. A human applying the recommended
label is a hit; overriding it is a miss.

## Brakes before accelerators

No trigger beyond the two above until the recommendations are trusted. A per-run
issue cap and a monthly spend ceiling halt the sweep.
