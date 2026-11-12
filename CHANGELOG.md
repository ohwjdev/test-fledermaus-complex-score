# Changelog

All notable changes to the campaign-template. Commit hashes are given in
parentheses.

## 2026-11-10

- The README and config.example.yaml describe the current pre-tasks and settings.
- The README says that GitHub may delay the hourly run by hours.

## 2026-10-20

- config.example.yaml caps the claims one person may hold at once at ten.

## 2026-10-06

- config.example.yaml sets claims to last 24 hours.

## 2026-10-05

- The `automation.ref` comment in config.example.yaml names the instance branch.
- `tracking/lock.csv` gains an `expires` column.

## 2026-10-02

- `tracking/comment.csv` gains a `fragment` column for campaign comments anchored to a measure.

## 2026-09-29

- README documents `sources/<piece-id>/layout-corrected.json`, the corrected layout boxes of an OMR piece.

## 2026-09-28

- config.example.yaml and the changelog name the renamed repositories.
- References follow the repository rename of instigation to lets-encode.
- Unused `templates/score.template.mei` removed.
- README documents which template files a new campaign replaces or drops.
- Caller: reduced to triggers and a call to the campaign workflow in instigation.
- Caller: runs for pull requests that change no file, so an unchanged encoding can be submitted.

## 2026-09-25

- README documents `sources/<piece-id>/omr.xml`, the recognition record an OMR piece's score setup writes.

## 2026-09-23

- history.csv gains a `pr` column naming the pull request each row decides.

## 0.4.5 – 2026-09-21

- README: `sources/<piece-id>/layout.json` documented — the layout model's raw, uncorrected output of an OMR-prepared piece, written with its layout correction.

## 0.4.4 – 2026-09-03

- Caller: one concurrency group per pull request, so a run is never cancelled because another PR's run is waiting; a push to the same PR waits for its running run and replaces a pending one.
- Caller: the central pointer read from config.yaml is validated (owner/repo, ref pattern, allowlisted entry point) and passed to the run step as a quoted environment variable.
- Caller: actions/checkout pinned to v5.1.0 (Node 24); the setup-node fallback is gone — a runner without Node 22.18 fails loudly.
- Caller: pull requests that change more than two files, are drafts, or come from non-user accounts get no run; the console closes its own such pull request with an explanation, the scheduled catch-up pass closes the rest. A draft marked ready for review now triggers a run.
- Template score: MEI 5.1 (`meiversion` and the `xml-model` schema reference).
- Caller: the xmllint install, cache and set-up steps are gone — the MEI machine-check runs inside the coordinator. Three steps remain: read the pointer, check out central, run it.

## 0.4.3 – 2026-08-28

- Added this changelog, reconstructed from the full git history.

## 0.4.2 – 2026-08-14

- Updated the caller workflow and the config example. (`62b3bed`, `c40b9b0`)

## 0.4.1 – 2026-08-13

- GitHub Actions speed improvements. (`73de78d`)

## 0.4.0 – 2026-08-11

- Better caching of xmllint and improved progress reporting; caller updated to optimize the xmllint step. (`bf10ce2`, `2bed414`)

## 0.3.3 – 2026-08-10

- Downgraded to Node 22. (`fc63650`)

## 0.3.2 – 2026-08-06

- Codebase cleanup with added comments. (`d02f629`)

## 0.3.1 – 2026-07-29

- Sped up GitHub Actions calls. (`2747fc7`)

## 0.3.0 – 2026-07-21

- Switched to GitHub numerical ids for repos and users. (`d5e395b`)

## 0.2.2 – 2026-07-17

- Security hardening: pinned to specific action versions. (`242f7ec`)

## 0.2.1 – 2026-07-13

- Updated the tracking tables. (`3e0ba56`)

## 0.2.0 – 2026-07-02/03

- Switched to a more generic workflow; removed the default task. (`cefc5f5`, `6e0a0fd`)
- Moved to four tracking tables. (`ecd61dc`)
- Added a test JPG to the template, moved it to the source folder, then removed the unneeded test file. (`7382e78`, `10fd204`, `3f2e4ea`)

## 0.1.1 – 2026-06-30

- Simplified and shortened comments; updated Node. (`3217780`, `6de768d`)

## 0.1.0 – 2026-06-23/26

- Initial commit and first files. (`0b39ddc`, `c1abfa1`)
- Added GitHub Actions to claim tasks, submit, and reap stale claims. (`84383eb`, `27d6713`)
