# Day 42 – Runners: GitHub-Hosted & Self-Hosted

## Overview

In GitHub Actions every job needs a machine to run. That machine is called a **runner**.
There are two types of runners:

1. GitHub-hosted runner (provided by GitHub)
2. Self-hosted runner (our own machine)

---

# Task 1 – GitHub-Hosted Runners

## Workflow with 3 jobs on different OS

We run jobs on these operating systems:

* ubuntu-latest
* windows-latest
* macos-latest

Each job prints:

* OS name
* Runner hostname
* Current user running the job

### Ubuntu Runner

```yaml
runs-on: ubuntu-latest
steps:
  - name: Display runner info
    run: |
      echo "OS Name: ${{ runner.os }}"
      echo "Hostname: $(hostname)"
      echo "Current User: $(whoami)"
```

### Windows Runner

```yaml
runs-on: windows-latest
steps:
  - name: Print Windows info
    run: |
      echo "OS Name: ${{ runner.os }}"
      echo "Hostname: $env:COMPUTERNAME"
      echo "Current User: $env:USERNAME"
```

### macOS Runner

```yaml
runs-on: macos-latest
steps:
  - name: Print macOS info
    run: |
      echo "OS Name: ${{ runner.os }}"
      echo "Hostname: $(hostname)"
      echo "Current User: $(whoami)"
```

All three jobs run **at the same time (parallel)** in GitHub Actions.

---

## What is a GitHub-Hosted Runner?

A **GitHub-hosted runner** is a virtual machine given by GitHub to run our workflow jobs.

When workflow starts, GitHub creates a temporary machine.
The job runs on that machine.
After the job finishes, the machine is deleted automatically.

### Who manages it?

GitHub manages everything:

* server
* operating system
* updates
* security

Developers only write workflow files.

---

# Task 2 – Explore Pre-installed Tools

On `ubuntu-latest` runner we checked versions of some tools.

```yaml
- name: Print software versions
  run: |
    echo "Docker version: $(docker --version)"
    echo "Python version: $(python3 --version)"
    echo "Node version: $(node --version)"
    echo "Git version: $(git --version)"
```

GitHub runners already have many tools installed like:

* Docker
* Python
* Node.js
* Git

---

## Why pre-installed tools are useful?

Because:

* we do not need to install tools every time
* workflow runs faster
* setup becomes easier

If tools were not installed, we would need to install them in every workflow run.

---

# Task 3 – Set Up a Self-Hosted Runner

A **self-hosted runner** means a machine owned by us that runs GitHub Actions jobs.

It can be:

* our laptop
* a server
* a cloud VM

### Why we use self-hosted runners?

Sometimes we need our own machine because:

* we need special software
* we need more CPU or RAM
* job needs access to private network
* long running jobs
* cheaper than GitHub minutes

---

## Steps to Configure Self-Hosted Runner

Go to:

Repository → **Settings → Actions → Runners → New self-hosted runner**

Choose **Linux**.

Then run these commands on your machine.

### Step 1 – Create folder

```bash
mkdir actions-runner
cd actions-runner
```

### Step 2 – Download runner

```bash
curl -o actions-runner-linux-x64.tar.gz -L https://github.com/actions/runner/releases/download/v2.332.0/actions-runner-linux-x64-2.332.0.tar.gz
```

### Step 3 – Extract files

```bash
tar xzf actions-runner-linux-x64.tar.gz
```

### Step 4 – Configure runner

```bash
./config.sh --url https://github.com/your-repo-url --token YOUR_TOKEN
```

### Step 5 – Start runner

```bash
./run.sh
```

After running this command, the runner appears in GitHub with a **green dot** and **Idle status**.

---

### Start runner again later

If the runner stops, run:

```bash
cd actions-runner
./run.sh
```

---

# Task 4 – Use Self-Hosted Runner

Example workflow:

```yaml
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

      - name: Create file
        run: echo "Hello from self hosted runner" > test-file.txt
```

After the workflow finishes, the file **test-file.txt** will be created on the machine where the runner is running.

---

# Task 5 – Labels

We can add labels to runners.

Example label:

```
my-linux-runner
```

Update workflow:

```yaml
runs-on: [self-hosted, my-linux-runner]
```

---

## Why labels are useful?

When we have many self-hosted runners, labels help us select the correct machine.

Example:

Linux machine → `linux-runner`
Windows machine → `windows-runner`
macOS machine → `macos-runner`

Workflow example:

```yaml
runs-on: [self-hosted, linux-runner]
```

This makes sure the job runs on the correct machine.

---

# Task 6 – GitHub-Hosted vs Self-Hosted

| Feature             | GitHub-Hosted                    | Self-Hosted                         |
| ------------------- | -------------------------------- | ----------------------------------- |
| Who manages it?     | Managed by GitHub                | Managed by user                     |
| Cost                | Free limited minutes, extra paid | You pay for your own server         |
| Pre-installed tools | Many tools already installed     | Need to install manually            |
| Good for            | Quick CI/CD and small projects   | Custom environment and heavy builds |
| Security concern    | Code runs on GitHub cloud        | You must secure your own machine    |

---

# Result

After completing Day 42:

* Jobs ran on **Ubuntu, Windows and macOS runners**
* Checked pre-installed tools
* Set up a **self-hosted runner**
* Ran jobs on personal machine
* Used labels to select the correct runner
