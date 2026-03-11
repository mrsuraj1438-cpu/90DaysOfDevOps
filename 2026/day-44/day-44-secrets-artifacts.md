# Day 44 – Secrets, Artifacts & Running Real Tests in CI

## Overview the 

Today the CI pipeline performs real tasks such as storing sensitive data securely, saving important files created during the pipeline, and running real scripts/tests automatically.

---

# Task 1 – GitHub Secrets

## Step 1: Create a Secret

1. Open the repository on GitHub
2. Go to **Settings → Secrets and Variables → Actions**
3. Click **New repository secret**
4. Name: `MY_SECRET_MESSAGE`
5. Add any value and save.

## Step 2: Workflow to Check Secret

Create workflow `.github/workflows/secret-check.yml`

```yaml
name: Secret Check

on: push

jobs:
  check-secret:
    runs-on: ubuntu-latest
    steps:
      - name: Check if secret exists
        run: |
          if [ -z "${{ secrets.MY_SECRET_MESSAGE }}" ]; then
            echo "The secret is set: false"
          else
            echo "The secret is set: true"
          fi
```

This workflow checks whether the secret exists without revealing its value.

## Try Printing the Secret

```yaml
- name: Try printing secret
  run: echo "${{ secrets.MY_SECRET_MESSAGE }}"
```

GitHub output:

```
***
```

GitHub automatically masks secrets in logs.

## Why You Should Never Print Secrets in CI Logs

Secrets contain sensitive information such as API keys, tokens, or passwords.
If these values appear in CI logs, anyone with access to the logs could misuse them to access services, modify data, or damage the system.

For example, if a Docker registry token or database password is printed in logs, attackers could use it to access your production services.

Therefore secrets should only be accessed securely through environment variables and never printed.

---

# Task 2 – Using Secrets as Environment Variables

Secrets can be passed safely to steps using environment variables.

Example:

```yaml
- name: Use secret as environment variable
  env:
    SECRET_MSG: ${{ secrets.MY_SECRET_MESSAGE }}
  run: echo "Secret loaded successfully"
```

Here the secret is used without hardcoding it in the workflow.

Additional secrets created for future tasks:

```
DOCKER_USERNAME
DOCKER_TOKEN
```

These will be used later for Docker authentication.

---

# Task 3 – Upload Artifacts

Artifacts store files created during the pipeline such as logs, reports, or build outputs.

Example workflow step:

```yaml
- name: Generate log file
  run: echo "Test completed successfully" > test-report.txt

- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: test-report
    path: test-report.txt
```

After the workflow finishes:

1. Open the **Actions** tab in GitHub.
2. Open the workflow run.
3. Download the artifact.

You will see the file `test-report.txt`.

---

# Task 4 – Download Artifacts Between Jobs

Artifacts can also be shared between jobs.

Example workflow:

```yaml
name: Artifact Example

on: push

jobs:
  job1:
    runs-on: ubuntu-latest
    steps:
      - name: Create file
        run: echo "Hello from Job1" > message.txt

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: message-file
          path: message.txt

  job2:
    runs-on: ubuntu-latest
    needs: job1

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: message-file

      - name: Print file content
        run: cat message.txt
```

Job1 creates the file and uploads it.
Job2 downloads the file and prints its content.

## When Would You Use Artifacts in a Real Pipeline?

Artifacts are used when a job produces important files that need to be stored or shared with other jobs.

For example:

* A build job creates a log file (`build.log`)
* The pipeline uploads it as an artifact
* If the build fails, developers download the log file and analyze the error.

Artifacts are commonly used for:

* logs
* test reports
* compiled build outputs
* debugging pipeline failures.

---

# Task 5 – Run Real Tests in CI

Example script from earlier days.

Create file `hello.sh`

```bash
#!/bin/bash
echo "Running CI script"
echo "Script executed successfully"
```

Workflow:

```yaml
name: Run Script

on: push

jobs:
  run-test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run script
        run: |
          chmod +x hello.sh
          ./hello.sh
```

If the script returns exit code **0**, the pipeline passes.
If the script returns a **non-zero exit code**, the pipeline fails.

Example failing script:

```bash
exit 1
```

When this happens, the CI pipeline turns **red**.

After fixing the script and pushing again, the pipeline turns **green**.

---

# Task 6 – Caching

Caching speeds up workflows by storing dependencies.

Example:

```yaml
- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: ~/.npm
    key: npm-cache-${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}
```

When the workflow runs the first time, dependencies are downloaded.
On the second run, they are restored from the cache, which makes the workflow faster.

## What Is Being Cached and Where Is It Stored?

The cache stores dependency files (for example npm packages or Python libraries).
GitHub stores this cache on its own infrastructure and restores it automatically in future workflow runs when the cache key matches.

---

# Result

At the end of Day 44:

* Secrets are stored securely and used in workflows
* Artifacts are uploaded and downloaded between jobs
* Real scripts run inside CI
* Pipelines fail when errors occur
* Caching improves workflow speed

This confirms the CI pipeline is performing real development tasks automatically.
