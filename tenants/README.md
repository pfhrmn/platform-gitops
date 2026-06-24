# Tenants

Tenant definitions will be added here through pull requests.

Release flow:
- update `release-staging.yaml` to test a new backend or frontend version in the staging tenant
- validate the staging tenant after Argo CD and Crossplane reconciliation
- promote the same image references into `release-production.yaml`
- all tenants labeled `platform.it-n.at/release-channel=production` reconcile to the promoted version through one Git change

Tenant onboarding flow:
- create a GitHub issue for the tenant
- add a tenant manifest
- review through pull request
- merge to `main`
- Argo CD and Crossplane reconcile the tenant instance

No tenant secrets are allowed in this directory.

Current production tenant:

- `production.gcloud.it-n.at`

Current staging tenant:

- `staging.gcloud.it-n.at`
