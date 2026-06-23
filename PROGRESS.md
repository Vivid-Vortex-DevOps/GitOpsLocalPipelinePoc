# Local Platform Implementation Progress

Last updated: 2026-06-24

Reference design: [CLAUDE_LOCAL.md](CLAUDE_LOCAL.md)

---

## Phase Summary

| Phase | Name | Status | Notes |
|-------|------|--------|-------|
| 1 | Architecture Review | DONE | CLAUDE_LOCAL.md design doc complete |
| 2 | Prerequisites | DONE | helm, kind, argocd CLI installed in WSL2 |
| 3 | Repository Structure | DONE | local-platform-infra generated and pushed |
| 4 | Bootstrap Foundation | DONE | Kind cluster running, namespaces created |
| 5 | Platform Components | DONE | All 9 components deployed and verified |
| 6 | Go Service | DONE | Deployed to Kind, CRUD API verified, all tests pass |
| 7 | Spring Boot Service | DONE | Deployed to Kind, CRUD API verified, all tests pass |
| 8 | CI/CD | DONE | Workflows created, runner binary installed, registration pending |
| 9 | GitOps | DONE | ArgoCD managing both apps, SealedSecrets, auto-sync verified |
| 10 | Observability | DONE | Prometheus, Grafana, Loki, Jaeger, Kiali verified; both services tracing to Jaeger |
| 11 | Security | DONE | SealedSecrets, container hardening, NetworkPolicies, non-root verified |
| 12 | Destruction and Recovery | DONE | destroy.sh, verify.sh scripts created |
| 13 | Documentation | DONE | ADRs, setup guide, troubleshooting guide |
| 14 | Final Validation | DONE | Full e2e validation passed |

---

## Repository Status

| Repository | Remote | Branch | Last Push | Status |
|------------|--------|--------|-----------|--------|
| GitOpsLocalPipelinePoc | Vivid-Vortex-DevOps | main | 2026-06-21 | CLAUDE_LOCAL.md, CLAUDE_CLOUD.md, PROGRESS.md |
| local-platform-infra | Vivid-Vortex-DevOps | main | 2026-06-24 | Bootstrap scripts, Helm values, ArgoCD apps, Kind configs, deploy-apps.sh |
| go-crud-service | Vivid-Vortex-DevOps | local_main | 2026-06-22 | Local Kind deployment, CI/CD, ServiceMonitor, OTLP tracing |
| springboot-crud-service | Vivid-Vortex-DevOps | local_main | 2026-06-24 | Local Kind deployment, CI/CD, ServiceMonitor, OTLP tracing fixed |

---

## Phase 1: Architecture Review - DONE

- [x] CLAUDE_LOCAL.md design document created
- [x] Cloud-to-local substitutions documented
- [x] Kind vs K3s comparison documented
- [x] Self-hosted runner strategy defined
- [x] Dual registry (JFrog local) strategy defined
- [x] ArgoCD GitOps flow defined
- [x] Enterprise-grade CI/CD pipeline (cicd.yaml + promotions.yaml) designed
- [x] Branch strategy defined (TBD: main for infra, local_main for app repos)

## Phase 2: Prerequisites - DONE

- [x] WSL2 Ubuntu verified (running, version 2)
- [x] System specs verified (32GB RAM, 8 cores)
- [x] Docker available (via Docker Desktop, native Docker Engine migration pending)
- [x] kubectl installed
- [x] helm installed (v3.21.1, ~/bin)
- [x] kind installed (v0.27.0, ~/bin)
- [x] argocd CLI installed (v3.4.4, ~/bin)
- [x] git installed (v2.25.1)
- [ ] systemd enabled in WSL2
- [ ] Native Docker Engine in WSL2 (currently using Docker Desktop)
- [x] setup-prerequisites.sh script created

## Phase 3: Repository Structure - DONE

- [x] local-platform-infra directory structure generated
- [x] bootstrap/ scripts created
- [x] cluster/ Kind configs created (single-node, multi-node)
- [x] helm/ value files for all components
- [x] argocd/ application definitions
- [x] namespaces/ definitions
- [x] runner/ setup script
- [x] Makefile created
- [ ] .devcontainer/ not yet created
- [ ] docs/ directory not yet populated

## Phase 4: Bootstrap Foundation - DONE

- [x] Kind cluster "vvd-local" created (single-node, K8s v1.32.2)
- [x] All namespaces created (argocd, istio-system, monitoring, registry, applications-dev, metallb-system, ingress-nginx, sealed-secrets)
- [x] MetalLB installed and configured (IPv4: 172.23.0.200-250)
- [x] NGINX Ingress Controller installed

## Phase 5: Platform Components - DONE

