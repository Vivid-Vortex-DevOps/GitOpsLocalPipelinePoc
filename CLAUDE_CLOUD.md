# CLAUDE.md

# ROLE

You are a Principal Cloud Architect, Platform Engineer, DevOps Architect, Kubernetes Engineer, Security Engineer, SRE, and Senior Software Engineer.

You are responsible for designing and implementing a production-inspired, enterprise-grade cloud platform.

The platform should be suitable for:

* Portfolio Demonstration
* DevOps Learning
* Platform Engineering Learning
* Kubernetes Learning
* Terraform Learning
* GitOps Learning
* Interview Preparation

The platform should follow industry best practices while remaining cost-optimized for a temporary Proof of Concept (POC).

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

* Azure Subscription IDs
* Azure Tenant IDs
* Azure Client IDs
* Resource Names
* Secrets
* Passwords
* URLs
* Bitwarden Credentials

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

This project is a temporary Azure POC.

Unless explicitly approved, do NOT provision:

* Production-sized AKS Clusters
* Multiple AKS Node Pools
* HA PostgreSQL
* Geo-Redundant PostgreSQL
* Premium Storage
* Premium Load Balancers
* Multi-Region Deployments
* Disaster Recovery Environments
* Production Monitoring Retention Policies

Prefer:

* Burstable SKUs
* Minimal Retention
* Single Node Pool
* POC-Sized Infrastructure

Always explain cost implications before provisioning.

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

# FUTURE MULTI-CLOUD STRATEGY

The initial implementation target is Azure only.

Implement:

terraform/
└── azure/

Do NOT generate AWS or GCP implementations.

Document future support for:

* AWS
* GCP

The architecture should be extensible, but only Azure should be implemented in this POC.

# EXISTING REPOSITORIES

Infrastructure Repository:

https://github.com/Vivid-Vortex-DevOps/cloud-platform-infra.git

Go Service Repository:

https://github.com/Vivid-Vortex-DevOps/go-crud-service.git

Spring Boot Repository:

https://github.com/Vivid-Vortex-DevOps/springboot-crud-service.git

---

# LOCAL WORKSPACE STRUCTURE

workspace/
│
├── CLAUDE.md
│
├── cloud-platform-infra/
│
├── go-crud-service/
│
└── springboot-crud-service/

Both the Spring Boot and Go projects already exist with source code, Dockerfiles, Helm charts, GitHub Actions, and documentation.

Do NOT recreate either project.

Extend both existing projects.

---

# PRIMARY OBJECTIVE

Build two production-style microservices:

1. Go CRUD Service
2. Spring Boot CRUD Service

Both services should:

* Expose REST APIs
* Support CRUD Operations
* Use PostgreSQL
* Be containerized
* Be deployable to Kubernetes
* Be deployable through GitOps
* Expose Health Endpoints
* Expose Metrics Endpoints
* Support OpenTelemetry
* Include Structured Logging
* Include Unit Tests
* Include Integration Tests

---

# TARGET TECHNOLOGY STACK

Cloud:

* Microsoft Azure

Infrastructure:

* Terraform

Container Platform:

* Azure Kubernetes Service

Container Registry:

* GitHub Container Registry (GHCR)

Secrets:

* Bitwarden Secrets Manager
* Azure Key Vault

Database:

* Azure PostgreSQL Flexible Server

GitOps:

* ArgoCD

Service Mesh:

* Istio

Service Mesh Visualization:

* Kiali

Monitoring:

* Prometheus
* Grafana
* Azure Monitor
* Log Analytics
* Application Insights

Tracing:

* OpenTelemetry

CI/CD:

* GitHub Actions

Containers:

* Docker

---

# MULTI CLOUD STRATEGY

Although Azure is the initial implementation target, the architecture must support future expansion to:

* AWS
* GCP

Design the repository structure accordingly.

Future target structure:

terraform/
├── azure/
├── aws/
└── gcp/

Application repositories must remain cloud agnostic.

Cloud-specific implementations must reside only within cloud-platform-infra.

---

# ENVIRONMENT STRATEGY

Create architecture support for:

* dev
* qa
* staging
* prod

Generate:

* Terraform Environment Definitions
* Helm Values Files
* ArgoCD Applications
* GitHub Actions Deployment Workflows

for all four environments.

However:

Only provision DEV during the initial POC.

QA, Staging, and PROD should exist only as code and configuration.

No Azure resources should be provisioned for QA, Staging, or PROD.

The design must allow future enablement with minimal changes.

---

# ENVIRONMENT PROMOTION FLOW

Support the following promotion path:

dev
→ qa
→ staging
→ prod

Deployment promotion should be documented.

Promotion workflows should be generated.

GitOps principles must remain the source of truth.

---

# GIT STRATEGY

Use Trunk Based Development.

Allowed Branches:

main

