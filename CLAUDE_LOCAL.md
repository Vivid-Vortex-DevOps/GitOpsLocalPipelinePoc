# CLAUDE_LOCAL.md

# ROLE

You are a Principal Platform Engineer, DevOps Architect, Kubernetes Engineer, Security Engineer, SRE, and Senior Software Engineer.

You are responsible for designing and implementing a production-inspired, enterprise-grade local platform that mirrors cloud-native patterns without cloud costs.

The platform should be suitable for:

* Portfolio Demonstration
* DevOps Learning
* Platform Engineering Learning
* Kubernetes Learning
* GitOps Learning
* CI/CD Learning
* Interview Preparation

The platform should follow industry best practices while running entirely on the developer's local machine using WSL2 as the Linux runtime.

---

# IMPORTANT WORKING RULES

Before implementing anything:

1. Review the architecture.
2. Identify missing prerequisites.
3. Explain design decisions.
4. Explain tradeoffs.
5. Wait for confirmation before major implementation phases.

Do not generate all code at once.

Proceed in phases.

After every phase:

* Show generated files.
* Show repository structure.
* Explain design decisions.
* Wait for approval.

---

# AI SAFETY REQUIREMENTS

Never fabricate:

* Resource Names
* Secrets
* Passwords
* URLs
* Bitwarden Credentials
* GitHub Tokens
* JFrog Credentials

If information is missing:

* Ask the user
* Explain what is required
* Wait for the response

Do not invent values.

---

# FILE GENERATION RULES

Before generating a file:

1. Check whether the file already exists.
2. Review the existing file.
3. Extend or refactor when appropriate.
4. Preserve user-created content.

Do not overwrite existing work unless explicitly requested.

Special Attention:

The Spring Boot project already exists.

The Go project already exists.

Extend both.

Do not regenerate either.

---

# COST GUARDRAILS

This project runs entirely locally.

There are no cloud billing costs.

The only external service is GitHub (free tier).

However, local resource consumption must be managed:

* Do not overcommit RAM or CPU to Kind/K3s clusters
* Use minimal replica counts
* Use resource limits on all workloads
* Monitor WSL2 memory consumption
* Document minimum and recommended system requirements

---

# IMPLEMENTATION PRIORITY

At every phase prioritize:

1. Working
2. Testable
3. Documented
4. Optimized

in that order.

Avoid spending excessive effort on architecture or documentation before producing working implementations.

---

# LOCAL-FIRST PHILOSOPHY

Everything runs locally inside WSL2 except:

* GitHub (repositories, issues, PRs)
* Bitwarden Secrets Manager (org-level GitHub Actions secrets)

GitHub Actions workflows are defined in GitHub repositories but executed locally via a self-hosted runner in WSL2.

Reference: https://github.com/Vivid-Vortex/Misc/blob/dev_m1_1.0.0/Concepts/DevOps/CICD/RunGithubActionRunnerLocally.md

All other components run on the developer's machine.

No Docker Desktop.

No Windows-native containers.

No cloud subscriptions required.

---

# WSL2 STRATEGY

WSL2 is the foundation of the local platform.

All infrastructure runs inside WSL2 to mimic production Linux nodes.

WSL2 provides:

* Linux kernel
* Docker Engine (native, not Docker Desktop)
* Kubernetes clusters (Kind or K3s)
* All CLI tools
* DevContainer runtime

---

# WSL2 REQUIREMENTS

Distro:

Ubuntu 22.04 LTS (recommended)

or

Ubuntu 24.04 LTS

Configuration:

* systemd enabled
* Sufficient memory allocation (minimum 8GB, recommended 16GB for WSL2)
* Sufficient disk space (minimum 40GB free)

WSL2 Configuration File:

.wslconfig (Windows side)

```ini
[wsl2]
memory=16GB
processors=4
swap=4GB
```

wsl.conf (Linux side)

```ini
[boot]
systemd=true
```

---

# DOCKER STRATEGY

Docker Engine runs natively inside WSL2.

Do NOT use Docker Desktop.

Install Docker Engine directly in WSL2 Ubuntu.

Docker provides:

* Container runtime for Kind/K3s
* Image building
* DevContainer support (VS Code connects to WSL2 Docker)

Docker Compose is used for local application development.

---

# DEVCONTAINER STRATEGY

DevContainers provide a standardized development environment.

The DevContainer runs inside WSL2 Docker.

VS Code on Windows connects to WSL2 Docker to run the DevContainer.

DevContainer includes all required tools:

* Go
* Java 21
* Maven / Gradle
* Git
* kubectl
* Helm
* Kind
* K3s (k3d)
* ArgoCD CLI
* istioctl
* jq
* yq
* curl
* Make

DevContainers live in each repository:

.devcontainer/
  devcontainer.json
  Dockerfile

---

# KUBERNETES STRATEGY

Local Kubernetes replaces cloud-managed Kubernetes (AKS/EKS/GKE).

---

# KIND VS K3S COMPARISON

## Kind (Kubernetes IN Docker) - PRIMARY CHOICE

Pros:

* Uses official Kubernetes (kubeadm-based)
* Identical API behavior to cloud-managed K8s (AKS, EKS, GKE)
* Multi-node support via Docker containers (each node is a container)
* Excellent for testing real K8s behavior
* Supports custom node images
* LoadBalancer support via MetalLB
* Ingress support via standard controllers
* Used by Kubernetes upstream CI itself
* Best at mimicking cloud-managed clusters

Cons:

* Slightly heavier resource usage than K3s
* Requires Docker (already available in WSL2)
* No built-in ingress or load balancer (must install separately)

## K3s (via k3d) - ALTERNATIVE CHOICE

Pros:

* Lightweight (single binary)
* Faster startup
* Built-in Traefik ingress
* Built-in local storage provisioner
* Lower resource consumption
* Good for resource-constrained machines

Cons:

* Uses its own components (not standard kubeadm)
* Some K8s API differences (flannel instead of standard CNI)
* SQLite backend instead of etcd (can configure etcd)
* Traefik instead of standard ingress
* Less representative of production cloud clusters
* Some Helm charts may behave differently

## Decision

Use Kind as the primary cluster runtime.

Kind better mimics cloud-managed Kubernetes.

Skills learned with Kind transfer directly to AKS, EKS, GKE.

K3s remains available as a lightweight alternative for resource-constrained environments.

All scripts and documentation should support both Kind and K3s.

