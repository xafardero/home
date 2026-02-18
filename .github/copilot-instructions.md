Purpose
-------
This repository is a collection of Kubernetes manifests and small support tooling for deploying a home cluster (apps, ingress, storage, and monitoring). The guidance below highlights repository structure, common patterns, and concrete examples an AI coding agent should follow when making changes.

Big picture
-----------
- Monorepo of raw Kubernetes YAMLs organized per-application under top-level directories (e.g., `mealie/`, `pi-hole/`, `actualbudget/`, `monitoring/`).
- GitOps / deployment orchestration: `argocd/` contains manifests used to install/configure ArgoCD for automated deployments.
- Ingress & networking: `traefik/` and `metallb/` provide cluster ingress and external IP allocation. `cert-manager/` is used for certs.
- Monitoring & logging: `monitoring/` includes `prometheus/`, `grafana/`, `loki/`, and `kube-state-metrics` manifests.

Repository conventions (important)
--------------------------------
- Per-application layout: most apps follow this sequence of files when present: `namespace.yml`, `storage-class.yml`, `persistent-volume.yml`, `persistent-volume-claim.yml`, `deployment.yml` (or `deployment.yaml`), `service.yml`, `ingress.yml`.
- YAML files are raw manifests (not Helm charts). Modify fields conservatively and preserve resource `apiVersion` / `kind` semantics.
 - Namespaces are explicit: always check the app's `namespace.yml` (e.g., [mealie/namespace.yml](../mealie/namespace.yml#L1)).
- When adding services or ingresses, prefer following existing Traefik annotations and host patterns used in `argocd/ingress.yaml` and `traefik/README.md`.

Key integration points (reference examples)
-----------------------------------------
- ArgoCD: [argocd/install.yaml](../argocd/install.yaml#L1) and [argocd/ingress.yml](../argocd/ingress.yml#L1) — used to bootstrap GitOps flows.
- Traefik ingress config: [traefik/README.md](../traefik/README.md#L1) — ingress resources should match the cluster-wide Traefik setup.
- MetalLB: [metallb/ip-address-pool.yaml](../metallb/ip-address-pool.yaml#L1) — external IP assignment for services of type `LoadBalancer`.
- Cert-Manager: [cert-manager/issuer.yml](../cert-manager/issuer.yml#L1) — cluster certificate issuer used by ingresses.
- Monitoring: [monitoring/grafana/deployment.yaml](../monitoring/grafana/deployment.yaml#L1) and [prometheus/prometheus.yaml](../prometheus/prometheus.yaml#L1) — follow existing metric scrape and datasource conventions when adding targets.

Developer workflows and common commands
--------------------------------------
- Apply a single app locally for testing: `kubectl apply -f <app-dir>/namespace.yml && kubectl apply -f <app-dir>/` (example: `kubectl apply -f mealie/`). See [argocd/README.md](../argocd/README.md#L15) for usage hints.
- ArgoCD-driven changes: edits to manifests in this repo will be picked up by the ArgoCD installation if it is configured to watch this repo. Use the `argocd/` manifests to understand cluster-level app wiring.
- Inspect runtime: `kubectl -n <ns> get all`, `kubectl -n <ns> logs -f deploy/<name>` and `kubectl -n <ns> describe pod <pod>` — prefer using resource names mirrored in the matching `deployment.yml` files.

Project-specific patterns to preserve
-----------------------------------
- Resource ordering: repo assumes namespaces and storage classes are created before PV/PVC and deployments. When adding CI or automation, replicate that ordering.
- Persistent data: many apps include `persistent-volume*.yml` and `persistent-volume-claim.yml` (examples: [pi-hole/persistent-volume.yml](../pi-hole/persistent-volume.yml#L1), [mealie/persistent-volume-claim.yml](../mealie/persistent-volume-claim.yml#L1)). Do not convert PV/PVC to ephemeral volumes without confirming data migration.
- File naming: the repo mixes `.yml` and `.yaml`; match existing file extensions in the target directory.

What to change and what to avoid
--------------------------------
- Safe edits: tweak container images, resource requests/limits, environment variables, and Service/Ingress hostnames following examples in `mealie/` and `pi-hole/`.
- Risky edits: changing cluster-level components (Traefik, MetalLB, cert-manager, ArgoCD) — avoid unless explicit by the user; these affect the whole cluster.

If you need more from me
------------------------
- Tell me which app or area to change and whether to update manifests or add automation (CI/ArgoCD). Provide target namespace and desired hostnames when adding ingresses.
- If unsure about runtime state, request me to list current manifests for an app (I can show the files and recommended kubectl commands).

Feedback
--------
Please review and tell me any missing examples or local workflows (e.g., kustomize/Helm usage, external registries, or CI pipelines) to include.