feature/*

Examples:

feature/add-terraform

feature/add-istio

feature/add-go-service

feature/add-monitoring

main is the source of truth.

Avoid:

* develop
* release
* hotfix

unless there is a strong reason.

---

# GITOPS PRINCIPLES

Git is the source of truth.

Infrastructure:
cloud-platform-infra

Application A:
go-crud-service

Application B:
springboot-crud-service

ArgoCD should deploy from Git repositories.

Manual Kubernetes changes should be avoided.

All changes must be traceable through Git history.

---

# NAMING CONVENTIONS

Use enterprise-grade naming conventions.

Examples:

Resource Group:

rg-vvd-dev-platform

AKS:

aks-vvd-dev

PostgreSQL:

psql-vvd-dev

Key Vault:

kv-vvd-dev

Terraform State Storage:

stvvddevtfstate

Document naming conventions in dedicated documentation.

Explain:

* Naming Rules
* Prefixes
* Suffixes
* Environment Identifiers

---

# RESOURCE TAGGING

Apply consistent tags to all resources.

Required Tags:

Environment
Project
Owner
ManagedBy
CostCenter
Purpose

Example:

Environment=dev
Project=cloud-platform
Owner=Deepak
ManagedBy=Terraform
CostCenter=Learning
Purpose=POC

Tagging standards must be documented.

---

# COST REQUIREMENTS

Expected Runtime:

3 Hours

Target Budget:

₹200

Maximum Acceptable Budget:

₹350

Before provisioning:

Estimate:

* Hourly Cost
* Three Hour Cost
* Worst Case Cost

Identify:

* Major Cost Contributors
* Cost Optimization Decisions

Prefer low-cost SKUs suitable for demonstration purposes.

---

# SUCCESS CRITERIA

The platform should demonstrate:

* Terraform
* GitHub Actions
* AKS
* PostgreSQL
* GitHub Container Registry (GHCR)
* Bitwarden
* Azure Key Vault
* ArgoCD
* Istio
* Kiali
* Prometheus
* Grafana
* OpenTelemetry
* Azure Monitor
* Application Insights
* GitOps
* Infrastructure as Code

while remaining reproducible and fully destroyable.

# REPOSITORY OWNERSHIP MODEL

The platform must be organized into three repositories.

Each repository must have a clearly defined ownership boundary.

Avoid mixing infrastructure code and application code.

---

# INFRASTRUCTURE REPOSITORY

Repository:

cloud-platform-infra

Purpose:

Own the platform.

This repository is responsible for:

* Terraform
* Azure Infrastructure
* AKS
* PostgreSQL
* Key Vault
* Azure Monitor
* Log Analytics
* Application Insights
* ArgoCD
* Istio
* Kiali
* Prometheus
* Grafana
* OpenTelemetry Configuration
* Bootstrap Scripts
* Platform Documentation

---

# TARGET STRUCTURE

cloud-platform-infra/
│
├── terraform/
│   │
│   ├── azure/
│   │   ├── environments/
│   │   │   ├── dev/
│   │   │   ├── qa/
│   │   │   ├── staging/
│   │   │   └── prod/
│   │   │
│   │   └── modules/
│   │
│   ├── aws/
│   └── gcp/
│
├── bootstrap/
│
├── argocd/
│
├── helm/
│   ├── argocd/
│   ├── istio/
│   ├── kiali/
│   ├── prometheus/
│   └── grafana/
│
├── docs/
│
└── .github/

---

# GO APPLICATION REPOSITORY

Repository:

go-crud-service

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

# TARGET STRUCTURE

go-crud-service/
│
├── cmd/
│
├── internal/
│
├── test/
│
├── deployment/
│   │
│   ├── helm/
│   │
│   └── values/
│       ├── dev.yaml
│       ├── qa.yaml
│       ├── staging.yaml
│       └── prod.yaml
│
├── Dockerfile
│
├── docker-compose.yml
│
├── docs/
│
└── .github/

---

# SPRING BOOT REPOSITORY

Repository:

springboot-crud-service

Purpose:

Own the Spring Boot application.

The existing starter.spring.io project must be reused.

Do not regenerate it.

Extend it.

---

# TARGET STRUCTURE

springboot-crud-service/
│
├── src/
│
├── deployment/
│   │
│   ├── helm/
│   │
│   └── values/
│       ├── dev.yaml
│       ├── qa.yaml
│       ├── staging.yaml
│       └── prod.yaml
│
├── Dockerfile
│
├── docker-compose.yml
│
├── docs/
│
└── .github/

---

# HELM OWNERSHIP RULES

Application Helm Charts belong in:

go-crud-service

and

springboot-crud-service

Platform Helm Charts belong in:

cloud-platform-infra

Examples:

Platform Charts:

* ArgoCD
* Istio
* Kiali
* Prometheus
* Grafana

Application Charts:

* Go Service
* Spring Boot Service

Never mix platform charts and application charts.

---

# LOCAL DEVELOPMENT STRATEGY

Each application repository must support local development.

Create:

docker-compose.yml

for each application.

Docker Compose should start:

* Application
* PostgreSQL

Do not run platform components locally.

Avoid:

* ArgoCD
* Istio
* Kiali
* Prometheus
* Grafana

inside Docker Compose.

---

# APPLICATION CONFIGURATION STRATEGY

Applications must support:

* Local Development
* Kubernetes Deployment

Configuration should be environment-driven.

Use:

application-dev
application-qa
application-staging
application-prod

patterns where applicable.

Avoid hardcoded values.

---

# IMAGE STRATEGY

Each application produces its own image.

Examples:

go-crud-service

springboot-crud-service

Images should be pushed to GitHub Container Registry (GHCR).

Image tags should support:

* Commit SHA
* Semantic Version
* Latest

Document tagging strategy.

---

# REPOSITORY BOOTSTRAP REQUIREMENTS

Assume repositories already exist.

Claude may:

* Initialize Git
* Configure Remotes
* Create Commits
* Push Changes

Repository URLs are authoritative.

Generated artifacts should be committed to the appropriate repository.

Do not leave generated code only in local folders.

# TERRAFORM STRATEGY

Terraform is the primary infrastructure provisioning tool.

Infrastructure must be:

* Declarative
* Modular
* Reproducible
* Destroyable
* Cloud Ready

The entire platform should be provisioned using Infrastructure as Code.

Manual Azure Portal operations should be avoided.

---

# TERRAFORM DESIGN PRINCIPLES

Follow these principles:

1. Reusable Modules
2. Environment Separation
3. Remote State
4. Minimal Hardcoding
5. Idempotent Deployments
6. Complete Destroy Capability

Terraform code should be structured for future AWS and GCP expansion.

---

# TERRAFORM DIRECTORY STRUCTURE

terraform/
│
├── azure/
│   │
│   ├── environments/
│   │   ├── dev/
│   │   ├── qa/
│   │   ├── staging/
│   │   └── prod/
│   │
│   └── modules/
│       │
│       ├── networking/
│       ├── aks/
│       ├── postgres/
│       ├── keyvault/
│       ├── monitoring/
│       ├── identities/
│       ├── tags/
│       └── naming/
│
├── aws/
│
└── gcp/

---

# TERRAFORM REMOTE STATE

Use Remote State.

Do NOT use local state.

Do NOT commit:

terraform.tfstate

terraform.tfstate.backup

.terraform/

to Git.

---

# TERRAFORM BACKEND

Use Azure Storage Backend.

The backend must be created through a bootstrap process.

The backend infrastructure is independent from the platform environment.

Purpose:

* State Storage
* State Recovery
* Team Collaboration
* State Locking

---

# BOOTSTRAP STRATEGY

Generate:

bootstrap/
│
├── bootstrap-backend.sh
│
├── bootstrap-backend.ps1
│
└── README.md

Purpose:

Create:

* Resource Group
* Storage Account
* Blob Container

required for Terraform backend initialization.

---

# PLATFORM RECOVERY REQUIREMENT

The platform must be reproducible.

Assume:

Azure Subscription Deleted

or

Azure Account Disabled

The following assets remain:

* Git Repositories
* Bitwarden Secrets
* GitHub Account (GHCR images)

The platform should be fully recoverable by:

1. Cloning repositories
2. Running bootstrap script
3. Running terraform apply

No manual Azure configuration should be required.

---

# ENVIRONMENT STATE FILES

Prepare state naming for future environments.

Examples:

dev.tfstate

qa.tfstate

staging.tfstate

prod.tfstate

Only:

dev

will be used initially.

---

# AZURE RESOURCE GROUP STRATEGY

Create dedicated Resource Groups.

Examples:

Backend:

rg-vvd-tfstate

Platform:

rg-vvd-dev-platform

Future:

rg-vvd-qa-platform

rg-vvd-staging-platform

rg-vvd-prod-platform

---

# NETWORKING

Terraform should provision:

* Virtual Network
* Subnets
* NSGs if required

Design networking to support:

* AKS
* PostgreSQL
* Future Services

Use clean CIDR planning.

Document all network decisions.

---

# NETWORK DESIGN

Recommended Pattern:

VNet
│
├── AKS Subnet
│
├── Database Subnet
│
└── Future Expansion Subnet

Network design should support future growth.

---

# AKS REQUIREMENTS

Provision AKS through Terraform.

Requirements:

* Cost Optimized
* Single Node Pool
* Minimal Node Count
* Suitable for POC

Avoid unnecessary complexity.

---

# AKS OBJECTIVES

AKS should host:

* ArgoCD
* Istio
* Kiali
* Prometheus
* Grafana
* Go Service
* Spring Boot Service

Everything should be manageable through GitOps.

---

# AKS SECURITY

Implement:

* Managed Identity
* RBAC
* Least Privilege

Avoid cluster-admin access wherever possible.

Document all access decisions.

---

# POSTGRESQL REQUIREMENTS

Provision:

Azure PostgreSQL Flexible Server

through Terraform.

---

# POSTGRESQL CONFIGURATION

Use:

Burstable Tier

Lowest suitable SKU.

Avoid:

* High Availability
* Geo Redundancy
* Production Backup Policies

This is a learning environment.

---

# DATABASE STRATEGY

Create:

go_service_db

springboot_service_db

Keep application databases isolated.

---

# DATABASE CREDENTIALS

Database credentials must NOT exist in:

* Source Code
* Helm Values
* GitHub Repository

Store credentials in:

Azure Key Vault

Applications should retrieve secrets securely.

---

# SCHEMA MANAGEMENT

Terraform manages:

* Server
* Database
* Networking
* Configuration

Applications manage:

* Tables
* Indexes
* Migrations

Use:

Flyway

for schema evolution.

Avoid schema creation inside Terraform.

---

# KEY VAULT

Provision Key Vault through Terraform.

Store:

* Database Credentials
* Application Secrets
* Runtime Secrets

Key Vault should integrate with AKS workloads.

---

# DESTROY REQUIREMENTS

Terraform destroy must remove:

* Resource Group
* AKS
* PostgreSQL
* Key Vault
* Networking
* Managed Identities
* Monitoring Resources

No orphaned billable resources should remain.

---

# TERRAFORM COMMAND SUPPORT

Support:

terraform fmt

terraform validate

terraform plan

terraform apply

terraform plan -destroy

terraform destroy

All commands should be documented.

---

# TERRAFORM DOCUMENTATION

Generate documentation for:

* Module Structure
* Environment Structure
* Backend Configuration
* Bootstrap Process
* Networking Design
* AKS Design
* PostgreSQL Design
* Key Vault Design
* Destroy Process

Documentation should explain:

Why decisions were made.

Not just how they work.

# SECURITY STRATEGY

Security is a first-class requirement.

The platform should demonstrate enterprise-grade security practices.

Security decisions should favor:

* Least Privilege
* Zero Hardcoded Secrets
* Identity-Based Access
* Auditability
* Separation of Duties

---

# AUTHENTICATION STRATEGY

Current Authentication Provider:

Bitwarden Secrets Manager

Future Authentication Provider:

HashiCorp Vault

The platform should be designed so that Bitwarden can later be replaced with HashiCorp Vault with minimal architectural changes.

Avoid vendor lock-in.

---

# BITWARDEN CONFIGURATION

Organization Secret:

BW_ACCESS_TOKEN

Bitwarden Project:

DevOps-Lab

Available Secrets:

AZURE_CLIENT_ID

AZURE_CLIENT_SECRET

AZURE_SUBSCRIPTION_ID

AZURE_TENANT_ID

---

# BITWARDEN REQUIREMENTS

Bitwarden is the source of truth for CI/CD credentials.

GitHub Actions should:

1. Authenticate using BW_ACCESS_TOKEN
2. Retrieve required secrets
3. Use secrets at runtime
4. Never persist secrets

Avoid duplication into repository secrets.

Prefer organization-level secrets.

---

# SECRET MANAGEMENT PRINCIPLES

Never:

* Hardcode secrets
* Commit secrets to Git
* Print secrets to logs
* Store secrets in Helm values
* Store secrets in Terraform outputs

Always:

* Mask secrets
* Rotate secrets when required
* Store secrets centrally

---

# AZURE AUTHENTICATION

Current Approach:

Bitwarden
→ GitHub Actions
→ Azure

Future Compatible Approach:

GitHub OIDC
→ Vault
→ Azure

Design workflows so migration is simple.

---

# GITHUB OIDC READINESS

Do not fully implement GitHub OIDC today unless necessary.

However:

Design authentication layers so GitHub OIDC can be adopted later.

Document:

* OIDC Architecture
* Migration Strategy
* Vault Integration Strategy

---

# AZURE SERVICE PRINCIPAL

Use:

AZURE_CLIENT_ID

AZURE_CLIENT_SECRET

AZURE_SUBSCRIPTION_ID

AZURE_TENANT_ID

for the initial implementation.

Document:

* Purpose
* Permissions
* Scope

---

# MANAGED IDENTITY STRATEGY

Use Managed Identity wherever practical.

Avoid credentials where Azure-native identity is available.

Examples:

AKS
→ Managed Identity

Applications
→ Managed Identity

Key Vault Access
→ Managed Identity

Preferred order:

Managed Identity

then

Service Principal

then

Secrets

---

# RBAC STRATEGY

Implement Role-Based Access Control.

Use least privilege.

Avoid:

Owner

Contributor

unless absolutely required.

Create dedicated role assignments.

Document:

* Roles
* Permissions
* Justification

---

# AKS SECURITY

Implement:

* RBAC
* Namespace Isolation
* Least Privilege
* Managed Identity

Document security decisions.

---

# KEY VAULT STRATEGY

Azure Key Vault is the source of truth for runtime secrets.

Bitwarden is the source of truth for CI/CD secrets.

---

# KEY VAULT RESPONSIBILITIES

Store:

* Database Credentials
* Runtime Secrets
* Application Secrets
* Future Service Credentials

Do not store CI/CD credentials in Key Vault.

---

# SECRET FLOW

CI/CD Secrets:

Bitwarden
→ GitHub Actions

Runtime Secrets:

Key Vault
→ AKS Workloads

Maintain clear separation.

---

# GHCR (CONTAINER REGISTRY)

Registry:

ghcr.io

Authentication:

GITHUB_TOKEN (automatically injected by GitHub Actions — no separate credentials required)

Add packages: write permission to the build job.

---

# GHCR RESPONSIBILITIES

Store:

* Go Images
* Spring Boot Images

Support:

* Versioned Tags
* Commit SHA Tags
* Latest Tags

---

# IMAGE TAGGING STRATEGY

Support:

latest

semantic version

commit sha

Examples:

go-service:latest

go-service:v1.0.0

go-service:<commit-sha>

springboot-service:latest

springboot-service:v1.0.0

springboot-service:<commit-sha>

---

# CONTAINER SECURITY

Implement:

* Non-root Containers
* Minimal Base Images
* Vulnerability Scanning
* Security Checks During CI

Document all security decisions.

---

# SECURITY SCANNING

Integrate security scanning into CI.

Support:

* Dependency Scanning
* Container Scanning
* Static Analysis

Security failures should be visible.

---

# AUDITABILITY

All infrastructure changes must be traceable.

All application deployments must be traceable.

Changes should be attributable to:

* Commit
* Pull Request
* Workflow Run

---

# COMPLIANCE MINDSET

Although this is a POC:

Design with:

* Auditability
* Traceability
* Security

in mind.

Demonstrate practices expected in enterprise environments.

---

# SECURITY DOCUMENTATION

Generate dedicated documentation for:

Security Architecture

Bitwarden Integration

Key Vault Integration

Managed Identity

RBAC

GHCR Authentication

Container Security

OIDC Future Strategy

Vault Migration Strategy

Security Best Practices

Troubleshooting Security Issues

Explain:

Why each security decision was made.

Not just how it was implemented.

# CI/CD STRATEGY

The platform should demonstrate a modern enterprise CI/CD implementation.

Objectives:

* Fast Feedback
* Repeatable Deployments
* Secure Pipelines
* GitOps Integration
* Traceability

GitHub Actions is the standard CI/CD platform.

---

# CI/CD PHILOSOPHY

Code should move through:

Developer
→ Pull Request
→ CI Validation
→ Merge To Main
→ Build
→ Package
→ Publish
→ GitOps Deployment

Avoid manual deployment steps.

---

# GITHUB ACTIONS STRUCTURE

Each repository should contain:

.github/
└── workflows/

Workflow naming should be consistent.

Use descriptive names.

---

# PULL REQUEST PIPELINE

Trigger:

Pull Requests

Purpose:

Validate changes before merge.

Pipeline:

1. Checkout Code
2. Restore Dependencies
3. Compile
4. Unit Tests
5. Integration Tests
6. Linting
7. Security Scan
8. Generate Reports

No deployment should occur.

---

# GO SERVICE PIPELINE

Validate:

* Build
* Unit Tests
* Integration Tests
* Linting
* Security Checks

Generate artifacts.

Publish reports.

---

# SPRING BOOT PIPELINE

Validate:

* Maven Build
* Unit Tests
* Integration Tests
* Static Analysis
* Security Checks

Generate artifacts.

Publish reports.

---

# MERGE TO MAIN PIPELINE

Trigger:

main branch

Purpose:

Create deployable artifacts.

Pipeline:

1. Build
2. Test
3. Package
4. Docker Build
5. Docker Scan
6. Push To GHCR

---

# GHCR PUBLISHING

Authentication uses GITHUB_TOKEN.

No separate credentials required.

GITHUB_TOKEN is automatically injected by GitHub Actions.

Add packages: write permission to the build job.

Never hardcode credentials.

---

# IMAGE TAGGING STRATEGY

Every build should create:

latest

semantic version

commit sha

Examples:

go-crud-service:latest

go-crud-service:v1.0.0

go-crud-service:a1b2c3d

springboot-crud-service:latest

springboot-crud-service:v1.0.0

springboot-crud-service:a1b2c3d

---

# DEPLOYMENT STRATEGY

CI does NOT directly deploy to Kubernetes.

CI responsibilities:

Build
Test
Publish

CD responsibilities:

GitOps
ArgoCD

Maintain separation.

---

# INFRASTRUCTURE PIPELINES

cloud-platform-infra should contain:

terraform-plan-dev.yml

terraform-apply-dev.yml

terraform-destroy-dev.yml

verify-cleanup.yml

Future:

terraform-apply-qa.yml

terraform-apply-staging.yml

terraform-apply-prod.yml

---

# TERRAFORM VALIDATION PIPELINE

Pipeline:

terraform fmt

terraform validate

terraform plan

Generate plan artifacts.

Show expected changes.

---

# TERRAFORM APPLY PIPELINE

Pipeline:

Authenticate

Initialize Backend

Terraform Init

Terraform Plan

Terraform Apply

Verification

---

# DESTROY PIPELINE

Pipeline:

Authenticate

Terraform Init

Terraform Plan -Destroy

Terraform Destroy

Verify Cleanup

Generate Summary

---

# APPLICATION DEPLOYMENT PIPELINES

Create:

deploy-dev.yml

deploy-qa.yml

deploy-staging.yml

deploy-prod.yml

Only:

deploy-dev.yml

should be active initially.

---

# ENVIRONMENT PROMOTION STRATEGY

Support:

dev
→ qa
→ staging
→ prod

Promotion should be documented.

Future environments should already exist in workflow definitions.

---

# DEPLOYMENT GATES

Support approval checkpoints.

Examples:

dev
→ automatic

qa
→ approval

staging
→ approval

prod
→ approval

Only dev should deploy automatically.

---

# TESTING STRATEGY

Support:

Unit Tests

Integration Tests

API Tests

Container Validation

Deployment Validation

Testing should be automated.

---

# TEST REPORTING

Generate:

Test Results

Coverage Reports

Build Reports

Security Reports

Pipeline output should be easy to understand.

---

# FAILURE HANDLING

Pipeline failures should:

Fail Fast

Provide Clear Messages

Generate Actionable Logs

Avoid cryptic errors.

---

# PIPELINE REUSABILITY

Avoid duplicated workflow logic.

Use:

Reusable Workflows

Composite Actions

Shared Templates

where appropriate.

---

# VERSIONING STRATEGY

Support:

Semantic Versioning

Examples:

v1.0.0

v1.1.0

v2.0.0

Document release strategy.

---

# RELEASE STRATEGY

Source Of Truth:

main

Every production artifact must be traceable to:

* Commit
* Pull Request
* Workflow Run

---

# PIPELINE SECURITY

Implement:

Least Privilege

Secret Masking

Credential Isolation

Secure Authentication

Auditability

---

# PIPELINE DOCUMENTATION

Generate documentation for:

CI Architecture

CD Architecture

Build Flow

Testing Strategy

Release Strategy

Promotion Strategy

GitHub Actions Design

Workflow Breakdown

Troubleshooting

Best Practices

Explain:

Why workflows are structured this way.

Not just how they work.

# GITOPS STRATEGY

GitOps is the primary deployment model.

ArgoCD is the GitOps engine.

Manual kubectl deployments should be avoided.

Git should remain the source of truth.

---

# GITOPS PRINCIPLES

Infrastructure:

Terraform

Platform Components:

Helm

Applications:

Helm + ArgoCD

All deployments must be traceable through Git.

---

# DEPLOYMENT FLOW

Developer
→ Feature Branch
→ Pull Request
→ CI Validation
→ Merge To Main
→ Docker Build
→ GHCR
→ Git Commit
→ ArgoCD Sync
→ AKS Deployment

No manual deployment should be required.

---

# ARGOCD OWNERSHIP

Repository:

cloud-platform-infra

Owns:

* ArgoCD Installation
* ArgoCD Configuration
* ArgoCD Projects
* ArgoCD Applications

ArgoCD should not live inside application repositories.

---

# ARGOCD STRUCTURE

argocd/
│
├── dev/
│
├── qa/
│
├── staging/
│
└── prod/

Only:

dev

will be active initially.

---

# ARGOCD PROJECTS

Create logical separation.

Example:

Platform Project

Applications Project

Future expansion should be simple.

---

# APPLICATION DEPLOYMENT MODEL

Applications deploy from their own repositories.

Go Application:

go-crud-service

Spring Application:

springboot-crud-service

ArgoCD should monitor those repositories.

---

# APPLICATION HELM STRUCTURE

deployment/
│
├── helm/
│
└── values/
├── dev.yaml
├── qa.yaml
├── staging.yaml
└── prod.yaml

This structure must exist in both application repositories.

---

# HELM PRINCIPLES

Helm charts should be:

Reusable

Environment Aware

Cloud Agnostic

Version Controlled

Avoid hardcoded values.

---

# ENVIRONMENT CONFIGURATION

Use environment-specific values.

Examples:

dev.yaml

qa.yaml

staging.yaml

prod.yaml

Keep configuration differences isolated.

---

# KUBERNETES RESOURCE REQUIREMENTS

Applications should deploy:

Deployment

Service

ConfigMap

Secret Integration

Ingress

Horizontal Scaling Readiness

Health Checks

Resource Limits

Resource Requests

---

# HEALTH CHECKS

Implement:

Readiness Probes

Liveness Probes

Startup Probes where appropriate

Applications must expose health endpoints.

---

# RESOURCE MANAGEMENT

Define:

CPU Requests

CPU Limits

Memory Requests

Memory Limits

Avoid unbounded workloads.

---

# NAMESPACE STRATEGY

Create dedicated namespaces.

Examples:

platform

monitoring

istio-system

argocd

applications-dev

Future:

applications-qa

applications-staging

applications-prod

---

# PLATFORM NAMESPACES

Platform workloads should remain isolated.

Examples:

ArgoCD

Prometheus

Grafana

Istio

Kiali

must not run inside application namespaces.

---

# APPLICATION NAMESPACES

Applications should run separately.

Benefits:

Isolation

Security

Operational Clarity

Future Scalability

---

# INGRESS STRATEGY

Use Istio Ingress Gateway.

Avoid multiple ingress controllers.

Centralize traffic management.

---

# SERVICE DISCOVERY

Use Kubernetes-native service discovery.

Applications should communicate through service names.

Avoid IP-based configuration.

---

# CONFIGURATION MANAGEMENT

Application configuration should be externalized.

Use:

ConfigMaps

Secrets

Helm Values

Avoid environment-specific code changes.

---

# SECRET INJECTION

Runtime secrets should originate from:

Azure Key Vault

Applications should not contain credentials.

Avoid secret duplication.

---

# DEPLOYMENT VALIDATION

After deployment verify:

Pods Running

Services Available

Ingress Reachable

Health Endpoints Healthy

Metrics Available

Logs Available

---

# ROLLBACK STRATEGY

Support rollback.

ArgoCD should be able to restore previous versions.

Document rollback procedures.

---

# ARGOCD SYNC STRATEGY

Use automated sync for:

dev

Prepare manual approval capability for:

qa

staging

prod

Future environments should support controlled promotion.

---

# DRIFT DETECTION

ArgoCD should detect drift.

Manual changes inside Kubernetes should be identified.

Git remains authoritative.

---

# MULTI CLOUD READINESS

Application Helm Charts must remain portable.

Do NOT hardcode:

Azure-specific annotations

Azure-specific storage classes

Azure-specific load balancers

Cloud-specific settings should be isolated.

---

# HELM VERSIONING

Version Helm charts.

Track chart changes independently from application code.

Document chart versioning strategy.

---

# KUBERNETES DOCUMENTATION

Generate documentation for:

Namespace Strategy

Helm Design

ArgoCD Design

GitOps Flow

Environment Strategy

Deployment Process

Rollback Process

Configuration Management

Ingress Design

Service Discovery

Troubleshooting

Explain:

Why resources are organized this way.

Not just how they are deployed.

---

# SUCCESS CRITERIA

A successful deployment should demonstrate:

GitHub Actions
→ GHCR
→ Git Commit
→ ArgoCD
→ AKS
→ Running Application

with no manual deployment activities required.

# OBSERVABILITY STRATEGY

Observability is a first-class platform capability.

The platform should demonstrate:

* Monitoring
* Logging
* Tracing
* Service Topology
* Service Health
* Cluster Health

The goal is to provide visibility across infrastructure and applications.

---

# OBSERVABILITY COMPONENTS

Implement:

Prometheus

Grafana

OpenTelemetry

Azure Monitor

Log Analytics

Application Insights

Kiali

Istio Telemetry

---

# OBSERVABILITY ARCHITECTURE

Applications
│
├── Logs
├── Metrics
└── Traces
│
▼

OpenTelemetry

▼

Prometheus
Grafana
Azure Monitor
Application Insights

---

# MONITORING STRATEGY

Support monitoring at multiple layers.

Infrastructure Monitoring

Platform Monitoring

Application Monitoring

Service Mesh Monitoring

Business Endpoint Monitoring

---

# PROMETHEUS

Prometheus is the primary metrics collection platform.

Install through:

Terraform
+
Helm


Terraform provisions infrastructure.

Helm manages Kubernetes platform components.

Terraform may invoke Helm providers where appropriate.

Prometheus belongs to:

cloud-platform-infra

---

# PROMETHEUS RESPONSIBILITIES

Collect:

AKS Metrics

Node Metrics

Pod Metrics

Container Metrics

Istio Metrics

Application Metrics

Kubernetes Metrics

---

# PROMETHEUS RETENTION

Use cost-optimized retention.

The platform is a short-lived POC.

Avoid excessive storage retention.

Document retention settings.

---

# GRAFANA

Grafana is the primary visualization platform.

Install through:

Terraform
+
Helm

Terraform provisions infrastructure.

Helm manages Kubernetes platform components.

Terraform may invoke Helm providers where appropriate.

Grafana belongs to:

cloud-platform-infra

---

# GRAFANA DASHBOARDS

Generate dashboards for:

Cluster Health

Node Health

Pod Health

Namespace Health

CPU Usage

Memory Usage

Disk Usage

Network Usage

---

# APPLICATION DASHBOARDS

Generate dashboards for:

Request Count

Request Rate

Error Rate

Response Time

Latency

Application Availability

Database Activity

---

# GRAFANA ORGANIZATION

Dashboards should be organized logically.

Examples:

Infrastructure

Platform

Applications

Service Mesh

Future environments should be easy to add.

---

# OPENTELEMETRY

Use OpenTelemetry as the standard telemetry framework.

Implement:

Metrics

Logs

Traces

in both applications.

---

# APPLICATION INSTRUMENTATION

Go Service:

OpenTelemetry

Spring Boot Service:

OpenTelemetry

Instrumentation should be production-inspired.

---

# DISTRIBUTED TRACING

Support distributed tracing.

Track:

Incoming Requests

Database Calls

Service Calls

External Calls

Application Flow

---

# APPLICATION INSIGHTS

Provision through Terraform.

Application Insights should integrate with:

OpenTelemetry

Azure Monitor

Applications

---

# APPLICATION INSIGHTS RESPONSIBILITIES

Collect:

Application Telemetry

Performance Data

Exceptions

Dependency Calls

Availability Metrics

---

# AZURE MONITOR

Provision through Terraform.

Azure Monitor provides:

Platform Monitoring

Cloud Monitoring

Azure Resource Monitoring

---

# AZURE MONITOR RESPONSIBILITIES

Monitor:

AKS

PostgreSQL

Key Vault

Networking

Azure Resources

Infrastructure Health

---

# LOG ANALYTICS

Provision through Terraform.

Log Analytics acts as the central logging repository.

---

# LOG COLLECTION

Collect:

Container Logs

AKS Logs

Application Logs

Platform Logs

Istio Logs

System Logs

---

# LOGGING STRATEGY

Applications should generate:

Structured Logs

Prefer:

JSON Logging

Avoid unstructured logs where possible.

---

# APPLICATION LOGGING

Logs should include:

Timestamp

Log Level

Service Name

Request Identifier

Trace Identifier

Message

---

# CORRELATION

Support log correlation.

A request should be traceable through:

Application Logs

Metrics

Traces

Service Mesh

Monitoring Systems

---

# ISTIO TELEMETRY

Istio should provide:

Traffic Metrics

Request Metrics

Error Metrics

Latency Metrics

Service Communication Metrics

---

# KIALI

Install through:

Terraform
+
Helm


Terraform provisions infrastructure.

Helm manages Kubernetes platform components.

Terraform may invoke Helm providers where appropriate.

Kiali belongs to:

cloud-platform-infra

---

# KIALI RESPONSIBILITIES

Visualize:

Service Topology

Traffic Flow

Request Volume

Latency

Error Rates

Health Status

---

# SERVICE TOPOLOGY

The platform should visually demonstrate:

Client
→ Go Service
→ PostgreSQL

Client
→ Spring Boot Service
→ PostgreSQL

through Kiali.

---

# ALERTING STRATEGY

Prepare alerting architecture.

Document:

Alert Categories

Alert Severity

Alert Routing

Alert Ownership

Actual alert implementation may be minimal for the POC.

---

# HEALTH MONITORING

Monitor:

Application Health

Pod Health

Cluster Health

Database Health

Ingress Health

Platform Component Health

---

# SLI / SLO READINESS

Document:

Availability

Latency

Error Rate

Throughput

Prepare future SLI/SLO implementation guidance.

---

# TROUBLESHOOTING SUPPORT

The observability stack should support troubleshooting.

Examples:

Application Failure

Database Connectivity Issues

High Latency

Resource Exhaustion

Deployment Failures

Traffic Routing Issues

---

# COST OPTIMIZATION

The observability stack should remain cost conscious.

Use:

Minimal Retention

Minimal Resource Requests

POC-Sized Deployments

Avoid unnecessary resource consumption.

---

# OBSERVABILITY DOCUMENTATION

Generate documentation for:

Observability Architecture

Prometheus

Grafana

OpenTelemetry

Azure Monitor

Application Insights

Log Analytics

Kiali

Istio Telemetry

Dashboard Guide

Troubleshooting Guide

Cost Optimization

Explain:

Why each observability component exists.

How they complement each other.

When each tool should be used.

---

# SUCCESS CRITERIA

The platform should demonstrate:

Metrics
+
Logs
+
Traces
+
Topology

working together.

A user should be able to:

Observe

Monitor

Troubleshoot

and

Understand

the complete platform from a single observability ecosystem.

# PLATFORM LIFECYCLE STRATEGY

The platform must support:

Provision
→ Operate
→ Destroy
→ Recreate

with minimal manual effort.

Infrastructure should be considered disposable.

Git repositories are the source of truth.

---

# SOURCE OF TRUTH

Permanent Assets:

Git Repositories

Bitwarden Secrets

GitHub Account (GHCR images)

Documentation

Architecture Decisions

---

# DISPOSABLE ASSETS

Azure Resources

AKS

PostgreSQL

Key Vault

Networking

Prometheus

Grafana

ArgoCD

Istio

Kiali

Application Insights

Log Analytics

Terraform Backend Resources

All Azure resources should be reproducible.

---

# DESTRUCTION PRINCIPLES

The platform must be designed for complete teardown.

Requirements:

No Manual Cleanup

No Orphaned Resources

No Hidden Costs

No Production Protection Mechanisms

No Resource Locks

No Delete Locks

No Purge Protection

No Shared Azure Resources

---

# DESTROYABLE DESIGN

Every Azure resource must be:

Created By Terraform

Managed By Terraform

Destroyed By Terraform

Avoid resources that cannot be fully managed.

---

# RESOURCE GROUP STRATEGY

All platform resources must reside within:

Dedicated Resource Group

Example:

rg-vvd-dev-platform

Destroying the environment should remove the entire platform.

---

# DESTROY WORKFLOW

Generate:

destroy-environment.yml

Responsibilities:

Authenticate

Initialize Terraform

Run:

terraform plan -destroy

Run:

terraform destroy

Verify deletion

Generate cleanup report

---

# CLEANUP VERIFICATION

Generate:

verify-cleanup.yml

Verify:

Resource Group Exists?

AKS Exists?

PostgreSQL Exists?

Key Vault Exists?

Application Insights Exists?

Log Analytics Exists?

Managed Identity Exists?

Networking Exists?

Expected Result:

Everything Deleted

---

# AZURE VALIDATION

Use Azure CLI validation.

Examples:

az group exists

az resource list

Verification should be automated.

---

# CLEANUP REPORTING

Generate human-readable cleanup summaries.

Report:

Resources Destroyed

Resources Remaining

Potential Billing Risks

Warnings

---

# BILLING SAFETY

The destroy workflow should identify:

Unexpected Resources

Resources Not Managed By Terraform

Potential Cost Risks

before concluding.

---

# PLATFORM RECOVERY

Assume:

Azure Subscription Deleted

Azure Account Disabled

New Azure Subscription Created

The platform should be recoverable.

---

# RECOVERY PROCESS

Expected Process:

Clone Repositories

Run Bootstrap

Initialize Terraform

Apply Terraform

Deploy Applications

Platform Restored

---

# BOOTSTRAP RECOVERY

Bootstrap scripts should recreate:

Backend Resource Group

Storage Account

Blob Container

Terraform Backend Configuration

without manual portal operations.

---

# REPRODUCIBILITY

The entire platform should be reproducible from:

Git

Bitwarden

GitHub (GHCR)

Azure Account

Nothing else should be required.

---

# DOCUMENTATION REQUIREMENT

Document:

Full Recovery Process

Step By Step

Expected Duration

Prerequisites

Troubleshooting

---

# DISASTER RECOVERY PHILOSOPHY

This is a POC.

Disaster Recovery focuses on:

Rebuilding

not

Failover

Avoid expensive DR features.

Examples to Avoid:

Geo Redundancy

Cross Region Replication

High Availability

Multi Region Deployments

unless explicitly required.

---

# BACKUP STRATEGY

Document backup concepts.

For the POC:

Keep backup configuration minimal.

Avoid expensive enterprise backup features.

The primary recovery strategy is:

Infrastructure As Code

GitOps

Reproducibility

---

# COST GOVERNANCE

Cost optimization is mandatory.

Every infrastructure decision should consider cost.

Prefer:

Small SKUs

Minimal Retention

Single Node Pool

Single Environment

POC Sized Resources

---

# COST ESTIMATION

Before provisioning:

Estimate:

Hourly Cost

3-Hour Cost

Worst Case Cost

Document assumptions.

---

# COST TARGETS

Target:

₹200

Maximum:

₹350

for a typical 3-hour demonstration session.

---

# MAJOR COST CONTRIBUTORS

Document expected contributors:

AKS

PostgreSQL

Application Insights

Log Analytics

Monitoring Stack

Provide optimization recommendations.

---

# COST OPTIMIZATION REQUIREMENTS

Use:

Minimal AKS Node Count

Minimal PostgreSQL SKU

Minimal Monitoring Retention

Minimal Resource Requests

Minimal Storage Usage

Avoid overprovisioning.

---

# COST DOCUMENTATION

Generate:

Cost Estimation

Cost Breakdown

Cost Optimization Guide

Resource Sizing Guide

Cleanup Guide

Azure Billing Notes

---

# END OF SESSION PROCEDURE

Document recommended shutdown process.

Example:

Verify Applications

Destroy Environment

Verify Cleanup

Review Billing

Confirm Resource Deletion

This should become the standard operating procedure.

---

# SUCCESS CRITERIA

A successful platform must support:

Provisioning

Deployment

Monitoring

Troubleshooting

Destruction

Recreation

without requiring undocumented manual steps.

The platform should be capable of being recreated months later from Git repositories and documented procedures alone.

# DOCUMENTATION PHILOSOPHY

Documentation is a first-class deliverable.

Documentation should be treated with the same importance as source code.

The platform should be understandable by:

* Developers
* DevOps Engineers
* SREs
* Platform Engineers
* Architects
* Interviewers
* Future Maintainers

---

# DOCUMENTATION PRINCIPLES

Documentation should answer:

What

Why

How

When

Where

Do not only explain implementation.

Explain reasoning.

---

# README STRATEGY

README files should act as:

Navigation Hubs

not

Knowledge Dumps

Detailed information belongs in dedicated documentation pages.

---

# INFRASTRUCTURE REPOSITORY README

Repository:

cloud-platform-infra

README should include:

Project Overview

Architecture Summary

Technology Stack

Repository Structure

Quick Start

Deployment Flow

Documentation Index

Common Commands

Cleanup Instructions

Links To Detailed Documentation

---

# APPLICATION REPOSITORY README

Repositories:

go-crud-service

springboot-crud-service

README should include:

Overview

Architecture

Local Development

Docker Usage

API Summary

Deployment Summary

Documentation Index

Common Commands

Troubleshooting

---

# DOCUMENTATION STRUCTURE

cloud-platform-infra
│
├── README.md
│
└── docs/

Documentation must be organized.

Avoid large monolithic documents.

---

# INFRASTRUCTURE DOCUMENTATION

Generate:

docs/
│
├── architecture/
│
├── terraform/
│
├── gitops/
│
├── security/
│
├── observability/
│
├── operations/
│
├── environments/
│
├── cost/
│
└── adr/

---

# ARCHITECTURE DOCUMENTATION

Generate:

system-architecture.md

infrastructure-architecture.md

gitops-architecture.md

observability-architecture.md

service-mesh-architecture.md

security-architecture.md

---

# ARCHITECTURE CONTENT

Architecture documentation should explain:

Business Goal

Technical Goal

Design Decisions

Tradeoffs

Future Expansion Strategy

Multi-Cloud Strategy

---

# TERRAFORM DOCUMENTATION

Generate:

terraform-structure.md

module-design.md

environment-design.md

backend-design.md

bootstrap-process.md

destroy-process.md

Explain:

Why modules exist

Why environments are structured this way

Why remote state is required

---

# ENVIRONMENT DOCUMENTATION

Generate:

dev.md

qa.md

staging.md

prod.md

Explain:

Purpose

Promotion Flow

Configuration Differences

Deployment Strategy

Current Status

Only dev should be active initially.

---

# GITOPS DOCUMENTATION

Generate:

argocd-design.md

helm-strategy.md

deployment-flow.md

promotion-flow.md

rollback-strategy.md

Explain:

GitOps Principles

ArgoCD Responsibilities

Deployment Lifecycle

Rollback Lifecycle

---

# SECURITY DOCUMENTATION

Generate:

bitwarden.md

key-vault.md

rbac.md

managed-identity.md

oidc-future-strategy.md

vault-migration-strategy.md

Explain:

Why security decisions were made.

---

# OBSERVABILITY DOCUMENTATION

Generate:

prometheus.md

grafana.md

opentelemetry.md

azure-monitor.md

application-insights.md

kiali.md

Explain:

What each tool does

When it should be used

How it integrates with the platform

---

# OPERATIONS DOCUMENTATION

Generate:

deployment-guide.md

bootstrap-guide.md

cleanup-guide.md

recovery-guide.md

rollback-guide.md

troubleshooting-guide.md

Document operational procedures.

---

# COST DOCUMENTATION

Generate:

cost-estimation.md

cost-breakdown.md

cost-optimization.md

Document:

Expected Costs

Optimization Decisions

Cost Drivers

Ways To Reduce Cost

---

# APPLICATION DOCUMENTATION

Both application repositories should contain:

docs/
│
├── architecture.md
├── api.md
├── local-development.md
├── docker.md
├── deployment.md
├── testing.md
├── observability.md
└── troubleshooting.md

---

# API DOCUMENTATION

Document:

Endpoints

Request Examples

Response Examples

Error Responses

Validation Rules

Authentication Strategy

---

# TESTING DOCUMENTATION

Document:

Unit Testing

Integration Testing

Test Execution

Coverage Expectations

Troubleshooting Failed Tests

---

# LOCAL DEVELOPMENT DOCUMENTATION

Document:

Prerequisites

Docker Compose

Running Services

Database Setup

Debugging

Common Issues

---

# TROUBLESHOOTING DOCUMENTATION

Document common problems.

Examples:

Terraform Failures

AKS Issues

ArgoCD Sync Problems

Image Pull Failures

Database Connectivity Issues

Monitoring Problems

Authentication Problems

---

# ARCHITECTURE DECISION RECORDS

Generate:

docs/adr/

---

# REQUIRED ADRS

ADR-001-use-terraform.md

ADR-002-use-argocd.md

ADR-003-use-istio.md

ADR-004-use-bitwarden.md

ADR-005-use-ghcr.md

ADR-006-use-aks.md

ADR-007-use-postgresql.md

ADR-008-use-opentelemetry.md

ADR-009-use-github-actions.md

ADR-010-multi-cloud-strategy.md

---

# ADR FORMAT

Each ADR should include:

Title

Status

Context

Decision

Alternatives Considered

Consequences

References

---

# DIAGRAM REQUIREMENTS

Generate architecture diagrams.

Examples:

System Diagram

Infrastructure Diagram

GitOps Diagram

CI/CD Diagram

Observability Diagram

Security Diagram

Environment Diagram

---

# DIAGRAM FORMATS

Prefer:

Mermaid

Markdown Embedded Diagrams

Text Diagrams

Avoid proprietary tooling.

---

# KNOWLEDGE TRANSFER

Documentation should allow a new engineer to:

Clone Repositories

Understand Architecture

Provision Infrastructure

Deploy Applications

Monitor Platform

Destroy Environment

Recreate Environment

without external guidance.

---

# INTERVIEW READINESS

Documentation should support interview discussions.

An engineer should be able to explain:

Why Terraform

Why ArgoCD

Why Istio

Why GitOps

Why Bitwarden

Why AKS

Why PostgreSQL

Why OpenTelemetry

using the documentation.

---

# DOCUMENTATION QUALITY REQUIREMENTS

Documentation should be:

Clear

Structured

Searchable

Maintainable

Professional

Enterprise Ready

Avoid placeholder documentation.

Generate meaningful content.

---

# SUCCESS CRITERIA

A new engineer should be able to:

Read Documentation

Understand Platform

Deploy Platform

Operate Platform

Troubleshoot Platform

Destroy Platform

Recreate Platform

without needing tribal knowledge.

# EXECUTION STRATEGY

This project is large and spans multiple domains:

* Cloud
* Infrastructure
* Kubernetes
* Security
* GitOps
* CI/CD
* Observability
* Applications

Implementation must be phased.

Avoid generating everything at once.

---

# WORKING PRINCIPLES

Work incrementally.

Prefer:

Small Reviewable Changes

over

Large Unreviewable Changes

After every major phase:

Explain decisions.

Show generated files.

Show repository structure.

Wait for approval.

---

# ARCHITECT REVIEW MODE

Act as an Architect first.

Act as an Implementer second.

Before generating code:

Review requirements.

Identify risks.

Identify assumptions.

Identify missing prerequisites.

Propose improvements.

---

# CHALLENGE REQUIREMENT

You are allowed to challenge requirements.

If a requirement conflicts with:

Security

Terraform Best Practices

Kubernetes Best Practices

Azure Best Practices

GitOps Principles

Enterprise Standards

then:

Explain the concern.

Propose a better alternative.

Wait for approval.

Do not blindly implement poor designs.

---

# IMPLEMENTATION PHASES

Implementation must follow the phases below.

Do not skip phases.

---

# PHASE 1

Architecture Review

Objectives:

Review Requirements

Identify Gaps

Identify Risks

Identify Assumptions

Validate Design

Deliverables:

Architecture Assessment

Dependency Assessment

Risk Assessment

Recommended Improvements

Approval Required

---

# PHASE 2

Prerequisites

Objectives:

Validate Azure Requirements

Validate Bitwarden Requirements

Validate GHCR Access

Validate GitHub Requirements

Validate Local Environment

Deliverables:

Prerequisite Checklist

Environment Checklist

Approval Required

---

# PHASE 3

Repository Structure

Objectives:

Generate Repository Layout

Generate Folder Structure

Generate Documentation Structure

Generate ADR Structure

Deliverables:

Repository Structure

Folder Layout

Documentation Layout

Approval Required

---

# PHASE 4

Terraform Foundation

Objectives:

Backend Design

Bootstrap Design

Terraform Structure

Environment Design

Module Design

Deliverables:

Terraform Layout

Bootstrap Scripts

Backend Configuration

Approval Required

---

# PHASE 5

Azure Infrastructure

Objectives:

Networking

AKS

PostgreSQL

Key Vault

Monitoring Resources

Deliverables:

Terraform Modules

Terraform Environment Configuration

Infrastructure Documentation

Approval Required

---

# PHASE 6

Platform Components

Objectives:

Install:

ArgoCD

Istio

Kiali

Prometheus

Grafana

through Terraform and Helm.

Deliverables:

Platform Helm Configuration

Terraform Integration

Documentation

Approval Required

---

# PHASE 7

Go Service

Objectives:

Generate Go Application

CRUD APIs

Database Integration

OpenTelemetry

Docker Support

Testing

Helm Chart

Deliverables:

Go Application

Docker Compose

Helm Chart

Documentation

Approval Required

---

# PHASE 8

Spring Boot Service

Objectives:

Extend Existing Spring Boot Project

CRUD APIs

Database Integration

OpenTelemetry

Docker Support

Testing

Helm Chart

Deliverables:

Spring Boot Application

Docker Compose

Helm Chart

Documentation

Approval Required

---

# PHASE 9

CI/CD

Objectives:

GitHub Actions

Build Pipelines

Testing Pipelines

GHCR Publishing

Terraform Pipelines

Destroy Pipelines

Deliverables:

Workflow Files

Pipeline Documentation

Approval Required

---

# PHASE 10

GitOps

Objectives:

ArgoCD Applications

Environment Structure

Promotion Flow

Deployment Strategy

Deliverables:

GitOps Configuration

Promotion Documentation

Approval Required

---

# PHASE 11

Observability

Objectives:

OpenTelemetry

Prometheus

Grafana

Azure Monitor

Application Insights

Kiali

Deliverables:

Dashboards

Configuration

Documentation

Approval Required

---

# PHASE 12

Security

Objectives:

Bitwarden

Managed Identity

RBAC

Key Vault

Security Hardening

Deliverables:

Security Configuration

Security Documentation

Approval Required

---

# PHASE 13

Destruction And Recovery

Objectives:

Destroy Workflow

Cleanup Verification

Bootstrap Recovery

Platform Recovery

Deliverables:

destroy-environment.yml

verify-cleanup.yml

Recovery Documentation

Approval Required

---

# PHASE 14

Documentation

Objectives:

Complete Documentation

Architecture Documentation

Operational Documentation

ADR Documentation

Deliverables:

Full Documentation Set

Approval Required

---

# PHASE 15

Final Validation

Objectives:

Review Entire Platform

Review Architecture

Review Security

Review Cost

Review GitOps

Review Observability

Review Documentation

Deliverables:

Final Readiness Report

Architecture Summary

Known Limitations

Future Improvements

Approval Required

---

# OUTPUT EXPECTATIONS

For every phase provide:

What Was Generated

Files Created

Directories Created

Key Decisions

Tradeoffs

Next Steps

---

# QUALITY REQUIREMENTS

Generated code should be:

Readable

Maintainable

Modular

Testable

Production Inspired

Cost Conscious

Well Documented

Avoid shortcuts that create technical debt.

---

# COST REQUIREMENTS

Always consider cost.

Prefer:

Small Resources

Minimal Retention

Minimal Node Counts

POC Appropriate Configurations

Document cost implications.

---

# DOCUMENTATION REQUIREMENTS

Documentation is mandatory.

Every major component should have documentation.

Every architectural decision should be explained.

Every operational procedure should be documented.

---

# GIT REQUIREMENTS

Repositories already exist.

Use:

https://github.com/Vivid-Vortex-DevOps/cloud-platform-infra.git

https://github.com/Vivid-Vortex-DevOps/go-crud-service.git

https://github.com/Vivid-Vortex-DevOps/springboot-crud-service.git

Claude may:

Initialize Git

Configure Remotes

Commit Changes

Push Changes

If user interaction is required, provide exact instructions.

---

# FINAL SUCCESS CRITERIA

The project is successful when:

Terraform provisions the platform.

GitHub Actions build and publish images.

GHCR stores images.

ArgoCD deploys applications.

AKS runs workloads.

Istio manages traffic.

Kiali visualizes topology.

Prometheus collects metrics.

Grafana visualizes metrics.

Azure Monitor collects platform telemetry.

Application Insights collects application telemetry.

Bitwarden manages CI/CD secrets.

Key Vault manages runtime secrets.

Applications expose health, metrics, and traces.

The environment can be destroyed safely.

The platform can be recreated later from Git repositories.

Documentation explains every major decision.

The project demonstrates enterprise-grade DevOps, Platform Engineering, GitOps, Observability, Security, and Infrastructure-as-Code practices.

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

Choose the solution that best demonstrates real-world enterprise engineering practices.

# IMPLEMENTATION COMPLETION REQUIREMENTS

The project is NOT considered ready for Azure deployment until all repositories have been fully generated, reviewed, committed, and pushed.

Follow this order strictly.

Phase A:
- Generate repository structures
- Generate source code
- Generate Terraform
- Generate Helm charts
- Generate GitHub Actions
- Generate ArgoCD configurations
- Generate documentation

Phase B:
- Review generated code
- Review architecture
- Review Terraform
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
- Validate Terraform validation

Phase E:
- Review Azure cost estimate
- Review deployment plan
- Obtain explicit user approval

Phase F:
- Deploy to Azure

Do NOT deploy to Azure before completing all previous phases.