---

# CLUSTER CONFIGURATION

## Initial Setup

Single node cluster.

This is sufficient for all platform components and both applications.

## Future Expansion

After validating with single node:

Test with 2-node cluster (1 control-plane + 1 worker).

This tests scheduling, node affinity, and pod distribution.

## Kind Cluster Configuration

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    kubeadmConfigPatches:
      - |
        kind: InitConfiguration
        nodeRegistration:
          kubeletExtraArgs:
            node-labels: "ingress-ready=true"
    extraPortMappings:
      - containerPort: 80
        hostPort: 80
        protocol: TCP
      - containerPort: 443
        hostPort: 443
        protocol: TCP
```

## Kind 2-Node Configuration (Future)

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    kubeadmConfigPatches:
      - |
        kind: InitConfiguration
        nodeRegistration:
          kubeletExtraArgs:
            node-labels: "ingress-ready=true"
    extraPortMappings:
      - containerPort: 80
        hostPort: 80
        protocol: TCP
      - containerPort: 443
        hostPort: 443
        protocol: TCP
  - role: worker
```

---

# EXISTING REPOSITORIES

Infrastructure Repository (Local):

https://github.com/Vivid-Vortex-DevOps/local-platform-infra.git

Go Service Repository:

https://github.com/Vivid-Vortex-DevOps/go-crud-service.git

Branch: main

Spring Boot Repository:

https://github.com/Vivid-Vortex-DevOps/springboot-crud-service.git

Branch: main

Workspace Repository:

https://github.com/Vivid-Vortex-DevOps/GitOpsLocalPipelinePoc.git

---

# BRANCH STRATEGY

Trunk Based Development (TBD).

main is the single source of truth for all repositories.

ArgoCD automatically syncs from main.

Infrastructure Repository (local-platform-infra):

Branch: main

Go Service Repository (go-crud-service):

Branch: main

Spring Boot Repository (springboot-crud-service):

Branch: main

Workspace Repository (GitOpsLocalPipelinePoc):

Branch: main

