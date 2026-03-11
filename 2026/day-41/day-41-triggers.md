# Day 41 – Triggers & Matrix Builds

## Overview

Today I learned how to trigger workflows in different ways and how to run jobs across multiple environments using a matrix.

---

# Task 1 – Trigger on Pull Request

**Goal:** Run a workflow whenever a pull request (PR) is opened or updated against the `main` branch.

**Steps:**

1. **Create the workflow file**

```bash id="create-pr-yml"
touch .github/workflows/pr-check.yml
```

2. **Edit the workflow file**

```bash id="edit-pr-yml"
vi .github/workflows/pr-check.yml
```

3. **Trigger only on PR to main**

```yaml id="pr-trigger"
on:
  pull_request:
    branches:
      - main
```

4. **Add steps to print PR info**

```yaml id="pr-steps"
steps:
  - name: Print PR info
    run: |
      echo "PR number is ${{ github.event.pull_request.number }}"
      echo "Source branch for PR: ${{ github.head_ref }}"
      echo "Target branch for PR: ${{ github.base_ref }}"
```

5. **Add, commit, and push workflow**

```bash id="git-commands"
git add .github/workflows/pr-check.yml
git commit -m "Add PR check workflow"
git push -u origin main
```

6. **Create a new branch and push**

```bash id="new-branch"
git checkout -b feat/merge
git add .
git commit -m "Add changes"
git push origin feat/merge
```

7. **Open a Pull Request** from the new branch → workflow runs automatically

**Why it shows up on the PR page:**
GitHub Actions link workflows to PRs when they are triggered by `on: pull_request`.

**What you see:**

* Workflow name
* Status ✅/❌/⏳
* Logs showing PR number, source branch, target branch

**Today I learned:** Workflows can automatically run on PRs and show results directly on the PR page.

---

# Task 2 – Scheduled Trigger

**Goal:** Run a workflow automatically on a schedule using cron.

**Steps:**

```yaml id="schedule-yml"
name: Daily Workflow

on:
  schedule:
    - cron: "0 0 * * *"  # every day at midnight UTC

jobs:
  schedule-job:
    runs-on: ubuntu-latest
    steps:
      - name: say hi
        run: echo "Scheduled workflow running"
```

**Notes:**

* Cron for **every Monday at 9 AM UTC:** `0 9 * * 1`
* For 9 AM IST → convert to UTC: 3:30 UTC

**Today I learned:** I can schedule workflows to run automatically using cron syntax.

---

# Task 3 – Manual Workflow Trigger

**Goal:** Trigger a workflow manually from the Actions tab with user input.

**Steps:**

1. **Create the workflow file**

```bash id="manual-yml"
touch .github/workflows/manual.yml
```

2. **Edit the workflow file**
   Add `workflow_dispatch` and an input for environment:

```yaml id="manual-trigger"
on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Choose environment (staging or production)"
        required: true
        default: staging
```

3. **Add a step to print input value**

```yaml id="manual-step"
steps:
  - name: Print environment
    run: echo "Selected environment is ${{ github.event.inputs.environment }}"
```

4. **Push workflow** → go to **Actions tab → Run workflow manually** → enter input → see printed value

**Today I learned:** I can trigger workflows manually and pass values like staging or production.

---

# Task 4 – Matrix Builds

**Goal:** Run the same job multiple times using different Python versions and OS.

**Steps:**

1. **Create workflow file**

```bash id="matrix-yml"
touch .github/workflows/matrix.yml
```

2. **Define matrix strategy**

```yaml id="matrix-strategy"
strategy:
  matrix:
    python-version: ["3.10", "3.11", "3.12"]
    os: ["ubuntu-latest", "windows-latest"]
```

3. **Run job on a specific OS**

```yaml id="matrix-job"
runs-on: ${{ matrix.os }}
```

4. **Add steps to install Python and print version**

```yaml id="matrix-steps"
steps:
  - uses: actions/checkout@v4
  - uses: actions/setup-python@v4
    with:
      python-version: ${{ matrix.python-version }}
  - run: python --version
```

5. **Push workflow** → 6 jobs run in parallel (3 Python × 2 OS)

**Today I learned:** Matrix lets me run the same job in multiple environments automatically.

---

# Task 5 – Exclude & Fail-Fast

**Goal:** Exclude one combination from matrix and set `fail-fast` behavior.

**Steps:**

1. **Add exclude section**

```yaml id="exclude-matrix"
exclude:
  - os: windows-latest
    python-version: 3.10
```

2. **Set fail-fast**

```yaml id="failfast"
fail-fast: false
```

3. **Push workflow** → if one job fails, others continue

**Notes:**

* `fail-fast: true` → stops all jobs if one fails
* `fail-fast: false` → all jobs run even if one fails

**Today I learned:** I can skip certain combinations and control whether workflow stops on failure.

---

# Result

After Day 41:

* Learned **PR triggers**, **scheduled triggers**, and **manual triggers**
* Learned how to **run matrix builds** for multiple Python versions and OS
* Learned how to **exclude combinations** and control fail-fast behavior
* Workflows now more automated, flexible, and beginner-friendly

---

This file is ready to save as `day-41-triggers.md`.
