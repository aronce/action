# Aronce GitHub Action

Review pull requests, fix failing CI, or run any task with [Aronce](https://aronce.ai) from GitHub Actions. Every change comes back verified: Aronce reproduces the problem, makes the change in a clean sandbox, runs the tests and reviews it before anything is delivered.

## Setup

1. In Aronce, add the repository, then create a token in **Organization Settings → API tokens**.
2. In GitHub, add it as a repository secret named `ARONCE_TOKEN`.
3. Add a workflow, for example one from [`examples/`](examples).

```yaml
- uses: aronce/action@v1
  with:
    token: ${{ secrets.ARONCE_TOKEN }}
    mode: review   # review | ci | exec
```

## Modes

| Mode | What it does | Needs |
|---|---|---|
| `review` | Runs the pull request's checks, reviews it, and posts the review | runs on `pull_request`, or set `pull-request` |
| `ci` | Finds why checks fail and opens a pull request that makes them pass, without weakening tests | `branch` (defaults to the repository's default branch) |
| `exec` | Does anything the `prompt` asks, verified like any run | `prompt` |

## Inputs

| Input | Default | |
|---|---|---|
| `token` | — | An Aronce API token (required) |
| `mode` | `review` | `review`, `ci` or `exec` |
| `prompt` | | For `exec` |
| `pull-request` | the triggering PR | For `review` |
| `branch` | the default branch | For `ci` and `exec` |
| `deliver` | `true` | Once verified, post the review or open the pull request. `false` leaves the run waiting for approval in Aronce |
| `budget` | the project's policy | Credit limit for the run |
| `repository` | this repository | `owner/name` in Aronce |
| `api-url` | `https://api.aronce.ai/api/v1` | |

## Outputs

`mission-id`, `status`, `verified` (`true`/`false`) and `pull-request-url`. Each run also writes a summary to the job page.

The job fails only when the run fails or is cancelled. A run waiting on you (questions, or paused for credits) shows a warning and passes.

## Publishing

This folder is the whole action. To make `uses: aronce/action@v1` work, copy it to the root of a public `aronce/action` repository, then tag a release `v1`.
