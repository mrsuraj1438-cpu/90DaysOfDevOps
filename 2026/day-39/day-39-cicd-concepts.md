# Day 39 – CI/CD Concepts

---

## 1. What is CI/CD?

CI/CD stands for **Continuous Integration and Continuous Delivery/Deployment**. It is a way to automatically build, test, and deploy software so developers do not have to do it manually every time.

* **CI (Continuous Integration)**: Developers frequently push code, and every push triggers automated builds and tests.
* **CD (Continuous Delivery/Deployment)**: The tested code is automatically prepared for production (Delivery) or automatically deployed (Deployment).

Think of it as a **software factory assembly line**.

---

## 2. Challenge Task 1 – The Problem

A team of 5 developers manually deploying code to production can face many issues.

* **What can go wrong?**

  * Code conflicts when multiple developers change the same file.
  * Broken builds or errors in production.
  * Downtime because deployment is slow and error-prone.

* **"It works on my machine" problem**

  * Code runs on a developer’s computer but fails in other environments because of different dependencies or configurations.
  * This leads to unexpected production errors.

* **How many times a day can a team safely deploy manually?**

  * Only a few times a day (1–2), because human errors and coordination take time.

---

## 3. Challenge Task 2 – CI vs CD

### Continuous Integration (CI)

* **What happens:** Code is automatically built and tested after every push.
* **How often:** Every code push, pull request, or scheduled run.
* **What it catches:** Integration conflicts, build failures, broken tests.
* **Example:** A FastAPI project runs CI on GitHub Actions after every push to main.

### Continuous Delivery (CD)

* **Difference from CI:** Prepares code for production but requires **manual approval**.
* **Delivery means:** Code is packaged and deployed to staging, ready for production.
* **Example:** Banking app deploys to staging, QA approves before going live.

### Continuous Deployment

* **Difference from Delivery:** Automatically deploys to production **without manual approval**.
* **When used:** Rapid release environments like SaaS, streaming platforms.
* **Example:** Netflix-style service deploys updates automatically after passing tests.

---

## 4. Challenge Task 3 – Pipeline Anatomy

| Part         | What it does                                                          |
| ------------ | --------------------------------------------------------------------- |
| **Trigger**  | Event that starts the pipeline (push, pull request, schedule, manual) |
| **Stage**    | Logical phase of pipeline (build, test, deploy)                       |
| **Job**      | Unit of work inside a stage (e.g., run tests)                         |
| **Step**     | Single command or action inside a job (e.g., `npm install`)           |
| **Runner**   | Machine that executes the job (VM, container, physical server)        |
| **Artifact** | Output produced by a job (build file, Docker image, test report)      |

Full flow: Developer → Trigger → Stage → Job → Step → Runner → Artifact → Next Stage → Users

---

## 5. Challenge Task 4 – Draw a Pipeline

**Scenario:** Developer pushes code to GitHub. App is tested, built into Docker image, deployed to staging server.

### Stages:

1. **Test Stage:** Run unit tests, code checks.
2. **Build Stage:** Install dependencies, build Docker image.
3. **Deploy Stage:** Push image and deploy to staging server.

**Pipeline Diagram (text-based):**

```
Developer
   |
   | git push
   v
GitHub Repository
   |
   v
+-------------------+
| Stage 1: Test     |
| - Run Unit Tests  |
| - Lint Code       |
+-------------------+
   |
   v
+-------------------+
| Stage 2: Build    |
| - Install deps    |
| - Build Docker    |
+-------------------+
   |
   v
+-------------------+
| Stage 3: Deploy   |
| - Push Docker     |
| - Deploy Staging  |
+-------------------+
   |
   v
Staging Server
```

---

## 6. Challenge Task 5 – Explore in the Wild (Real Example)

**Repository Chosen:** FastAPI GitHub Actions CI/CD Demo (berkecengiz/fastapi-github-actions-ci-demo)

**Workflow File:** `.github/workflows/ci.yml`

**What triggers it?**

* Push to the `main` branch
* Pull request targeting the `main` branch

**How many jobs does it have?**

* 4 jobs:

  1. Lint & Format (checks code style)
  2. Security Scan (checks for vulnerabilities)
  3. Run Tests (automated test suite)
  4. Build Docker Image (builds the container)

**What does it do? (best guess/explanation)**

* Ensures code is formatted correctly
* Scans for security issues
* Runs all automated tests
* Builds a Docker image ready for deployment

**Visual flow (text-based):**

```
Developer pushes code
       |
       v
GitHub Actions workflow starts
       |
   +--------------------+
   | Job 1: Lint & Format |
   +--------------------+
       |
   +--------------------+
   | Job 2: Security Scan |
   +--------------------+
       |
   +--------------------+
   | Job 3: Run Tests   |
   +--------------------+
       |
   +--------------------+
   | Job 4: Build Docker|
   +--------------------+
       |
   Docker image ready for deployment
```

---

## 7. CI/CD Stages Summary

* Stage 1: Test (lint, unit/integration tests)
* Stage 2: Build (compile, Docker image, package)
* Stage 3: Deploy (staging or production)

All connected by triggers, jobs, steps, runners, and artifacts.

---

## 8. One-Line Summary

**CI/CD automates software building, testing, packaging, and deployment, triggered by code changes, using stages, jobs, steps, runners, and artifacts.**

---

## 9. Today I Learned 

Today I learned that CI/CD pipelines help teams automatically build, test, and deploy code safely and quickly, catching errors early, and making development faster and more reliable.
