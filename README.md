# Repo Autopilot

Zero-dependency GitHub Actions that automate the repetitive parts of running
this repo: triaging issues, labeling and summarizing PRs, sweeping stale
items, and rolling up a weekly digest. Everything runs on `actions/github-script`
against the built-in `GITHUB_TOKEN` — no external services, no secrets to
configure, no `npm install` step.

## What's included

| Workflow | Trigger | What it does |
| --- | --- | --- |
| [`pr-autopilot.yml`](.github/workflows/pr-autopilot.yml) | PR opened / synced | Labels the PR by diff size (`size/XS`…`size/XL`) and touched area (`area/docs`, `area/tests`, `area/ci`, …), then posts a sticky summary comment with the stats. Re-runs and updates in place on every push. |
| [`issue-autopilot.yml`](.github/workflows/issue-autopilot.yml) | Issue opened | Classifies the issue by keyword into `type/bug`, `type/feature`, `type/docs`, `type/question`, or `type/needs-triage`, flags `priority/high` for urgent-sounding reports, and posts a triage comment explaining the labels it applied. |
| [`stale-sweep.yml`](.github/workflows/stale-sweep.yml) | Daily cron (06:00 UTC) + manual | Marks issues/PRs stale after 30 days of inactivity, closes them 7 days later. Exempts `priority/high` and `pinned`. |
| [`weekly-digest.yml`](.github/workflows/weekly-digest.yml) | Weekly cron (Mon 14:00 UTC) + manual | Opens a `digest`-labeled issue summarizing the past 7 days: merged/opened PRs, opened/closed issues, and top contributors by merged-PR count. |

All labels are created on demand (idempotent `ensureLabel` check), so there's
nothing to pre-provision — the first run bootstraps everything.

## Trying it out

- **PR autopilot**: open any pull request against this repo. Within a few
  seconds it'll get `size/*` and `area/*` labels and a summary comment.
  Push another commit and watch the comment update in place instead of
  piling up.
- **Issue autopilot**: open an issue with a title like "App crashes on
  startup" vs. "Would be nice to support dark mode" and compare the labels
  each gets.
- **Stale sweep / weekly digest**: both support `workflow_dispatch`, so you
  can trigger them manually from the Actions tab (or via the API) instead of
  waiting for the cron schedule.

## Extending it

Each workflow is a single inline script, so tuning behavior is a matter of
editing constants near the top of the relevant file:

- Size thresholds and area-detection globs → `pr-autopilot.yml`
- Keyword rules for issue classification → `issue-autopilot.yml`
- Stale/close windows and exempt labels → `stale-sweep.yml`
- Lookback window and digest sections → `weekly-digest.yml`

No build step, no dependency lockfile — edit the YAML, push, done.
