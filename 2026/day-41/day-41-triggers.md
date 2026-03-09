Day 41 – Triggers & Matrix Builds

This task focuses on learning different ways to trigger GitHub Actions workflows and how to run jobs across multiple environments using Matrix builds.

Challenge Tasks
Task 1: Trigger on Pull Request
Step 1: Create Workflow File

Create the workflow file:

touch .github/workflows/pr-check.yml

Edit the file:

vi .github/workflows/pr-check.yml
Step 2: Trigger Only When PR Targets main
on:
  pull_request:
    branches:
      - main

This configuration ensures the workflow runs only when a pull request is opened or updated against the main branch.

Step 3: Print Pull Request Information

Add the following step:

steps:
  - name: Print PR info
    run: |
      echo "PR number is ${{ github.event.pull_request.number }}"
      echo "Source branch for PR: ${{ github.head_ref }}"
      echo "Target branch for PR: ${{ github.base_ref }}"

Explanation:

github.event.pull_request.number → Shows the PR number

github.head_ref → Shows the source branch

github.base_ref → Shows the target branch

Step 4: Add, Commit, and Push Workflow
git add .github/workflows/pr-check.yml
git commit -m "Add PR check workflow"
git push origin main
Step 5: Create a Branch and Open a PR
git checkout -b feat/merge
git add .
git commit -m "Add changes"
git push origin feat/merge

Then open a Pull Request from feat/merge → main.

Why It Shows on the PR Page

GitHub Actions workflows run automatically when specific events occur.

Your workflow trigger:

on:
  pull_request:
    branches:
      - main

This means:

Run this workflow whenever a pull request is opened or updated targeting the main branch.

GitHub automatically connects the workflow run to the PR.

What You See on the PR Page

When the workflow runs, GitHub shows a Checks section on the PR page.

You can see:

Workflow name

Status (Running / Success / Failed)

Logs for each step

Example logs:

PR number is 2
Source branch for PR: feature/pr-test
Target branch for PR: main
Task 2: Scheduled Trigger

A workflow can run automatically using a cron schedule.

Example: Run every day at midnight (UTC)

on:
  schedule:
    - cron: "0 0 * * *"
Cron Expression for Every Monday at 9 AM
0 9 * * 1

Cron format:

minute hour day-of-month month day-of-week

Explanation:

0 → minute

9 → hour

* → every day of month

* → every month

1 → Monday

Task 3: Manual Workflow Trigger

The goal is to run a workflow manually from the GitHub Actions tab.

Step 1: Create Workflow File
touch .github/workflows/manual.yml
Step 2: Add Manual Trigger
on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Choose environment (staging or production)"
        required: true
        default: "staging"
Step 3: Print Input Value
steps:
  - name: Print environment
    run: echo "Selected environment is ${{ github.event.inputs.environment }}"
Step 4: Run Workflow Manually

Go to GitHub Repository

Click Actions tab

Select the workflow

Click Run workflow

Enter the environment name

Example output:

Selected environment is staging
Task 4: Matrix Builds

Matrix builds allow running the same job with multiple configurations.

Step 1: Create Workflow File
touch .github/workflows/matrix.yml
Step 2: Define Python Matrix
strategy:
  matrix:
    python-version: ["3.10", "3.11", "3.12"]
Step 3: Run Job
runs-on: ubuntu-latest

Install Python and print version:

steps:
  - uses: actions/setup-python@v4
    with:
      python-version: ${{ matrix.python-version }}

  - run: python --version

Result:

3 jobs run in parallel:

Python 3.10

Python 3.11

Python 3.12

Extend Matrix with Operating Systems
matrix:
  os: ["ubuntu-latest", "windows-latest"]
  python-version: ["3.10", "3.11", "3.12"]
Total Jobs
2 OS × 3 Python versions = 6 Jobs
Task 5: Exclude & Fail-Fast
Exclude Specific Combination

Example: Exclude Python 3.10 on Windows

matrix:
  os: ["ubuntu-latest", "windows-latest"]
  python-version: ["3.10", "3.11", "3.12"]
  exclude:
    - os: windows-latest
      python-version: "3.10"
Disable Fail-Fast
strategy:
  fail-fast: false
Fail-Fast Explanation
fail-fast: true (Default)

If one job fails, the remaining jobs are cancelled immediately.

Example:

Job1 → Failed
Job2 → Cancelled
Job3 → Cancelled
fail-fast: false

If one job fails, other jobs continue running.

Example:

Job1 → Failed
Job2 → Success
Job3 → Success

This helps you see all test results instead of stopping early.
