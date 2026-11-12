# Let's Encode! — campaign template

This is the **template repository** every campaign is stamped from (via GitHub's
"create a repository from a template"). A copy of it becomes one crowd-encoding
campaign: it holds the score sources, the campaign configuration, five
machine-maintained tracking tables, and one generic caller workflow that runs
the campaign automation.

> You normally don't touch these files by hand. The Let's Encode app fills
> in the configuration at campaign start, and the volunteer client claims and
> submits work through pull requests. The files are kept human-readable so they
> review cleanly in pull-request diffs.

## Layout

```
README.md                    # this file; replaced at init by the campaign's own README
CHANGELOG.md                 # changes to this template; removed at init
config.example.yaml          # campaign config TEMPLATE (schema v3) — documented inline; removed at init
config.yaml                  # written at campaign creation by the app (not in the template)
sources/
  img/                       # the source's committed page images (facsimile campaigns)
  <piece-id>/score.mei       # one MEI per piece, written at init (not in the template)
  <piece-id>/layout.json     # OMR-prepared pieces: the layout model's raw output per page,
                             #   written with the layout correction; NOT corrected
  <piece-id>/layout-corrected.json
                             # OMR-prepared pieces: the corrected measure, staff and
                             #   grand-staff boxes, written with the layout correction;
                             #   score.mei holds the measure zones grown by a margin
  <piece-id>/omr.xml         # OMR-prepared pieces: every staff's transcription and label,
                             #   written with the score setup; the page drafts are made from it
tracking/                    # five tables keyed by (task_id, subtask_id) —
  task.csv                   #   task/subtask definitions (fragment, locator, gates, depends_on)
  state.csv                  #   live status + validation cells
  lock.csv                   #   active claims
  history.csv                #   append-only log of every action, incl. rejects
  comment.csv                #   comments on tasks and the campaign (threaded, resolvable)
                             # generated at init & maintained by the automation
.github/workflows/
  caller.yml                 # triggers only — calls the campaign workflow in
                             # lets-encode/lets-encode, which runs the automation
                             # named in config.yaml
```

## Lifecycle in one paragraph

The Let's Encode app generates a campaign repo from this template, then commits —
in one go — a filled-in `config.yaml` (shaped like `config.example.yaml`), one
`sources/<piece-id>/score.mei` per piece (header filled from the config), the
five tracking tables (tasks `encoding_required`, their validation subtasks
`pending`, empty lock, history and comment tables) and the campaign's own
README, and removes `CHANGELOG.md` and `config.example.yaml`.
Each facsimile piece opens with pre-tasks chained by the `depends_on` column:
a measure correction (a layout correction for the OMR preparations), an
alignment when the piece starts from an uploaded encoding, then a score setup,
which the `omr-instruments` preparation leaves out. Its per-page encoding tasks
depend on the last pre-task, so they unlock once the pre-tasks complete.
From there, volunteers claim and submit work as pull requests; on each one
`caller.yml` calls the campaign workflow in `lets-encode/lets-encode`, which
checks out the central automation repo named in `config.yaml` and runs it,
and that coordinator validates the contribution, mutates the tables with its
own commit, and closes the PR. A run of the same workflow scheduled hourly
reaps stale locks; GitHub may delay a scheduled run by hours.

## File formats

`config.yaml` is YAML; the tracking tables are CSV (chosen for clean,
cell-level pull-request diffs and trivial machine parsing). The schema, the
column meanings, and the validation-cell encoding are defined in
`src/lib/campaign-init.ts` and `src/lib/campaign-tables.ts` of
`lets-encode/lets-encode`.
