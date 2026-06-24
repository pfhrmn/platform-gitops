# platform-gitops

GitOps repository for our Kubernetes platform. ArgoCD reconciles this repository
onto the GKE cluster.

## Documentation

- **[Crossplane Tenant SaaS Platform (Person 2 / Day 2)](docs/crossplane-tenant-platform.md)**
  — how tenant instances of the 3-tier SaaS application are provisioned on-the-fly via
  GitOps + Crossplane: the `XTenantApplication` XRD/Composition, per-tenant CloudNativePG
  database, soft multi-tenancy, per-tenant HTTPS ingress, and the staging-first update model.

## Related repositories

| Repo | Purpose |
|---|---|
| [`infra-iac-gke`](https://github.com/pfhrmn/infra-iac-gke) | IaC: GKE cluster, network, IAM / Workload Identity, ArgoCD bootstrap |
| `platform-gitops` (this repo) | GitOps: platform services + the Crossplane tenant layer |

## Layout

- `base/` — platform services and the ArgoCD app-of-apps root
- `base/platform/` — Crossplane control plane + tenant API (`applications/`, `crossplane-providers/`, `crossplane-config/`, `tenant-api/`)
- `tenants/` — tenant instances + image-release config (staging-first rollout)
