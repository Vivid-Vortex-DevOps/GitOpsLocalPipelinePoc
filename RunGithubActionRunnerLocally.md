# GitHub Self-Hosted Runner Guide (Windows, Linux, WSL, and macOS)

## Platform Comparison

| Platform                           | Recommended          | Best For                                                         | Notes                            |
| ---------------------------------- | -------------------- | ---------------------------------------------------------------- | -------------------------------- |
| Windows                            | ✅ Yes                | Local Windows applications, Docker Desktop, Visual Studio builds | Simplest setup for Windows users |
| Linux                              | ✅ Highly Recommended | CI/CD, Docker, Kubernetes, Server workloads                      | Most common production setup     |
| WSL2 (Windows Subsystem for Linux) | ✅ Yes                | Linux tooling while staying on Windows                           | Great compromise for developers  |
| macOS                              | ✅ Yes                | iOS/macOS builds, Xcode projects                                 | Required for native Apple builds |

---

# Overview

GitHub officially supports **self-hosted runners** on:

* Windows
* Linux
* macOS

A self-hosted runner is a lightweight agent installed on your own machine that executes GitHub Actions workflows.

Instead of GitHub creating a temporary cloud VM, jobs run directly on your machine.

Typical use cases:

* Accessing local Docker containers
* Accessing internal services
* Publishing to local repositories (Artifactory/Nexus)
* Accessing VPN-protected resources
* Running platform-specific builds
* Avoiding GitHub-hosted runner minute limits

---

# Architecture

```text
GitHub Workflow Triggered
          │
          ▼
GitHub Actions Service
          │
          ▼
Self-Hosted Runner Polls GitHub
          │
          ▼
Job Assigned
          │
          ▼
Runner Executes Locally
          │
          ▼
Access Local Services
          │
          ▼
Build / Test / Deploy
```

Important:

```text
localhost = The Machine Hosting The Runner
```

If the runner is installed on:

* Windows → localhost refers to Windows
* Linux → localhost refers to Linux
* WSL → localhost refers to WSL
* macOS → localhost refers to macOS

---

---

# Windows Setup

## Step 1: Create Runner

Navigate to:

```text
Repository
 └── Settings
      └── Actions
           └── Runners
                └── New Self-hosted Runner
```

Choose:

```text
OS: Windows
Architecture: x64
```

---

## Step 2: Install Runner

```powershell
mkdir actions-runner
cd actions-runner
```

Download:

```powershell
Invoke-WebRequest `
  -Uri https://github.com/actions/runner/releases/download/v2.x.x/actions-runner-win-x64-2.x.x.zip `
  -OutFile actions-runner-win-x64.zip
```

Extract:

```powershell
Add-Type -AssemblyName System.IO.Compression.FileSystem

[System.IO.Compression.ZipFile]::ExtractToDirectory(
    "$PWD/actions-runner-win-x64.zip",
    "$PWD"
)
```

Configure:

```powershell
.\config.cmd `
  --url https://github.com/YOUR_ORG/YOUR_REPO `
  --token YOUR_TOKEN
```

Run:

```powershell
.\run.cmd
```

---

## Install As Service

```powershell
.\svc.cmd install
.\svc.cmd start
```

Verify:

```powershell
Get-Service actions.runner*
```

---

## Workflow Example

```yaml
jobs:
  build:
    runs-on: self-hosted

    steps:
      - uses: actions/checkout@v4
```

---

---

# Linux Setup (Ubuntu Example)

## Prerequisites

```bash
sudo apt update
sudo apt install -y curl tar
```

---

## Create Runner Directory

```bash
mkdir actions-runner
cd actions-runner
```

---

## Download Runner

```bash
curl -o actions-runner-linux-x64.tar.gz -L \
https://github.com/actions/runner/releases/download/v2.x.x/actions-runner-linux-x64-2.x.x.tar.gz
```

---

## Extract

```bash
tar xzf actions-runner-linux-x64.tar.gz
```

---

## Configure

```bash
./config.sh \
  --url https://github.com/YOUR_ORG/YOUR_REPO \
  --token YOUR_TOKEN
