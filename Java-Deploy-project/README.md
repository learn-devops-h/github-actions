# End-to-End CI/CD Pipeline for Java Application using GitHub Actions, Maven, SonarQube, and Kubernetes

## Project Overview

This project demonstrates a complete CI/CD (Continuous Integration and Continuous Deployment) pipeline for a Java application using:

- GitHub Actions
- Maven
- SonarQube/SonarCloud
- Kubernetes (K8s)

The pipeline automatically builds, analyzes, and deploys the application whenever code is pushed to the main branch.

---

# Architecture

```text
Developer
    │
    ▼
GitHub Repository
    │
    ▼
GitHub Actions Pipeline
    │
    ├── Checkout Code
    ├── Setup Java
    ├── Maven Build & Test
    ├── SonarQube Analysis
    │
    ▼
Kubernetes Deployment
    │
    ▼
Running Application
```

---

# Technologies Used

| Technology | Purpose |
|------------|----------|
| Java | Application Development |
| Maven | Build Tool |
| GitHub Actions | CI/CD Automation |
| SonarQube/SonarCloud | Code Quality Analysis |
| Kubernetes | Container Orchestration |
| GitHub Secrets | Secure Credential Storage |

---

# Workflow Trigger

The workflow is triggered whenever code is pushed to the `main` branch.

```yaml
on:
  push:
    branches:
      - main
```

Example:

```bash
git add .
git commit -m "New feature added"
git push origin main
```

After the push, GitHub Actions automatically starts the pipeline.

---

# Environment Variables

The workflow uses GitHub Secrets to securely store sensitive information.

```yaml
env:
  KUBECONFIG: ${{ secrets.KUBECONFIG }}
  SONAR_TOKEN: ${{ secrets.SONAR_TKN }}
```

## Required Secrets

### SONAR_TKN

Used to authenticate with SonarQube/SonarCloud.

### KUBECONFIG

Used to authenticate with the Kubernetes cluster.

---

# CI Pipeline (Build Job)

## Build Job

```yaml
build:
  runs-on: ubuntu-latest
```

The build process runs on an Ubuntu virtual machine provided by GitHub.

---

## Step 1: Checkout Source Code

```yaml
- name: Checkout code
  uses: actions/checkout@v2
```

Purpose:

- Downloads repository code to the runner.
- Makes source code available for build and deployment.

---

## Step 2: Setup Java

```yaml
- name: Java jdk setup
  uses: actions/setup-java@v1
  with:
    java-version: 12
```

Purpose:

- Installs Java Development Kit (JDK).
- Prepares the environment for Maven build.

---

## Step 3: Build Application Using Maven

```yaml
- name: maven target
  run: mvn clean install
```

Command Breakdown:

### mvn clean

Removes previous build artifacts.

### mvn install

Performs:

1. Compile
2. Test
3. Package
4. Install

Build Flow:

```text
Source Code
    │
    ▼
Compile
    │
    ▼
Unit Tests
    │
    ▼
Package
    │
    ▼
Install
```

Generated artifact:

```text
target/application.jar
```

---

## Step 4: SonarQube Analysis

```yaml
- name: Perform analysis on Sonar Qube
  uses: sonarsource/sonarcloud-github-action@v1
```

Purpose:

- Static code analysis
- Bug detection
- Security vulnerability detection
- Code smell detection
- Maintainability checks

### SonarQube Metrics

- Bugs
- Vulnerabilities
- Code Smells
- Code Coverage
- Duplications

Benefits:

- Improved code quality
- Better maintainability
- Security compliance

---

# CD Pipeline (Deploy Job)

## Deploy Job

```yaml
deploy:
  needs: build
```

This ensures deployment starts only after a successful build.

Pipeline Logic:

```text
Build Success
      │
      ▼
Deploy
```

If build fails:

```text
Build Failed
      │
      ▼
Deployment Skipped
```

---

## Checkout Code Again

```yaml
- name: Checkout code
  uses: actions/checkout@v2
```

Reason:

Each job runs on a fresh GitHub runner.

The code must be downloaded again.

---

## Deploy to Kubernetes

```yaml
- name: Deploy to k8s
  uses: stefanzweifel/k8s-action@v2.0.0
```

Command:

```yaml
args: apply -f kubernetes/
```

Equivalent Kubernetes command:

```bash
kubectl apply -f kubernetes/
```

This deploys all manifests located inside the Kubernetes directory.

Example:

```text
kubernetes/
├── deployment.yaml
├── service.yaml
├── ingress.yaml
```

---

# Kubernetes Resources

## Deployment

Responsible for:

- Creating Pods
- Scaling application instances
- Managing updates

Example:

```yaml
apiVersion: apps/v1
kind: Deployment
```

---

## Service

Responsible for:

- Exposing Pods
- Internal networking

Example:

```yaml
kind: Service
```

---

## Ingress

Responsible for:

- External access
- Load balancing
- Routing traffic

Example:

```yaml
kind: Ingress
```

---

# Complete Workflow Execution

```text
Developer Pushes Code
            │
            ▼
GitHub Actions Triggered
            │
            ▼
Checkout Repository
            │
            ▼
Setup Java Environment
            │
            ▼
Maven Build & Test
            │
            ▼
SonarQube Analysis
            │
            ▼
Build Successful?
            │
           Yes
            │
            ▼
Deploy Job Starts
            │
            ▼
Kubernetes Deployment
            │
            ▼
Application Running
```

---

# Repository Structure

```text
project-root/
│
├── .github/
│   └── workflows/
│       └── ci-cd.yml
│
├── src/
│   ├── main/
│   └── test/
│
├── kubernetes/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
│
├── pom.xml
│
└── README.md
```

---

# Advantages of This Pipeline

- Automated Build Process
- Automated Testing
- Code Quality Checks
- Continuous Deployment
- Faster Releases
- Reduced Human Errors
- Kubernetes Integration
- Secure Secret Management

---

# Future Improvements

- Docker Image Build
- Docker Hub Integration
- Helm Charts
- ArgoCD Deployment
- Slack Notifications
- Automated Rollbacks
- Security Scanning
- Multi-Environment Deployment

---

# Interview Explanation

This project implements an end-to-end CI/CD pipeline using GitHub Actions. Whenever code is pushed to the main branch, GitHub Actions automatically builds the Java application using Maven, performs code quality analysis through SonarQube, and deploys the application to a Kubernetes cluster. The pipeline ensures automated testing, quality checks, and continuous delivery, reducing manual effort and improving deployment reliability.

---

# Author

Harsh Saini

DevOps | Cloud | Kubernetes | CI/CD Enthusiast
