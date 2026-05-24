# Me & Argo

## Argo CD App of Apps (`root-app.yaml`)

`root-app.yaml` is a bootstrap Argo CD Application that watches a Git folder containing other `Application` manifests.
a
Flow:

root-app -> watches `/apps` -> creates child apps -> child apps deploy workloads

Example structure:

gitops-repo/
├── root-app.yaml
├── apps/
│   ├── nginx-app.yaml
│   └── redis-app.yaml
└── workloads/

Bootstrap once:

kubectl apply -f root-app.yaml

After that:
- adding app YAML to `/apps` auto-creates apps
- deleting app YAML auto-removes apps/resources
- Git becomes source of truth