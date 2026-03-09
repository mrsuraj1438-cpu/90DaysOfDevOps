
# This is day41 task:
Challenge Tasks

Task 1: Trigger on Pull Request

Create .github/workflows/pr-check.yml

solution: 
step1: touch .github/workflows/pr-check.yml : create workflow file

step2: vi .github/workflows/pr-check.yml : write for workflow file

step3: after that add and commit then push to github 

1. git add .github/workflows/pr-check.yml
2. git commit -m "add the pr check"
3. git push -u origin main

Q2: Trigger it only when a pull request is opened or updated against main
step1: this step is used for only pull request is opend or updated againt main
on:
  pull_request:
    branches:
      - main
        
Q3: Add a step that prints: PR check running for branch: <branch name>
        - name: Print PR info
          run: |
          echo "PR number is ${{ github.event.pull_request.number }}" # this step show the PR number
          echo "Source branch For PR ${{ github.head_ref }}"  # this step show the which branch is pull request ask to
          echo "Target branch for PR ${{ gitub.base_ref }}" # this step show the target branch where to pull request

Q4: Create a new branch, push a commit, and open a PR
  git checkout -b feat/merge
  git add .
  git commit -m "Add" 
  git push origin feat/merg

Watch the workflow run automatically
Verify: Does it show up on the PR page?
1️⃣ Why it shows up on the PR page

GitHub Actions are automated workflows that run on events (like push or pull request).

Your workflow is triggered on this event:

on:
  pull_request:
    branches:
      - main

This means: “Run this workflow anytime someone opens or updates a PR targeting the main branch.”

GitHub automatically links this workflow run to the PR, so you can see it in the PR page.

2️⃣ How it shows up

You open a PR (example: feature/pr-test → main).

GitHub detects the PR and triggers all workflows that match the on: pull_request condition.

The workflow runs on a virtual machine (runner).

Each step executes, and the logs are saved.

On the PR page, you see a “Checks” section showing:

Workflow name (e.g., PR Check)

Status (✅ success, ❌ fail, ⏳ in progress)

Clicking it shows step-by-step logs, like:

PR number is 2
Source branch for PR: feature/pr-test
Target branch for PR: main
3️⃣ What you see

On the PR page:

Conversation tab: Where you discuss the PR.

Commits tab: Shows all commits in the PR.

Files changed tab: Shows the diff between branches.

Checks / Actions section: Shows your workflow running.

✅ This is where your PR Check workflow output appears.

4️⃣ Simple steps for a beginner

Push a branch:

git checkout -b feature/pr-test;
git add .
git commit -m "Test PR workflow"
git push origin feature/pr-test

Open a Pull Request from feature/pr-test → main.

On the PR page, scroll to the Checks section. You’ll see your workflow running.

Click the workflow name → see logs for each step.

Once complete, you can see “PR branch name is feature/pr-test” in the logs.




Task 1: Create a GitHub Actions Workflow on Pull Request
The goal of this task is to run a workflow whenever a pull request is opened or updated against the main branch.

Step 1: Create the workflow file
Run the command: touch .github/workflows/pr-check.yml. This will create the empty workflow file in the correct directory.

Step 2: Edit the workflow file
Open the file using a text editor, for example: vi .github/workflows/pr-check.yml. In this file, define the workflow to trigger on pull requests to the main branch, like so:

on:
  pull_request:
    branches:
      - main

Step 3: Add steps to print PR info
Under the steps, add a step to print details of the PR:

steps:
  - name: Print PR info
    run: |
      echo "PR number is ${{ github.event.pull_request.number }}"
      echo "Source branch for PR: ${{ github.head_ref }}"
      echo "Target branch for PR: ${{ github.base_ref }}"

Step 4: Add, commit, and push the workflow
Once you save the file, use the following commands: git add .github/workflows/pr-check.yml, git commit -m "Add PR check workflow", and git push origin main. After pushing, create a new branch with git checkout -b feat/merge, make your changes, commit them, and push the branch. Open a pull request from that branch to main, and you will see the workflow run under the Checks section.

Task 2: Add a Scheduled Trigger
In this task, we create a workflow that runs on a schedule. First, we define a schedule trigger in the workflow file. For example, we can set it to run every day at midnight UTC by adding:

on:
  schedule:
    - cron: "0 0 * * *"

If you want to run it every Monday at 9 AM, you would write:

cron: "0 9 * * 1"

Note that if you want 9 AM IST, convert it to UTC by subtracting 5 hours 30 minutes, so it would be 3:30 UTC. Once you save it, push the workflow file, and it will run automatically as per your schedule.




Task 3: Manual Workflow Trigger
The goal is to create a workflow that you can manually trigger from the Actions tab.
Step 1: Create the workflow file by running:
touch .github/workflows/manual.yml.
Step 2: Open the file using a text editor, for example: vi .github/workflows/manual.yml.
Step 3: Add a workflow_dispatch trigger to allow manual execution.
Step 4: Add an input for the environment by specifying:

inputs:
  environment:
    description: "Choose environment (staging or production)"
    required: true
    default: "staging"

Step 5: Add a step to print the input value by using:

steps:
  - name: Print environment
    run: echo "Selected environment is ${{ github.event.inputs.environment }}"

Step 6: Save the file, then push it to your repository. Once pushed, go to the Actions tab, select the workflow, and run it manually, entering the environment you choose.



Task 4: Matrix Builds
The goal is to run a job multiple times by testing different Python versions.
Step 1: Create the workflow file by running:
touch .github/workflows/matrix.yml.
Step 2: Open the file in a text editor, for example:
vi .github/workflows/matrix.yml.
Step 3: Define a matrix strategy with Python versions, such as:

strategy:
  matrix:
    python-version: ["3.10", "3.11", "3.12"]

Step 4: Set the job to run on a specific OS, for example:
runs-on: ubuntu-latest.
Step 5: Add steps to install Python using actions/setup-python and print the Python version.
Step 6: Save the file and push it. All three jobs will run in parallel, each with a different Python version.
Step 7: Extend the matrix by adding operating systems. For example, add:

matrix:
  os: ["ubuntu-latest", "windows-latest"]
  python-version: ["3.10", "3.11", "3.12"]

Step 8: Calculate the total jobs. Since there are 2 operating systems and 3 Python versions, you get 6 total jobs.

Task 5: Exclude & Fail-Fast
The goal is to exclude one combination in the matrix and set fail-fast to false.
Step 1: In the matrix, add an exclude section. For example:

matrix:
  os: ["ubuntu-latest", "windows-latest"]
  python-version: ["3.10", "3.11", "3.12"]
  exclude:
    - os: windows-latest
      python-version: 3.10

Step 2: Set fail-fast to false by adding fail-fast: false under the job.
Step 3: Save and push. When one job fails, others continue running.
Step 4: Write in your notes: When fail-fast is true (the default), the workflow stops as soon as one job fails. When fail-fast is false, all jobs continue running even if one fails, allowing you to see all results.
