# AI Tooling — Usage & Evaluation

> The assignment explicitly expects use of generative AI tools and that we *"include our
> evaluation and usage of the tools"*. This documents how AI was used for the platform work
> in this repository, and our critical assessment of it.

## Tool

- **Claude Code (Claude Opus 4.x)** — an agentic CLI assistant that could read/write the
  manifests in this repo and run `gcloud` / `kubectl` / `git` / `gh` against the live setup.

## Where it was used

- **Porting** the Crossplane tenant API (`XTenantApplication` XRD + Composition) and the
  tenants into this GitOps structure as an additive app-of-apps (with sync-waves).
- **Bootstrapping & debugging the live platform** on GKE: making Crossplane schedulable
  (node capacity), creating the Cloud DNS zone, fixing ExternalDNS (domain filter + the
  missing RBAC that made it crash), wiring cert-manager DNS-01 via Workload Identity +
  the `letsencrypt-dns01` ClusterIssuer, building the app images via Cloud Build, and
  resolving an Ingress host conflict that masked the app with a default-nginx page.
- **Documentation**, the capacity & cost planning issue, and reconciling the live fixes
  back into Git so the platform stays reproducible.

## Evaluation — what worked, where human judgement was essential

- **Strong accelerator** for manifest authoring, multi-step debugging, and orchestrating
  several tools together — decisive for a tight (2-day) delivery.
- **Human verification was repeatedly necessary:**
  - An `HTTP 200` was initially the **default nginx page**, not our app — caught only by a
    teammate's **screenshot of the rendered page**. Lesson: verify *behaviour*, not status codes.
  - The GCP project topology (old vs. new project) was misread at first and corrected after
    a teammate shared the real project info.
  - **Sensitive / irreversible actions were gated:** resource deletions, cross-project IAM
    grants, and admin-collaborator grants were blocked until a human explicitly authorized them.
- **Net assessment:** the AI was a major force-multiplier, but the team's reviews, screenshots,
  and explicit authorizations were the safety net and remained essential. Output was treated
  as a draft to verify, not as ground truth.

## Hygiene

- No secrets are committed; credentials use Workload Identity and auto-generated secrets.
- Decisions and corrections are also captured in commit messages and pull requests (audit trail).