```

---

## Run

```bash
./run.sh
```

---

## Install As Service

```bash
sudo ./svc.sh install
sudo ./svc.sh start
```

Check status:

```bash
sudo systemctl status actions.runner.*
```

---

## Why Linux Is Preferred For CI

* Lowest resource consumption
* Native Docker support
* Easier automation
* Better Kubernetes integration
* Most CI/CD examples target Linux

For production runners, Linux is usually the preferred choice.

---

---

# WSL2 Setup (Windows + Linux)

## When To Use WSL

Use WSL when:

* You primarily work on Windows
* Your build tools require Linux
* You want Linux containers and tooling

Examples:

* Python
* Node.js
* Docker
* Terraform
* Kubernetes
* Ansible

---

## Install WSL

```powershell
wsl --install
```

Install Ubuntu:

```powershell
wsl --install -d Ubuntu
```

---

## Inside Ubuntu

Follow the exact same Linux runner installation steps.

---

## Important Networking Notes

If your workflow needs:

```text
Windows Docker Desktop
Windows Artifactory
Windows Nexus
```

then networking becomes important.

WSL2 can usually access Windows services through:

```bash
curl http://host.docker.internal:8082
```

or

```bash
curl http://<windows-ip>:8082
```

Test connectivity before relying on it.

---

## Recommended WSL Use Cases

| Scenario                    | Recommendation       |
| --------------------------- | -------------------- |
| Java + Maven                | ✅ Excellent          |
| Python Builds               | ✅ Excellent          |
| Docker Builds               | ✅ Excellent          |
| Kubernetes                  | ✅ Excellent          |
| Windows GUI Applications    | ❌ Not ideal          |
| Visual Studio Native Builds | ❌ Use Windows Runner |

---

---

# macOS Setup

## Why Use macOS Runners

Required for:

* Xcode
* iOS builds
* macOS application builds
* Swift projects

Apple licensing requires macOS for these workloads.

---

## Create Directory

```bash
mkdir actions-runner
cd actions-runner
```

---

## Download Runner

```bash
curl -o actions-runner-osx-x64.tar.gz -L \
https://github.com/actions/runner/releases/download/v2.x.x/actions-runner-osx-x64-2.x.x.tar.gz
```

For Apple Silicon:

```bash
curl -o actions-runner-osx-arm64.tar.gz -L \
https://github.com/actions/runner/releases/download/v2.x.x/actions-runner-osx-arm64-2.x.x.tar.gz
```

---

## Extract

```bash
tar xzf actions-runner-osx-arm64.tar.gz
```

---

## Configure

```bash
./config.sh \
  --url https://github.com/YOUR_ORG/YOUR_REPO \
  --token YOUR_TOKEN
```

---

## Run

```bash
./run.sh
```

---

## Install As Launch Service

```bash
./svc.sh install
./svc.sh start
```

---

---

# Using Local Services

Suppose:

```text
Artifactory -> localhost:8082
Nexus       -> localhost:8081
Postgres    -> localhost:5432
Redis       -> localhost:6379
```

Your workflow can directly access them.

Example:

```yaml
- name: Upload Package
  run: |
    twine upload \
      --repository-url http://localhost:8082/artifactory/api/pypi/pypi-local \
      dist/*
```

---

# Security Considerations

## Private Repositories

Generally safe.

Recommended:

```yaml
on:
  workflow_dispatch:
```

or

```yaml
on:
  push:
```

---

## Public Repositories

Be extremely careful.

Potential risk:

```yaml
on:
  pull_request:
```

A malicious PR could execute arbitrary commands on your machine.

Avoid unless you understand the implications.

---

# Practical Limitations

| Concern                | Reality                   |
| ---------------------- | ------------------------- |
| Machine must stay on   | Yes                       |
| Internet required      | Yes                       |
| Docker must be running | If workflow depends on it |
| Maintenance            | Your responsibility       |
| Runner upgrades        | Manual or scripted        |
| Team dependency        | Not ideal                 |
| Resource usage         | Consumes local CPU/RAM    |

---

# Advantages

✅ Local service access

✅ No GitHub Actions minute limits

✅ Faster builds with cached dependencies

✅ Complete control of environment

✅ Access to VPN/internal resources

✅ Ideal for POCs and experimentation

✅ Supports Docker, Kubernetes, Terraform, Python, Java, Node.js, etc.

---

# When NOT To Use Self-Hosted Runners

Avoid for:

* Mission-critical CI/CD
* 24×7 build availability
* Large development teams
* High-security environments
* Shared corporate CI infrastructure

Instead consider:

* GitHub-hosted runners
* Dedicated Linux build servers
* Kubernetes-based runners
* Cloud VM runners

---

# Realistic Verdict

For personal projects, learning, and proofs of concept:

✅ Excellent choice

Typical workflow:

1. Start laptop
2. Start Docker Desktop
3. Ensure Artifactory/Nexus is running
4. Trigger workflow manually
5. Runner executes locally
6. Artifacts are published successfully

For production-grade CI/CD, prefer dedicated infrastructure instead of relying on a personal laptop.

---

# Recommendation by Use Case

| Use Case                           | Best Runner            |
| ---------------------------------- | ---------------------- |
| Java + Maven + Docker              | Linux                  |
| Python Packaging                   | Linux                  |
| Node.js                            | Linux                  |
| Kubernetes                         | Linux                  |
| Terraform                          | Linux                  |
| Visual Studio C#                   | Windows                |
| Windows Desktop Apps               | Windows                |
| iOS Development                    | macOS                  |
| macOS Apps                         | macOS                  |
| Windows User Needing Linux Tooling | WSL2                   |
| Personal POC                       | Any Platform           |
| Enterprise CI/CD                   | Dedicated Linux Server |
