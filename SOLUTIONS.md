# GitHub Actions Assignment — Solution

This file contains the work completed for each question, along with the screenshots and successful workflow runs.

---

## Q1 — Manual Workflow with Inputs and Job Outputs

**Workflow:** `.github/workflows/q1-release-notes.yml`

### What I did

I created a manually triggered workflow using `workflow_dispatch`.

It accepts:
- `version` as a required input
- `release_type` as a choice of `patch`, `minor`, or `major`

The `prepare` job creates the build tag and current date and passes them as outputs.

The `announce` job uses these outputs and displays them in the workflow summary.

### Screenshots

- [Q1 Part 1](images/q1/q1-part1.png)
- [Q1 Part 2](images/q1/q1-part2.png)

### Workflow Run

[Q1 Successful Run](https://github.com/prachi-satbhai0741/gha-assignment/actions/runs/33971906818)

---

## Q2 — Matrix Strategy, Caching and Artifacts

**Workflow:** `.github/workflows/q2-matrix-build.yml`

### What I did

I created the build workflow using a matrix with:

- `ubuntu-latest`
- `ubuntu-22.04`
- Node.js `18`
- Node.js `20`

The combination of `ubuntu-22.04` with Node.js `18` is excluded.

Each matrix job checks out the repository, sets up the required Node.js version, prints the Node.js and npm versions, creates the required report file and uploads it as an artifact.

I also used `fail-fast: false` so that the other matrix jobs continue even if one job fails.

The `collect` job runs after the complete matrix and collects the generated artifacts.

### Screenshots

- [Q2 Part 1](images/q2/q2-part1.png)
- [Q2 Part 2](images/q2/q2-part2.png)

### Workflow Run

[Q2 Successful Run](https://github.com/prachi-satbhai0741/gha-assignment/actions/runs/33973383146)

---

## Q3 — Debugging the Broken Workflow

**Workflow:** `.github/workflows/q3-fix-me.yml`

I checked the broken workflow, identified the four issues and fixed them.

### Bugs and Fixes

| Bug | Code Line | Cause | Fix |
|---|---|---|---|
| Incorrect workflow trigger | `workflow_dispatch` | The trigger syntax was incomplete. | Changed it to `workflow_dispatch:` |
| Incorrect runner configuration | `runs-on` | The runner value was incorrect. | Used `ubuntu-latest`. |
| Script file was not found | `run: bash scripts/hello.sh` | The repository was not checked out before running the script. | Added checkout before the script step. |
| Checkout action version missing | `uses: actions/checkout` | The action did not specify a version. | Changed it to `actions/checkout@v4`. |

### Screenshot

- [Q3](images/q3/q3.png)

### Workflow Run

[Q3 Successful Run](https://github.com/prachi-satbhai0741/gha-assignment/actions/runs/33973660850)

---

## Q4 — Conditional Deploy

**Workflow:** `.github/workflows/q4-fix-me.yml`

### Bugs and Fixes

| Bug | Code Line | Cause | Fix |
|---|---|---|---|
| Environment free text | `environment:` | Users could enter any value. | Changed the input to a choice with `staging` and `production`. |
| Deploy job restricted to production | `if: ${{ inputs.environment == 'production' }}` | The complete job was skipped for staging. | Removed the job-level condition. |
| Guarded step condition incorrect | `if: $APP_NAME == 'demo-app'` | The environment variable was not referenced through the GitHub Actions context. | Changed it to `if: ${{ env.APP_NAME == 'demo-app' }}`. |
| Notify step condition  incorrect | `if: ${{ success() && failure() }}` | `success()` and `failure()` cannot be true at the same time. | Changed it to `if: ${{ always() }}`. |

### Screenshot

- [Q4](images/q4/q4.png)

### Workflow Run

[Q4 Successful Run](https://github.com/prachi-satbhai0741/gha-assignment/actions/runs/33974167200)

---

## Q5 — Job Outputs Come Through Empty

**Workflow:** `.github/workflows/q5-fix-me.yml`

### Bugs and Fixes

| Bug | Code Line | Cause | Fix |
|---|---|---|---|
| Step ID missing | `${{ steps.make.outputs.version }}` | The output was referring to a step that had no `id`. | Added `id: make`. |
| Old output syntax used | `::set-output` | The `set-output` command is deprecated. | Used `$GITHUB_OUTPUT` instead. |
| Job dependency missing | `consume:` | The second job could start before `generate` finished. | Added `needs: generate`. |

### What I checked

After fixing the workflow, I verified that the generated version is passed correctly from the `generate` job to the `consume` job.

### Screenshots

- [Q5 Part 1](images/q5/q5-part1.png)
- [Q5 Part 2](images/q5/q5-part2.png)

### Workflow Run

[Q5 Successful Run](https://github.com/prachi-satbhai0741/gha-assignment/actions/runs/33974367639)

---

## Q6 — Composite Action + Reusable Workflow

**Files:**
- `.github/actions/greet/action.yml`
- `.github/workflows/q6-reusable.yml`
- `.github/workflows/q6-caller.yml`

### What I did

I created a composite action that accepts `name` as a required input and `env_name` with `dev` as the default value. It provides a `greeting` output and runs two shell steps.

The reusable workflow is triggered using `workflow_call`. It accepts `target_env`, uses the composite action and prints the greeting along with the simulated deployment message.

The caller workflow is triggered using `workflow_dispatch` and calls the reusable workflow twice — once with `dev` and once with `prod`. The `prod` call depends on the successful completion of the `dev` call.

### Screenshots

- [Q6 Part 1](images/q6/q6-part1.png)
- [Q6 Part 2](images/q6/q6-part2.png)

### Workflow Run

[Q6 Successful Run](https://github.com/prachi-satbhai0741/gha-assignment/actions/runs/33975142049)

---