- [x] Sealed Secrets (v0.31.0) - sealed-secrets namespace
- [x] ArgoCD (v3.4.4) - argocd namespace
- [x] PostgreSQL (v18.4.0) - applications-dev namespace
  - [x] go_service_db created
  - [x] springboot_service_db created
  - [x] app_db created
- [x] Prometheus + Grafana (kube-prometheus-stack v0.91.0) - monitoring namespace
- [x] Loki + Promtail (v2.9.3) - monitoring namespace
- [x] Jaeger (v2.19.0) - monitoring namespace
- [x] Istio base + istiod (v1.30.1) - istio-system namespace
- [x] Kiali (v2.27.0) - istio-system namespace
- [ ] JFrog Container Registry - not yet installed (registry namespace reserved)

## Phase 6: Go Service - DONE

- [x] Adapt Dockerfile for local deployment (golang:1.25-alpine builder)
- [x] Update Helm chart for local deployment (removed Azure Workload Identity from deployment.yaml, serviceaccount.yaml)
- [x] Update deployment values for local Kind cluster (image: go-crud-service, pullPolicy: Never, OTEL→Jaeger)
- [x] Create K8s Secret for database credentials (go-crud-db-credentials)
- [x] Docker build validated (distroless runtime, ~24MB image)
- [x] Image loaded into Kind via `kind load docker-image`
- [x] Helm lint and template rendering validated
- [x] Service deployed and running (2/2 pods, health checks passing)
- [x] CRUD API verified (Create, List products, /metrics endpoint)
- [x] go vet and unit tests pass (8/8 tests)
- [ ] GitHub Actions workflows update (deferred to Phase 8 — self-hosted runner not yet configured)

## Phase 7: Spring Boot Service - DONE

- [x] Dockerfile already uses eclipse-temurin:21 + distroless java21 — no changes needed
- [x] Update Helm chart for local deployment (removed Azure Workload Identity from deployment.yaml, serviceaccount.yaml)
- [x] Update deployment values for local Kind cluster (image: springboot-crud-service, pullPolicy: Never, OTEL→Jaeger HTTP)
- [x] Add spring-boot-flyway dependency (required in Spring Boot 4.x for Flyway auto-configuration)
- [x] Create K8s Secret for database credentials (springboot-crud-db-credentials: JDBC URL, username, password)
- [x] Docker build validated (layered JAR, distroless runtime)
- [x] Image loaded into Kind via `kind load docker-image`
- [x] Helm lint and template rendering validated
- [x] Service deployed and running (2/2 pods, health checks passing)
- [x] CRUD API verified (Create, List products, /actuator/health, /actuator/prometheus, Swagger UI)
- [x] Unit tests pass (Gradle BUILD SUCCESSFUL)
- [ ] GitHub Actions workflows update (deferred to Phase 8 — self-hosted runner not yet configured)

## Phase 8: CI/CD - DONE

- [x] Self-hosted runner binary downloaded and extracted (~/$HOME/actions-runner)
- [x] cicd.yaml workflow for go-crud-service (enterprise DAG: init → tests/security → build → helm → deploy → smoke test → telemetry)
- [x] cicd.yaml workflow for springboot-crud-service (same DAG pattern)
- [x] promotions.yaml workflow for both repos (workflow_dispatch, env selector)
- [x] PR validation workflows updated (branch: local_main, runs-on: [self-hosted, linux])
- [x] Image tagging strategy: v{version}-g{sha}, {sha}, latest
- [x] Pipeline artifacts: coverage, test results, SBOM uploads
- [x] Job summaries with ArgoCD links and deployment metadata
- [x] Image loading via `kind load docker-image` (JFrog deferred)
- [ ] Runner configuration and registration (requires GitHub token — interactive step for user)
- [ ] Runner systemd service (requires WSL2 systemd — deferred)

## Phase 9: GitOps - DONE

- [x] Apply ArgoCD AppProject (`applications`) to cluster
- [x] Apply ArgoCD Applications (go-crud-service-dev, springboot-crud-service-dev) to cluster
- [x] Create SealedSecrets for database credentials (go-crud-db, springboot-crud-db)
- [x] Install kubeseal CLI v0.29.0 in ~/bin
- [x] Commit and push all changes to GitHub (go-crud-service, springboot-crud-service, local-platform-infra)
- [x] Migrated from manual Helm management to ArgoCD-managed deployments
- [x] Verified ArgoCD auto-sync: both apps Synced/Healthy
- [x] End-to-end verification: CRUD APIs working through ArgoCD-managed deployments

## Phase 10: Observability - DONE

