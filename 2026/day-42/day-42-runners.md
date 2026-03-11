# Day 42 – Runners: GitHub-Hosted & Self-Hosted

## Overview

Every job in GitHub Actions needs a machine called a **runner** to run the workflow steps.
There are two types of runners:

1. **GitHub-hosted runner** – provided by GitHub
2. **Self-hosted runner** – your own machine or server

---

# Task 1 – GitHub-Hosted Runners

## Workflow with 3 jobs on different OS

We run jobs on:

* ubuntu-latest
* windows-latest
* macos-latest

Each job prints:

* OS name
* Runner hostname
* Current user running the job

### Ubuntu Runner

```yaml id="ubuntu-job"
runs-on: ubuntu-latest
steps:
  - name: Display runner info
    run: |
      echo "OS Name: ${{ runner.os }}"
      echo "Hostname: $(hostname)"
      echo "Current User: $(whoami)"
```

### Windows Runner

```yaml id="windows-job"
runs-on: windows-latest
steps:
  - name: Print Windows info
    run: |
      echo "OS Name: ${{ runner.os }}"
      echo "Hostname: $env:COMPUTERNAME"
      echo "Current User: $env:USERNAME"
```

### macOS Runner

```yaml id="macos-job"
runs-on: macos-latest
steps:
  - name: Print macOS info
    run: |
      echo "OS Name: ${{ runner.os }}"
      echo "Hostname: $(hostname)"
      echo "Current User: $(whoami)"
```

All three jobs run **in parallel**.

---

## What is a GitHub-Hosted Runner?

A **GitHub-hosted runner** is a virtual machine given by GitHub to run your workflow jobs.
GitHub automatically creates a temporary machine, runs your jobs, and deletes it after the workflow ends.

### Who manages it?

GitHub manages:

* the server
* the operating system
* updates and security

Developers just write workflows.

---

# Task 2 – Explore Pre-installed Tools

On `ubuntu-latest` runner we checked software versions:

```yaml id="preinstalled-tools"
- name: Print software versions
  run: |
    echo "Docker version: $(docker --version)"
    echo "Python version: $(python3 --version)"
    echo "Node version: $(node --version)"
    echo "Git version: $(git --version)"
```

GitHub runners already have many tools installed:

* Docker, Python, Node.js, Git, Java, .NET, etc.

You can check full list in GitHub docs.

---

## Why pre-installed tools are useful?

* Saves time – no need to install every tool manually
* Workflow runs faster
* Makes setup easy
* Builds are more reliable

---

# Task 3 – Set Up a Self-Hosted Runner

A **self-hosted runner** is your own machine (laptop, server, or cloud VM) that can run GitHub Actions jobs.

### Why use self-hosted runner?

* Need special software
* More CPU/RAM for heavy jobs
* Access to private network
* Long running jobs
* Cheaper than GitHub minutes

---

### Steps to Configure Self-Hosted Runner

Go to **Settings → Actions → Runners → New self-hosted runner** in your repo.
Choose **Linux**.

On your machine:

```bash id="selfhosted-setup"
mkdir actions-runner
cd actions-runner
curl -o actions-runner-linux-x64.tar.gz -L https://github.com/actions/runner/releases/download/v2.332.0/actions-runner-linux-x64-2.332.0.tar.gz
tar xzf actions-runner-linux-x64.tar.gz
./config.sh --url https://github.com/your-repo-url --token YOUR_TOKEN
./run.sh
```

After this, your runner shows **green dot and Idle status** in GitHub.

#### Restarting the Runner Later

```bash id="selfhosted-restart"
cd actions-runner
./run.sh
```

---

# Task 4 – Use Self-Hosted Runner

Example workflow:

```yaml id="selfhosted-job"
name: Self Hosted Test

on: push

jobs:
  test-runner:
    runs-on: self-hosted

    steps:
      - name: Print hostname
        run: hostname

      - name: Print working directory
        run: pwd

      - name: Create a file
        run: echo "Hello from self hosted runner" > test-file.txt
```

After running, **test-file.txt** appears on your machine.

---

# Task 5 – Labels

Labels help select the right runner when multiple self-hosted runners exist.

### Step 1 – Add label

Go to:

**Actions tab → Settings → Runners → Click your runner → Settings icon → Add label**

Example label: `my-linux-runner`

### Step 2 – Update workflow

```yaml id="label-workflow"
runs-on: [self-hosted, my-linux-runner]
```

Trigger workflow – job runs on the correct runner.

---

## Why labels are useful?

* Easy to choose the right runner
* Helps when runners have different OS or purposes
* Without labels, it’s hard to remember which runner is Linux, Windows, or macOS

### Example Scenario

Multiple self-hosted runners:

* Runner 1 → Linux → label: `linux-runner`
* Runner 2 → Windows → label: `windows-runner`
* Runner 3 → macOS → label: `macos-runner`

Workflow example:

```yaml id="label-example"
jobs:
  build:
    runs-on: [self-hosted, linux-runner]

    steps:
      - uses: actions/checkout@v4
      - run: echo "Running on Linux self-hosted runner"
```

If job needs Windows, use:

```yaml
runs-on: [self-hosted, windows-runner]
```

Job will run on correct machine.

---

# Result

After completing Tasks 1–5:

* Jobs ran on Ubuntu, Windows, macOS GitHub-hosted runners
* Checked pre-installed tools
* Configured self-hosted runner
* Ran jobs on personal machine
* Added labels to select correct runner
