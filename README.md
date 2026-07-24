# Let's Encode! — campaign template

This is the **template repository** every campaign is stamped from (via GitHub's
"create a repository from a template"). A copy of it becomes one crowd-encoding
campaign: it holds the score sources, the campaign configuration, two
machine-maintained tracking tables, and one generic caller workflow that runs
the campaign automation.

> You normally don't touch these files by hand. The instigation platform fills
> in the configuration at campaign start, and the volunteer client claims and
> submits work through pull requests. The files are kept human-readable so they
> review cleanly in pull-request diffs.

## Layout

```
config.example.yaml          # campaign config TEMPLATE (schema v1) — documented inline
config.yaml                  # written at instigation by the GUI (not in the template)
sources/
  score.mei                  # written at init from the template below (not in the template)
templates/
  score.template.mei         # barebones MEI: 1 measure, 1 note, header placeholders
tracking/                    # four tables keyed by (task_id, subtask_id) —
  task.csv                   #   task/subtask definitions (fragment, locator, gates)
  state.csv                  #   live status + validation cells
  lock.csv                   #   active claims
  history.csv                #   append-only log of every action, incl. rejects
                             # generated at init & maintained by the automation
.github/workflows/
  caller.yml                 # the ONE task-agnostic caller — forwards every event
                             # to the central automation named in config.yaml
```

## Lifecycle in one paragraph

The instigation GUI generates a campaign repo from this template, then commits —
in one go — a filled-in `config.yaml` (shaped like `config.example.yaml`),
`sources/score.mei` stamped from `templates/score.template.mei` (header filled
from the config), and the four tracking tables (tasks `encoding_required`,
their validation subtasks `pending`, empty lock and history tables). From
there, volunteers claim and submit work
as pull requests; on each one `caller.yml` checks out the central automation
repo named in `config.yaml` and runs it, and that coordinator validates the
contribution, mutates the tables, and closes or merges the PR.

## File formats

`config.yaml` is YAML; the tracking tables are CSV (chosen for clean,
cell-level pull-request diffs and trivial machine parsing). The schema, the
column meanings, and the validation-cell encoding are specified in `DESIGN.md`
in the `instigation` repo (the single design + status document for the project).