- [x] Grafana fixed (resolved dual-default datasource conflict between Prometheus and Loki)
- [x] Grafana datasources configured: Prometheus (default), Loki, Jaeger, Alertmanager
- [x] Prometheus ServiceMonitors created for both services (ArgoCD-synced via Helm templates)
- [x] Prometheus scraping both apps: go-crud-service (UP), springboot-crud-service (UP)
- [x] OpenTelemetry in Go service: traces flowing to Jaeger via gRPC OTLP (port 4317)
- [x] Loki collecting logs from all namespaces (including applications-dev)
- [x] Kiali service mesh visualization responsive
- [x] Jaeger UI accessible with Go service traces
- [x] Spring Boot OTLP tracing: working via spring-boot-starter-opentelemetry + corrected OTLP base endpoint (port 4318, no /v1/traces suffix)

## Phase 11: Security - DONE

- [x] Sealed Secrets controller installed and operational
- [x] SealedSecrets managing database credentials (SYNCED=True for both services)
- [x] Container security: allowPrivilegeEscalation=false, capabilities drop ALL, readOnlyRootFilesystem
- [x] Pod security: runAsNonRoot=true, runAsUser=65532 (distroless nonroot), fsGroup=65532
- [x] Dedicated ServiceAccounts per service (not using default SA)
- [x] NetworkPolicies: ingress restricted to port 8080, egress restricted to PostgreSQL+Jaeger+DNS
- [x] Namespace isolation: 13 namespaces with dedicated purposes
- [x] kubeseal CLI v0.29.0 installed for secret management

## Phase 12: Destruction and Recovery - DONE

- [x] destroy.sh script
- [x] verify.sh script
- [x] port-forward.sh script
- [x] Recovery documented (bootstrap.sh recreates everything)

## Phase 13: Documentation - DONE

- [x] CLAUDE_LOCAL.md (full design)
- [x] PROGRESS.md (this file)
- [x] local-platform-infra README.md
- [x] ADR-001: Kind over K3s
- [x] ADR-002: Sealed Secrets for GitOps
- [x] ADR-003: ArgoCD for GitOps
- [x] Setup guide (prerequisites, quick start, service access, runner setup)
- [x] Troubleshooting guide (Docker/WSL2, Kind, ArgoCD, Grafana, Sealed Secrets)

## Phase 14: Final Validation - DONE

- [x] Cluster: Kind vvd-local, K8s v1.32.2, node Ready
- [x] ArgoCD: Both apps Synced/Healthy, auto-sync working
- [x] Go service: 2/2 Running, CRUD API verified, Prometheus scraped, Jaeger traces flowing
- [x] Spring Boot service: 2/2 Running, CRUD API verified, Prometheus scraped
- [x] PostgreSQL: Running, both service databases accessible
- [x] Platform components: All running (ArgoCD, Istio, Kiali, Prometheus, Grafana, Loki, Jaeger, Sealed Secrets, MetalLB, Ingress)
- [x] Security: Non-root containers, read-only FS, NetworkPolicies, SealedSecrets
- [x] GitOps: Git push → ArgoCD sync → deployment verified end-to-end

---

## Access Information

| Service | Port-Forward Command | URL |
|---------|---------------------|-----|
| ArgoCD | `kubectl port-forward svc/argocd-server -n argocd 8080:443` | https://localhost:8080 |
| Grafana | `kubectl port-forward svc/kube-prometheus-stack-grafana -n monitoring 3000:80` | http://localhost:3000 |
| Prometheus | `kubectl port-forward svc/kube-prometheus-stack-prometheus -n monitoring 9090:9090` | http://localhost:9090 |
| Jaeger | `kubectl port-forward svc/jaeger -n monitoring 16686:16686` | http://localhost:16686 |
| Kiali | `kubectl port-forward svc/kiali -n istio-system 20001:20001` | http://localhost:20001 |

ArgoCD admin password: Retrieve with `kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`

---

## Pending Items / Deferred

1. **WSL2 systemd + Native Docker Engine** (deferred):
   - Update WSL2 to Store version (`Add-AppxPackage` — requires admin)
   - Enable systemd (PID 1) for native Docker Engine and runner systemd service
   - Currently using Docker Desktop (works fine for all platform operations)
   - `.wslconfig` already created (16GB RAM, 4 CPU, 4GB swap)
2. **JFrog Container Registry** (deferred): Using `kind load docker-image` — works for local dev, JFrog would add registry pull flow
3. **Self-hosted runner registration** (interactive): Binary at ~/actions-runner, needs `./config.sh` with GitHub token
4. **Runner systemd service**: Requires WSL2 systemd (item 1)

## Bootstrapping (New Machine)

Prerequisites: Windows 11 + WSL2 (Ubuntu) + Docker Desktop

```bash
# 1. Clone infra repo
git clone https://github.com/Vivid-Vortex-DevOps/local-platform-infra.git
cd local-platform-infra

# 2. Full setup (installs tools, creates cluster, deploys apps)
make all

# 3. Verify everything is healthy
make verify

# 4. Start port-forwards for UI access
make port-forward
```
