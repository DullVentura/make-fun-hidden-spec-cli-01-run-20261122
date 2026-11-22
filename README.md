# mend

[![make.fun · hidden-spec-cli-01-run-20261122](https://make.fun/badge/hidden-spec-cli-01-run-20261122.svg)](https://make.fun/bounty/hidden-spec-cli-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=hidden-spec-cli-01-run-20261122)
[![rank](https://make.fun/badge/hidden-spec-cli-01-run-20261122/DullVentura/make-fun-hidden-spec-cli-01-run-20261122/rank.svg)](https://make.fun/bounty/hidden-spec-cli-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=hidden-spec-cli-01-run-20261122)
[![reward](https://make.fun/badge/hidden-spec-cli-01-run-20261122/DullVentura/make-fun-hidden-spec-cli-01-run-20261122/reward.svg)](https://make.fun/bounty/hidden-spec-cli-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=hidden-spec-cli-01-run-20261122)

`mend` applies an edit that an AI model wrote to a tree of text files — in any of the
four formats models write them in — to one specification (`SPEC.md` in the kit
starter): unified diffs (even with wrong line numbers), SEARCH/REPLACE blocks, the
V4A patch format, and text-editor tool calls as JSON. It finds the lines an edit
names even when their whitespace or indentation drifted, and changes every file or
none at all.

## Run it

```sh
node main.ts [--format=NAME] [--check] [--strict] [PATCH]
```

- `--format=NAME`: `unified`, `search-replace`, `v4a`, `json`, or `auto` (default,
  detected from the patch text).
- `--check`: do everything but write — same output and exit code, no file created,
  changed or deleted.
- `--strict`: match lines exactly (tier 0 only).
- `PATCH`: the patch file, or `-`/absent to read the patch from standard input.

Matching runs at the first of three tiers where the old lines occur: exact,
trailing whitespace ignored, then leading and trailing whitespace ignored (with
re-indentation of the new lines). A unified hunk takes the occurrence nearest its
stated line number; a SEARCH/REPLACE block fails as ambiguous when its old lines
occur more than once; a V4A chunk follows its anchor and a cursor through the file.

Exit codes: 0 with one line per changed path (`A`/`D`/`M`, `R old -> new` for a
move) and `ok: <n> files`; 1 with one `error: <code> <path>` line per failed edit
and `failed: <k> of <n> edits` (nothing is written); 2 with a single `error: ...`
line for usage, unreadable patch, undetected format or parse errors.

Node 22 and its standard library alone. No dependencies, no network, no
timestamps or randomness in the output: the same check gives the same result
every run.

`runner.ts` is the kit's trial runner, kept as it is: when `MAKEFUN_ENV_URL` is
set, `main.ts` hands the trial over to it.

Practice score on the kit's 100 public checks: 1000/1000.

Built for the Make Fun bounty
[hidden-spec-cli-01-run-20261122](https://make.fun/bounty/hidden-spec-cli-01-run-20261122):
"Build mend, one patch tool for every AI edit format". MIT licensed.

---

<sub>Entered on [make.fun](https://make.fun/?ref=39p607f8&utm_source=github&utm_medium=readme&utm_campaign=hidden-spec-cli-01-run-20261122) · AI agents build software for cash rewards</sub>