Feature branches follow: feature/* pattern from main.

All merges go to main.

No long-lived branches.

---

# LOCAL WORKSPACE STRUCTURE

workspace/
|
├── CLAUDE_LOCAL.md
├── CLAUDE_CLOUD.md
|
├── local-platform-infra/
|
├── go-crud-service/           (main branch)
|
└── springboot-crud-service/   (main branch)

Both the Spring Boot and Go projects already exist with source code, Dockerfiles, Helm charts, GitHub Actions, and documentation.

Do NOT recreate either project.

Extend both existing projects on main.

---

# PRIMARY OBJECTIVE

Build two production-style microservices:

1. Go CRUD Service
2. Spring Boot CRUD Service

Both services should:

* Expose REST APIs
* Support CRUD Operations
* Use PostgreSQL (local, in-cluster)
* Be containerized
* Be deployable to local Kubernetes
* Be deployable through GitOps (ArgoCD)
* Expose Health Endpoints
* Expose Metrics Endpoints
* Support OpenTelemetry
* Include Structured Logging
* Include Unit Tests
* Include Integration Tests

---

# TARGET TECHNOLOGY STACK

Runtime Environment:

* WSL2 (Ubuntu)

Container Runtime:

* Docker Engine (native in WSL2, NOT Docker Desktop)

Development Environment:

* VS Code + DevContainers

Container Platform:

* Kind (primary) / K3s via k3d (alternative)

Container Registry (Local):

* JFrog Container Registry (runs in local K8s)

Container Registry (CI/CD):

* JFrog Container Registry (local) - self-hosted runner pushes directly

Secrets (CI/CD):

* Bitwarden Secrets Manager (org-level, GitHub Actions)

Secrets (Runtime):

* Kubernetes Secrets
* Sealed Secrets (for GitOps-safe secret management)

Database:

* PostgreSQL (Helm chart, in-cluster)

GitOps:

* ArgoCD

Service Mesh:

* Istio

Service Mesh Visualization:

* Kiali

Monitoring:

* Prometheus
* Grafana

Logging:

* Loki + Promtail

Tracing:

* OpenTelemetry
* Jaeger

CI/CD:

* GitHub Actions (self-hosted runner in WSL2)

Containers:

* Docker (WSL2-native)

Ingress:

* NGINX Ingress Controller (Kind)
* MetalLB (LoadBalancer support for Kind)

---

# CONTAINER REGISTRY STRATEGY

## Single Local Registry - JFrog Container Registry

JFrog Container Registry runs inside the Kind cluster.

The self-hosted runner executes on the same WSL2 machine and pushes images directly to JFrog via localhost.

No GHCR or external registry required.

Purpose:

* CI/CD image publishing (self-hosted runner → JFrog)
* Local development image pushes
* Fast image pulls (no internet required after initial push)
* Source of truth for all container images
* Learning JFrog administration

### Image Flow

Local Development:

Developer → Docker Build → JFrog (localhost) → Kind cluster

CI/CD:

GitHub triggers workflow → Self-hosted runner (WSL2) → Docker Build → JFrog (localhost) → ArgoCD syncs → Kind pulls from JFrog

No tunnels, no external registries, no public IP exposure.

---

# NETWORKING STRATEGY

## No Tunnel Required

Self-hosted runner runs on the same WSL2 machine as the Kind cluster.

All CI/CD operations are local (runner → JFrog → Kind).

ArgoCD polls GitHub repositories (outbound connection only).

No inbound connections from the internet to local services are required.

## Local Access

Services are accessed via:

* kubectl port-forward
* Kind extraPortMappings (ports 80/443 mapped to WSL2)
* NodePort services
* MetalLB (optional, for LoadBalancer type services)

## WSL2 to Windows Access

WSL2 services are accessible from Windows via:

* localhost (WSL2 port forwarding is automatic on Windows 11)
* WSL2 IP address (for older Windows versions)

## No Caddy Required

Since the self-hosted runner and all services share the same WSL2 machine:

* No reverse proxy needed
* No tunnel needed
* No public IP exposure needed

If future webhooks from GitHub to local ArgoCD are desired:

* ArgoCD polling is sufficient for this POC
* Document webhook setup with cloudflared as a future enhancement

---

# DATABASE STRATEGY

## PostgreSQL (Local, In-Cluster)

PostgreSQL runs inside the Kind cluster via Helm chart.

Use Bitnami PostgreSQL Helm chart.

### Databases

Create:

go_service_db

springboot_service_db

Keep application databases isolated.

### Credentials

Database credentials are managed via Kubernetes Secrets.

For GitOps-safe storage, use Sealed Secrets.

Credentials do NOT exist in:

* Source Code
* Helm Values (plaintext)
* GitHub Repository (plaintext)

### Schema Management

Applications manage their own schemas:

* Tables
* Indexes
* Migrations

Use Flyway for schema evolution.

PostgreSQL Helm chart manages:

* Server lifecycle
* Storage
* Networking
* Configuration

---

# SECRET MANAGEMENT STRATEGY

## Three-Tier Secret Management

### Tier 1: Bitwarden Secrets Manager (CI/CD)

Purpose: GitHub Actions secrets (used by self-hosted runner)

Scope: Organization-level

Already configured.

Available Secrets:

BW_ACCESS_TOKEN

GITHUB_TOKEN (auto-injected by GitHub)

### Tier 2: Kubernetes Secrets (Runtime)

Purpose: In-cluster application secrets

Scope: Namespace-level

Examples:

* Database credentials
* Application configuration secrets
* JFrog registry credentials

### Tier 3: Sealed Secrets (GitOps-Safe)

Purpose: Encrypted secrets stored in Git

Scope: Cluster-level

Sealed Secrets Controller runs in-cluster.

Secrets are encrypted with the cluster's public key.

Only the Sealed Secrets controller can decrypt them.

Encrypted secrets can be safely committed to Git.

### Secret Flow

CI/CD Secrets:

Bitwarden → GitHub Actions → Self-hosted Runner (WSL2)

Runtime Secrets:

Sealed Secrets (Git) → Sealed Secrets Controller → Kubernetes Secrets → Application Pods

Local Development Secrets:

.env files (gitignored) → Docker Compose → Application

---

# SECURITY STRATEGY

Security remains a first-class requirement even in a local environment.

Security decisions should favor:

* Least Privilege
* Zero Hardcoded Secrets
* RBAC
* Namespace Isolation
* Non-root Containers

---

# RBAC STRATEGY

Implement Kubernetes RBAC.

Use least privilege.

Create dedicated ServiceAccounts for:

* ArgoCD
* Applications
* Monitoring components

Avoid cluster-admin where possible.

Document RBAC decisions.

---

# CONTAINER SECURITY

Implement:

* Non-root Containers
* Minimal Base Images (distroless or Alpine)
* Read-only root filesystem where possible
* Security contexts on all pods
* Resource limits on all containers

---

# ARGOCD STRATEGY

ArgoCD is the GitOps engine.

ArgoCD runs inside the Kind cluster.

ArgoCD watches GitHub repositories for changes.

Manual kubectl deployments should be avoided for application workloads.

Git is the source of truth.

---

# ARGOCD OWNERSHIP

Repository:

local-platform-infra

Owns:

* ArgoCD Installation (Helm)
* ArgoCD Configuration
* ArgoCD Projects
* ArgoCD Applications

ArgoCD should not live inside application repositories.

---

# ARGOCD STRUCTURE

argocd/
|
├── base/
|
├── dev/
|
├── qa/         (config only, not active)
|
├── staging/    (config only, not active)
|
└── prod/       (config only, not active)

Only dev will be active initially.

---

# ARGOCD SYNC STRATEGY

Use automated sync for dev.

ArgoCD polls GitHub repositories.

No webhook required (avoids tunnel/public IP).

Default poll interval: 3 minutes (configurable).

For immediate sync: use ArgoCD CLI or UI to trigger manual sync.

---

# ARGOCD ACCESS

ArgoCD UI is accessed via:

kubectl port-forward svc/argocd-server -n argocd 8080:443

Accessible at: https://localhost:8080

Initial admin password retrieved from Kubernetes secret.

---

# GITOPS DEPLOYMENT FLOW

Developer
→ Feature Branch
→ Pull Request
→ CI Validation (self-hosted runner)
→ Merge To main
→ Docker Build (self-hosted runner)
→ Push to JFrog (localhost)
→ Update image tag in Git
→ ArgoCD Sync (polls Git)
→ Kind Cluster Deployment

No manual deployment should be required.

---

# SERVICE MESH STRATEGY

## Istio

Istio runs inside the Kind cluster.

Installed via Helm through local-platform-infra.

Provides:

* Traffic Management
* mTLS between services
* Observability (metrics, traces)
* Traffic policies

## Kiali

Kiali runs inside the Kind cluster.

Provides:

* Service Topology Visualization
* Traffic Flow
* Health Status

Kiali UI accessed via:

kubectl port-forward svc/kiali -n istio-system 20001:20001

---

# MONITORING STRATEGY

## Prometheus

Primary metrics collection platform.

Installed via kube-prometheus-stack Helm chart.

Collects:

* Cluster Metrics
* Node Metrics
* Pod Metrics
* Container Metrics
* Istio Metrics
* Application Metrics

Retention: Minimal (24h for POC, configurable).

## Grafana

Primary visualization platform.

Installed via kube-prometheus-stack Helm chart.

Dashboards:

* Cluster Health
* Node Health
* Pod Health
* Application Metrics (request rate, error rate, latency)
* Istio Mesh Dashboard

Grafana UI accessed via:

kubectl port-forward svc/grafana -n monitoring 3000:3000

---

# LOGGING STRATEGY

## Loki + Promtail

Replaces Azure Log Analytics and Application Insights logging.

Loki: Log aggregation backend.

Promtail: Log collection agent (DaemonSet).

Installed via Helm in the monitoring namespace.

Logs are queryable through Grafana (Loki data source).

---

# TRACING STRATEGY

## OpenTelemetry + Jaeger

OpenTelemetry: Standard telemetry framework for both applications.

Jaeger: Distributed tracing backend and UI.

Replaces Application Insights tracing.

Both applications instrument:

* Incoming Requests
* Database Calls
* Service Calls
* Custom Spans

Jaeger UI accessed via:

kubectl port-forward svc/jaeger-query -n monitoring 16686:16686

---

# APPLICATION INSTRUMENTATION

Go Service:

OpenTelemetry Go SDK

Spring Boot Service:

OpenTelemetry Java Agent / Micrometer

Both applications expose:

* /health (liveness)
* /ready (readiness)
* /metrics (Prometheus format)

Structured JSON logging with:

* Timestamp
* Log Level
* Service Name
* Request ID
* Trace ID
* Message

---

# JFROG CONTAINER REGISTRY

## Installation

JFrog Container Registry (OSS) runs inside the Kind cluster.

Installed via Helm chart.

Namespace: registry

## Purpose

* Local image registry for development workflow
* Mirror/cache for external images
* Learning JFrog administration and configuration

## Access

JFrog UI accessed via:

kubectl port-forward svc/artifactory -n registry 8082:8082

Registry endpoint (for docker push/pull):

localhost:8082

## Image Naming

Local images:

localhost:8082/go-crud-service:latest

localhost:8082/springboot-crud-service:latest

---

# CI/CD STRATEGY

GitHub Actions is the CI/CD platform.

GitHub Actions workflows are defined in GitHub repositories but executed locally via a self-hosted runner installed in WSL2.

The self-hosted runner has direct access to:

* Docker Engine (builds images locally)
* Kind cluster (via kubeconfig)
* JFrog Container Registry (via localhost)
* All local platform services

Setup Reference: https://github.com/Vivid-Vortex/Misc/blob/dev_m1_1.0.0/Concepts/DevOps/CICD/RunGithubActionRunnerLocally.md

---

# SELF-HOSTED RUNNER SETUP

Install the GitHub Actions self-hosted runner inside WSL2 (Ubuntu).

Follow the WSL2 / Linux setup from the reference guide above.

Runner Configuration:

* Labels: self-hosted, linux, wsl2, local
* Runner group: Default
* Install as systemd service for automatic startup

Workflow target:

```yaml
jobs:
  build:
    runs-on: [self-hosted, linux]
```

The runner polls GitHub for jobs.

No inbound network access required.

---

# CI/CD PHILOSOPHY

Code should move through:

Developer
→ Pull Request
→ CI Validation (self-hosted runner)
→ Merge To main
→ Full CI/CD Pipeline (self-hosted runner)
→ Build + Test + Scan + Package
→ Push to JFrog (localhost)
→ Generate Helm Chart
→ Deploy to dev (ArgoCD)
→ Integration Tests
→ Observability Apply
→ Collect Telemetry

---

# WORKFLOW STRUCTURE

Model after enterprise-grade pipeline with DAG parallelism.

Two workflows per application repository:

## 1. cicd.yaml (on: push to main)

Full CI/CD pipeline with parallel job execution.

## 2. promotions.yaml (on: workflow_dispatch)

Manual promotion workflow for deploying to specific environments.

---

# CICD PIPELINE (cicd.yaml)

Trigger: Push to main

Runs on: [self-hosted, linux]

## Pipeline DAG (jobs with dependencies)

```
Initialize Build Context
    |
    ├──→ Run Unit Tests
    |
    ├──→ Run Integration Tests (testcontainers)
    |
    └──→ Static AppSec Analysis & SBOM
             |
             └──→ Static AppSec Analysis Report
                      |
    ┌────────────────┘
    |
    └──→ Store SBOM (artifact)
    |
    └──→ Run SonarQube Static Analysis (if available, or local linting)
    |
    ▼
Create Semantic Version Tag
    |
    ├──→ Build Service Image
    |
    ├──→ Build Migration Image (if applicable)
    |
    └──→ Image Scan Results
    |
    ▼
Generate Helm Chart
    |
    ▼
Push to JFrog (localhost)
    |
    ▼
Deploy to dev (ArgoCD sync)
    |
    ├──→ Run Integration Tests (post-deploy)
    |
    ├──→ o11y apply dev (observability config)
    |
    ▼
Collect and Publish Telemetry
```

## Job Details

### Initialize Build Context

* Checkout code
* Determine build version
* Set up build environment
* Output build metadata (version, commit SHA, branch)

### Run Unit Tests

* Depends on: Initialize Build Context
* Run unit test suite
* Generate test reports
* Upload test results as artifacts

### Run Integration Tests

* Depends on: Initialize Build Context
* Run integration tests with Docker Compose / testcontainers
* Parallel with Unit Tests

### Static AppSec Analysis & SBOM

* Depends on: Initialize Build Context
* Run static security analysis
* Generate SBOM (Software Bill of Materials)
* Parallel with tests

### Static AppSec Analysis Report

* Depends on: Static AppSec Analysis & SBOM
* Format and publish security report

### Create Semantic Version Tag

* Depends on: Unit Tests, Integration Tests, AppSec Analysis
* All quality gates must pass
* Create semantic version tag (e.g., v1.2.3)
* Tag format: v{major}.{minor}.{patch}-g{short-sha}

### Build Service Image

* Depends on: Create Semantic Version Tag
* Docker build with semantic version tag
* Tag with: latest, semantic version, commit SHA

### Image Scan Results

* Depends on: Build Service Image
* Scan built image for vulnerabilities
* Fail pipeline on critical/high vulnerabilities

### Generate Helm Chart

* Depends on: Build Service Image
* Package Helm chart with updated image tag
* Validate chart (helm lint, helm template)

### Push to JFrog

* Depends on: Generate Helm Chart, Image Scan
* Push Docker image to JFrog (localhost:8082)
* Push Helm chart to JFrog (optional)

```yaml
- name: Push to JFrog
  run: |
    docker tag $IMAGE localhost:8082/$IMAGE:$TAG
    docker push localhost:8082/$IMAGE:$TAG
```

### Deploy to Dev

* Depends on: Push to JFrog
* Trigger ArgoCD sync for dev environment
* Wait for deployment to be healthy
* Generate job summary with:
  * Platform deploy info
  * ArgoCD Application link (localhost:8080)
  * App URL (localhost)

### Run Post-Deploy Integration Tests

* Depends on: Deploy to Dev
* Run integration tests against live deployment
* Validate API endpoints

### o11y Apply Dev

* Depends on: Deploy to Dev
* Apply observability configuration
* Configure dashboards, alerts
* Parallel with integration tests

### Collect and Publish Telemetry

* Depends on: Post-Deploy Integration Tests, o11y Apply Dev
* Final pipeline step
* Collect pipeline metrics
* Publish pipeline summary

---

# PROMOTION PIPELINE (promotions.yaml)

Trigger: workflow_dispatch (manual)

Runs on: [self-hosted, linux]

Purpose: Promote a specific version to a target environment.

## Inputs

* environment: dev / qa / staging / prod
* version: semantic version to deploy

## Pipeline (sequential)

```
Deploy to {environment}
    |
    ▼
Apply o11y for {environment}
```

## Job Summary Output

Each deployment job generates a summary:

```
Platform deploy: service-name → namespace/environment @ version
ArgoCD Application: http://localhost:8080/applications/{app-name}
App URL: http://localhost:{port}
```

---

# PULL REQUEST PIPELINE

Trigger: Pull Requests to main

Runs on: [self-hosted, linux]

Pipeline (subset of cicd, no deployment):

1. Initialize Build Context
2. Run Unit Tests (parallel)
3. Run Integration Tests (parallel)
4. Static AppSec Analysis & SBOM (parallel)
5. Linting
6. Helm Chart Validation (helm lint, helm template)

No build, no push, no deployment.

Quality gates only.

---

# JFROG PUBLISHING FROM RUNNER

The self-hosted runner accesses JFrog via localhost.

No external registry credentials required.

JFrog credentials managed via Kubernetes Secrets or runner environment.

---

# IMAGE TAGGING STRATEGY

Every build creates:

semantic version (source of truth)

commit sha

latest

Examples:

go-crud-service:v1.2.3-ga1b2c3d
go-crud-service:a1b2c3d
go-crud-service:latest

springboot-crud-service:v1.2.3-ga1b2c3d
springboot-crud-service:a1b2c3d
springboot-crud-service:latest

---

# PIPELINE ARTIFACTS

Each pipeline run produces artifacts (uploaded to GitHub Actions):

* Test results (JUnit XML)
* Coverage reports
* SBOM
* Security scan reports
* Helm chart package
* Image scan results
* Pipeline telemetry

---

# PIPELINE JOB SUMMARIES

Every deployment job writes a GitHub Actions job summary containing:

* Platform deploy details (service, namespace, version)
* ArgoCD Application link
* App URL
* Deployment status

This mirrors enterprise-grade patterns for deployment traceability.

---

# INFRASTRUCTURE MANAGEMENT

## No Terraform for Local

Local infrastructure does not use Terraform.

Terraform is for cloud resource provisioning (Azure, AWS, GCP).

Local infrastructure is managed via:

* Shell scripts (bootstrap)
* Helm charts (platform components)
* Kind/K3s configuration files
* kubectl (when necessary)
* Makefile (developer commands)

## Why No Terraform Locally

Terraform manages cloud provider APIs.

Local Kubernetes does not have a cloud provider API.

Using Terraform locally adds complexity without value.

Shell scripts + Helm is the idiomatic approach for local K8s.

## Future Cloud Migration

When migrating to cloud (Azure/AWS/GCP):

Switch to cloud-platform-infra repository.

Terraform manages cloud resources.

Helm charts and ArgoCD configurations remain mostly unchanged.

Application code remains identical.

---

# BOOTSTRAP STRATEGY

## Purpose

A single bootstrap script provisions the entire local platform.

## Bootstrap Process

```
Run bootstrap.sh
    |
    v
Validate Prerequisites (Docker, Kind, kubectl, Helm)
    |
    v
Create Kind Cluster
    |
    v
Install MetalLB (LoadBalancer support)
    |
    v
Install NGINX Ingress Controller
    |
    v
Install Sealed Secrets Controller
    |
    v
Install ArgoCD
    |
    v
Install Istio
    |
    v
Install Kiali
    |
    v
Install Prometheus + Grafana (kube-prometheus-stack)
    |
    v
Install Loki + Promtail
    |
    v
Install Jaeger
    |
    v
Install JFrog Container Registry
    |
    v
Install PostgreSQL
    |
    v
Create Namespaces
    |
    v
Configure ArgoCD Applications
    |
    v
Verify All Components
    |
    v
Print Access Information
    |
    v
Ready
```

## Bootstrap Scripts

bootstrap/
|
├── bootstrap.sh           (full platform setup)
├── bootstrap-cluster.sh   (Kind cluster only)
├── bootstrap-platform.sh  (platform components only)
├── destroy.sh             (full teardown)
├── verify.sh              (health checks)
├── port-forward.sh        (start all port-forwards)
└── README.md

---

# INFRASTRUCTURE REPOSITORY STRUCTURE

local-platform-infra/
|
├── .devcontainer/
|   ├── devcontainer.json
|   └── Dockerfile
|
├── bootstrap/
|   ├── bootstrap.sh
|   ├── bootstrap-cluster.sh
|   ├── bootstrap-platform.sh
|   ├── bootstrap-runner.sh
|   ├── destroy.sh
|   ├── verify.sh
|   ├── port-forward.sh
|   └── README.md
|
├── runner/
|   ├── setup-runner.sh
|   └── README.md
|
├── cluster/
|   ├── kind-single-node.yaml
|   ├── kind-multi-node.yaml
|   ├── k3d-single-node.yaml     (alternative)
|   └── k3d-multi-node.yaml      (alternative)
|
├── helm/
|   ├── argocd/
|   │   └── values.yaml
|   ├── istio/
|   │   ├── base-values.yaml
|   │   └── istiod-values.yaml
|   ├── kiali/
|   │   └── values.yaml
|   ├── prometheus/
|   │   └── values.yaml
|   ├── grafana/
|   │   └── values.yaml          (if separate from kube-prometheus-stack)
|   ├── loki/
|   │   └── values.yaml
|   ├── jaeger/
|   │   └── values.yaml
|   ├── jfrog/
|   │   └── values.yaml
|   ├── postgresql/
|   │   └── values.yaml
|   ├── sealed-secrets/
|   │   └── values.yaml
|   ├── metallb/
|   │   └── values.yaml
|   └── nginx-ingress/
|       └── values.yaml
|
├── argocd/
|   ├── projects/
|   │   ├── platform.yaml
|   │   └── applications.yaml
|   ├── dev/
|   │   ├── go-crud-service.yaml
|   │   └── springboot-crud-service.yaml
|   ├── qa/          (config only)
|   ├── staging/     (config only)
|   └── prod/        (config only)
|
├── namespaces/
|   ├── platform.yaml
|   ├── monitoring.yaml
|   ├── istio-system.yaml
|   ├── argocd.yaml
|   ├── registry.yaml
|   ├── applications-dev.yaml
|   ├── applications-qa.yaml      (future)
|   ├── applications-staging.yaml (future)
|   └── applications-prod.yaml    (future)
|
├── docs/
|   ├── architecture/
|   │   ├── system-architecture.md
|   │   ├── local-vs-cloud.md
|   │   └── kind-vs-k3s.md
|   ├── setup/
|   │   ├── prerequisites.md
|   │   ├── wsl2-setup.md
|   │   ├── docker-wsl2-setup.md
|   │   ├── self-hosted-runner-setup.md
|   │   └── bootstrap-guide.md
|   ├── gitops/
|   │   ├── argocd-design.md
|   │   ├── deployment-flow.md
|   │   └── promotion-flow.md
|   ├── security/
|   │   ├── bitwarden.md
|   │   ├── sealed-secrets.md
|   │   ├── rbac.md
|   │   └── container-security.md
|   ├── observability/
|   │   ├── prometheus.md
|   │   ├── grafana.md
|   │   ├── loki.md
|   │   ├── jaeger.md
|   │   ├── opentelemetry.md
|   │   └── kiali.md
|   ├── operations/
|   │   ├── deployment-guide.md
|   │   ├── bootstrap-guide.md
|   │   ├── cleanup-guide.md
|   │   ├── recovery-guide.md
|   │   ├── port-forwarding-guide.md
|   │   └── troubleshooting-guide.md
|   └── adr/
|       ├── ADR-001-use-kind.md
|       ├── ADR-002-use-argocd.md
|       ├── ADR-003-use-istio.md
|       ├── ADR-004-use-bitwarden.md
|       ├── ADR-005-use-self-hosted-runner.md
|       ├── ADR-006-use-jfrog-local.md
|       ├── ADR-007-use-postgresql-in-cluster.md
|       ├── ADR-008-use-opentelemetry.md
|       ├── ADR-009-use-github-actions.md
|       ├── ADR-010-use-wsl2.md
|       ├── ADR-011-use-sealed-secrets.md
|       └── ADR-012-no-terraform-locally.md
|
├── Makefile
|
├── .github/
|   └── workflows/
|       ├── lint.yml
|       └── validate-helm.yml
|
└── README.md

---

# GO APPLICATION REPOSITORY

Repository:

go-crud-service

Branch: main

Purpose:

Own the Go application.

Responsible for:

* Source Code
* Unit Tests
* Integration Tests
* Dockerfile
* Docker Compose
* Helm Chart
* Application Documentation

---

# GO TARGET STRUCTURE

go-crud-service/ (main branch)
|
├── .devcontainer/
|   ├── devcontainer.json
|   └── Dockerfile
|
├── cmd/
|   └── server/
|       └── main.go
|
├── internal/
|   ├── handler/
|   ├── service/
|   ├── repository/
|   ├── model/
|   ├── config/
|   └── middleware/
|
├── test/
|   ├── unit/
|   └── integration/
|
├── deployment/
|   ├── helm/
|   │   ├── Chart.yaml
|   │   ├── values.yaml
|   │   └── templates/
|   └── values/
|       ├── dev.yaml
|       ├── qa.yaml
|       ├── staging.yaml
|       └── prod.yaml
|
├── Dockerfile
├── docker-compose.yml
├── Makefile
|
├── docs/
|   ├── architecture.md
|   ├── api.md
|   ├── local-development.md
|   ├── docker.md
|   ├── deployment.md
|   ├── testing.md
|   ├── observability.md
|   └── troubleshooting.md
|
├── .github/
|   └── workflows/
|       ├── cicd.yaml
|       └── promotions.yaml
|
└── README.md

---

# SPRING BOOT REPOSITORY

Repository:

springboot-crud-service

Branch: main

Purpose:

Own the Spring Boot application.

The existing starter.spring.io project must be reused.

Do not regenerate it.

Extend it on main.

---

# SPRING BOOT TARGET STRUCTURE

springboot-crud-service/ (main branch)
|
├── .devcontainer/
|   ├── devcontainer.json
|   └── Dockerfile
|
├── src/
|   ├── main/
|   │   ├── java/
|   │   └── resources/
|   │       ├── application.yml
|   │       ├── application-dev.yml
|   │       ├── application-qa.yml
|   │       ├── application-staging.yml
|   │       └── application-prod.yml
|   └── test/
|
├── deployment/
|   ├── helm/
|   │   ├── Chart.yaml
|   │   ├── values.yaml
|   │   └── templates/
|   └── values/
|       ├── dev.yaml
|       ├── qa.yaml
|       ├── staging.yaml
|       └── prod.yaml
|
├── Dockerfile
├── docker-compose.yml
├── Makefile
|
├── docs/
|   ├── architecture.md
|   ├── api.md
|   ├── local-development.md
|   ├── docker.md
|   ├── deployment.md
|   ├── testing.md
|   ├── observability.md
|   └── troubleshooting.md
|
├── .github/
|   └── workflows/
|       ├── cicd.yaml
|       └── promotions.yaml
|
└── README.md

---

# LOCAL DEVELOPMENT STRATEGY

Each application repository supports local development via Docker Compose.

Docker Compose starts:

* Application
* PostgreSQL

Do not run platform components locally via Docker Compose.

Platform components (ArgoCD, Istio, Prometheus, etc.) run in the Kind cluster.

---

# APPLICATION CONFIGURATION STRATEGY

Applications support:

* Local Development (Docker Compose, application-dev profile)
* Kubernetes Deployment (Helm, environment-specific profiles)

Configuration is environment-driven.

Use Spring profiles / Go environment variables.

Avoid hardcoded values.

---

# ENVIRONMENT STRATEGY

Create architecture support for:

* dev
* qa
* staging
* prod

Generate:

* Helm Values Files
* ArgoCD Applications
* GitHub Actions Deployment Workflows

for all four environments.

However:

Only provision dev during the initial local POC.

QA, Staging, and PROD exist only as configuration files.

The design must allow future enablement with minimal changes.

---

# ENVIRONMENT PROMOTION FLOW

dev → qa → staging → prod

Deployment promotion should be documented.

Promotion workflows should be generated (config only for non-dev).

GitOps principles remain the source of truth.

---

# NAMESPACE STRATEGY

Create dedicated namespaces:

platform (if needed for shared platform components)

monitoring (Prometheus, Grafana, Loki, Jaeger)

istio-system (Istio control plane)

argocd (ArgoCD)

registry (JFrog Container Registry)

applications-dev (Go + Spring Boot services)

Future:

applications-qa

applications-staging

applications-prod

---

# INGRESS STRATEGY

Use NGINX Ingress Controller for Kind.

Istio Ingress Gateway for service mesh traffic.

Traffic flow:

External (localhost) → NGINX Ingress → Istio Gateway → Application Services

---

# HEALTH CHECKS

Implement:

Readiness Probes

Liveness Probes

Startup Probes where appropriate

Applications must expose health endpoints.

---

# RESOURCE MANAGEMENT

Define resource limits for all workloads.

Local resources are constrained.

Recommended limits (adjustable):

Platform Components:

* ArgoCD: 256Mi-512Mi memory
* Prometheus: 512Mi-1Gi memory
* Grafana: 256Mi memory
* Loki: 256Mi memory
* Jaeger: 256Mi memory
* Istio: 256Mi-512Mi memory
* JFrog: 512Mi-1Gi memory
* PostgreSQL: 256Mi-512Mi memory

Applications:

* Go Service: 64Mi-128Mi memory
* Spring Boot Service: 256Mi-512Mi memory

Total estimated cluster memory: 4-6GB

---

# PLATFORM LIFECYCLE

## Provision

Run bootstrap.sh

Everything is created.

## Operate

Applications deploy via ArgoCD.

Monitor via Grafana.

Debug via Jaeger, Kiali.

## Destroy

Run destroy.sh

Kind cluster deleted.

All resources gone.

No orphaned processes.

## Recreate

Run bootstrap.sh again.

Full platform restored.

---

# DESTROY STRATEGY

Destroying the local platform:

```bash
kind delete cluster --name vvd-local
```

This removes:

* All namespaces
* All workloads
* All persistent volumes
* All services
* The entire cluster

No orphaned resources remain.

No billing implications.

---

# RECOVERY STRATEGY

The platform is fully recoverable from:

* Git Repositories
* Bootstrap Scripts

Recovery process:

1. Clone repositories
2. Run bootstrap.sh
3. ArgoCD syncs applications from Git
4. Platform restored

No external state required.

---

# MAKEFILE STRATEGY

Each repository contains a Makefile as the single entry point.

## Infrastructure Makefile

```makefile
setup:          bootstrap the full platform
cluster:        create Kind cluster only
platform:       install platform components only
destroy:        tear down everything
verify:         health check all components
port-forward:   start all port-forwards
status:         show cluster status
```

## Application Makefile

```makefile
build:          compile the application
test:           run unit tests
test-integration: run integration tests
lint:           run linters
docker-build:   build Docker image
docker-push:    push to local JFrog
compose-up:     start with Docker Compose
compose-down:   stop Docker Compose
helm-lint:      validate Helm chart
helm-template:  render Helm templates
deploy-local:   deploy to local Kind cluster
```

---

# PORT FORWARDING GUIDE

Since Kind does not have a cloud load balancer:

| Service     | Command                                                              | URL                     |
|-------------|----------------------------------------------------------------------|-------------------------|
| ArgoCD      | kubectl port-forward svc/argocd-server -n argocd 8080:443           | https://localhost:8080   |
| Grafana     | kubectl port-forward svc/grafana -n monitoring 3000:3000             | http://localhost:3000    |
| Prometheus  | kubectl port-forward svc/prometheus -n monitoring 9090:9090          | http://localhost:9090    |
| Jaeger      | kubectl port-forward svc/jaeger-query -n monitoring 16686:16686      | http://localhost:16686   |
| Kiali       | kubectl port-forward svc/kiali -n istio-system 20001:20001           | http://localhost:20001   |
| JFrog       | kubectl port-forward svc/artifactory -n registry 8082:8082           | http://localhost:8082    |
| Go Service  | kubectl port-forward svc/go-crud-service -n applications-dev 8081:80 | http://localhost:8081    |
| Spring Boot | kubectl port-forward svc/springboot-crud-service -n applications-dev 8082:80 | http://localhost:8082 |

The port-forward.sh script starts all of these in the background.

---

# CLOUD MIGRATION PATH

This local platform is designed to migrate to cloud with minimal changes.

## What Changes When Moving to Cloud

| Local                      | Cloud (Azure)                    |
|----------------------------|----------------------------------|
| Kind cluster               | AKS                              |
| In-cluster PostgreSQL      | Azure PostgreSQL Flexible Server |
| JFrog (local)              | ACR or GHCR                      |
| Self-hosted runner (WSL2)  | GitHub-hosted runners             |
| Kubernetes Secrets         | Azure Key Vault                  |
| Sealed Secrets             | Azure Key Vault + CSI Driver     |
| Loki + Promtail            | Azure Log Analytics              |
| Jaeger                     | Application Insights             |
| Bootstrap scripts          | Terraform                        |
| MetalLB                    | Azure Load Balancer              |
| NGINX Ingress              | Azure Application Gateway (opt)  |

## What Does NOT Change

* Application source code
* Dockerfiles
* Helm chart templates (mostly)
* ArgoCD configuration (mostly)
* GitHub Actions workflows (change runs-on from self-hosted to GitHub-hosted)
* Istio configuration
* Prometheus/Grafana dashboards
* OpenTelemetry instrumentation
* GitOps principles
* RBAC patterns

---

# SYSTEM REQUIREMENTS

## Minimum

* Windows 10/11 with WSL2
* 16GB RAM (8GB allocated to WSL2)
* 4 CPU cores
* 40GB free disk space
* Internet connection (for GitHub, image pulls)

## Recommended

* Windows 11
* 32GB RAM (16GB allocated to WSL2)
* 8 CPU cores
* 80GB free disk space
* SSD storage

## Why These Requirements

Kind cluster + all platform components require significant memory.

Estimated memory breakdown:

* Kind control plane: ~500MB
* Istio: ~500MB
* ArgoCD: ~500MB
* Prometheus + Grafana: ~1GB
* Loki + Jaeger: ~500MB
* PostgreSQL: ~500MB
* JFrog: ~1GB
* Applications: ~500MB
* System overhead: ~1GB

Total: ~6GB minimum for the cluster

---

# IMPLEMENTATION PHASES

## PHASE 1: Architecture Review

* Review requirements
* Identify gaps
* Validate local design
* Kind vs K3s decision confirmation
* System requirements validation

## PHASE 2: Prerequisites

* WSL2 setup and validation
* Docker Engine installation in WSL2
* Tool installation (kubectl, Helm, Kind, etc.)
* Git configuration
* Repository setup (clone, branch creation)

## PHASE 3: Repository Structure

* Generate folder structures for all repositories
* Generate DevContainer configurations
* Generate Makefiles
* Generate documentation structure

## PHASE 4: Bootstrap Foundation

* Kind cluster configuration
* Bootstrap scripts
* Namespace creation
* Basic cluster validation

## PHASE 5: Platform Components

* Install ArgoCD
* Install Istio + Kiali
* Install Prometheus + Grafana
* Install Loki + Promtail
* Install Jaeger
* Install JFrog Container Registry
* Install PostgreSQL
* Install Sealed Secrets
* Install NGINX Ingress + MetalLB

## PHASE 6: Go Service

* Extend existing Go application (main branch)
* Adapt for local K8s deployment
* Validate CRUD APIs, PostgreSQL integration, OpenTelemetry
* Validate Docker and Helm chart for local environment

## PHASE 7: Spring Boot Service

* Extend existing Spring Boot project (main branch)
* Adapt for local K8s deployment
* Validate CRUD APIs, PostgreSQL integration, OpenTelemetry
* Validate Docker and Helm chart for local environment

## PHASE 8: CI/CD

* Self-hosted runner setup in WSL2
* GitHub Actions workflows (runs-on: self-hosted)
* PR validation pipelines
* Build and publish pipelines (push to local JFrog)
* Helm validation

## PHASE 9: GitOps

* ArgoCD applications
* Environment structure
* Deployment flow
* Sealed Secrets for credentials

## PHASE 10: Observability

* OpenTelemetry integration
* Prometheus metrics
* Grafana dashboards
* Loki log aggregation
* Jaeger tracing
* Kiali service mesh visualization

## PHASE 11: Security

* Sealed Secrets
* RBAC
* Container security
* Namespace isolation

## PHASE 12: Destruction and Recovery

* Destroy script
* Verification script
* Recovery documentation

## PHASE 13: Documentation

* Complete all documentation
* Architecture documentation
* Operational guides
* ADRs

## PHASE 14: Final Validation

* Full platform review
* End-to-end deployment test
* Observability validation
* Documentation review

---

# GIT STRATEGY

Use Trunk Based Development (TBD).

main is the single source of truth for all repositories.

ArgoCD automatically syncs from main.

All Repositories:

Base branch: main

Feature branches: feature/*

All merges go to main. No long-lived branches.

---

# NAMING CONVENTIONS

Cluster:

vvd-local

Namespaces:

argocd
istio-system
monitoring
registry
applications-dev
applications-qa (future)
applications-staging (future)
applications-prod (future)

Helm Releases:

argocd
istiod
istio-base
kiali
kube-prometheus-stack
loki
jaeger
jfrog
postgresql
sealed-secrets
nginx-ingress
metallb

---

# DOCUMENTATION PHILOSOPHY

Documentation is a first-class deliverable.

The platform should be understandable by:

* Developers
* DevOps Engineers
* SREs
* Platform Engineers
* Interviewers

Documentation should answer:

What, Why, How, When, Where

---

# DOCUMENTATION REQUIREMENTS

Generate documentation for:

* System Architecture
* Local vs Cloud comparison
* Kind vs K3s comparison
* WSL2 Setup Guide
* Docker (WSL2) Setup Guide
* Self-Hosted Runner Setup Guide
* Bootstrap Process
* ArgoCD Design
* Istio Design
* Observability Stack
* Security Design
* GitOps Flow
* Port Forwarding Guide
* Troubleshooting Guide
* Recovery Guide
* Cloud Migration Guide
* All ADRs

---

# ADR REQUIREMENTS

ADR-001-use-kind.md
ADR-002-use-argocd.md
ADR-003-use-istio.md
ADR-004-use-bitwarden.md
ADR-005-use-self-hosted-runner.md
ADR-006-use-jfrog-local.md
ADR-007-use-postgresql-in-cluster.md
ADR-008-use-opentelemetry.md
ADR-009-use-github-actions.md
ADR-010-use-wsl2.md
ADR-011-use-sealed-secrets.md
ADR-012-no-terraform-locally.md

---

# SUCCESS CRITERIA

The project is successful when:

* Kind cluster runs all platform components
* ArgoCD deploys applications from Git
* Self-hosted runner executes GitHub Actions workflows locally
* JFrog serves as the single container registry (CI/CD and runtime)
* PostgreSQL runs in-cluster
* Istio manages service mesh traffic
* Kiali visualizes service topology
* Prometheus collects metrics
* Grafana visualizes metrics
* Loki aggregates logs
* Jaeger provides distributed tracing
* OpenTelemetry instruments both applications
* Sealed Secrets manages credentials in Git
* Applications expose health, metrics, and traces
* The environment can be destroyed with one command
* The platform can be recreated with one command
* Documentation explains every major decision
* The project demonstrates enterprise-grade DevOps practices locally

All of this runs on a single developer laptop using WSL2.

No cloud subscription required.

No Docker Desktop required.

No paid services required (except GitHub free tier).

---

# FINAL INSTRUCTION

Do not optimize for speed.

Optimize for:

Correctness

Maintainability

Reproducibility

Security

Documentation

Architectural Quality

Long-Term Learning Value

When in doubt:

Choose the solution that best demonstrates real-world enterprise engineering practices while remaining practical for a local environment.

---

# IMPLEMENTATION COMPLETION REQUIREMENTS

The project is NOT considered ready for local deployment until all repositories have been fully generated, reviewed, committed, and pushed.

Follow this order strictly.

Phase A:
- Generate repository structures
- Generate source code
- Generate bootstrap scripts
- Generate Helm value files
- Generate GitHub Actions
- Generate ArgoCD configurations
- Generate documentation

Phase B:
- Review generated code
- Review architecture
- Review bootstrap scripts
- Review security
- Review GitHub Actions
- Review Helm charts
- Review documentation

Phase C:
- Commit changes
- Push changes to repositories
- Verify repository structures
- Verify documentation completeness

Phase D:
- Perform local validation
- Validate Docker Compose
- Validate application startup
- Validate tests
- Validate build pipelines
- Validate Helm chart rendering
- Validate Kind cluster creation

Phase E:
- Review system requirements
- Verify WSL2 readiness
- Obtain explicit user approval

Phase F:
- Bootstrap local platform
- Deploy to Kind cluster

Do NOT deploy before completing all previous phases.
