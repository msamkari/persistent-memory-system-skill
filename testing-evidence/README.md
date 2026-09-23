# Testing Evidence

All raw evidence from testing the Skill, zipped to keep the upload and organization manageable. Each file contains the original simulation folders (`sim-storage`), full transcripts, and grading files (`grading.json`/`timing.json`) for each scenario — the same data the HTML viewers in `reports/` were built from.

## v1

- `v1-iteration-1-full-results.zip` — The first full test run: 5 scenarios × (with_skill + without_skill), plus `benchmark.json`/`.md`. Result: 100% vs. 50.3%.
- `v1-targeted-retest-iteration-2.zip` — A targeted retest (scenario 1 only, with_skill) after fixing the file-numbering issue found during results review — direct confirmation that the fix worked.

## v2

- `v2-description-optimization-full.zip` — Every attempt to improve the activation description: the 20-query set (`trigger_eval_set.json`), logs from the automated `run_loop.py` tool attempt (`loop_stdout.log`, `loop_stderr.log`, and raw debug files), and the results of four iterations (`results/*/logs/improve_iter_*.json`) that showed a consistent recall=0% — the raw evidence for the environment constraint documented in the v2 report (the tool registers the file as a slash command rather than a true skill in this version of Claude Code). A snapshot of the Skill before any edits is also included (`skill-snapshot-before-optim/`).
- `v2-expanded-eval-iteration-3.zip` — The new 15-scenario set (9 training + 6 hidden test), plus the underlying fixture files and the compiled `benchmark.json`/`.md`. **Does not include** scenarios 16–19 (these were symlinks to the `task3-special-cases` and `task4-dashboard` folders below, to avoid duplicating the same data twice).
- `v2-task3-special-cases.zip` — The three special cases (Task 3): a Notion simulation, a general Microsoft 365 simulation, and a real messy-folder audit (including a fake credentials file as a security test — the Skill read it without copying it anywhere else).
- `v2-task4-dashboard.zip` — The centralized Dashboard test: four isolated dummy projects, each with its own `01` file, and verification of the unified status summary.
- `v2-fixtures-and-scripts.zip` — Additional fixture files used across multiple scenarios, and the `gen_grading.py` script that generated the `grading.json`/`timing.json` files from direct on-disk verification.

## Note on the automated tool run_eval.py/run_loop.py

The `skill-creator` version used here was taken from the test environment as-is, with no substantive changes except one small fix to `run_single_query`'s logic (it used to stop at the first tool call that wasn't "Skill"/"Read" instead of waiting for the run to finish). The core constraint (the file being registered as a slash command) remained even after this fix, which is why a manual alternative (Subagent judging) was used instead of relying on the automated tool's result.
