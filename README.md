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

```
kubectl apply -f root-app.yaml
```

After that:
- adding app YAML to `/apps` auto-creates apps
- deleting app YAML auto-removes apps/resources
- Git becomes source of truth


> Without App of Apps, Argo CD only watches already-existing `Application` CRs, so workload changes in Git auto-sync, but new app manifests (like `apps/foo-app.yaml`) require manual bootstrap via `kubectl apply -f foo-app.yaml` because Argo cannot auto-discover new `Application` objects from Git. App of Apps solves this by creating one bootstrap `root-app.yaml` (`kubectl apply -f root-app.yaml`) that watches the `/apps` folder itself, causing Argo to continuously reconcile child `Application` manifests too — so adding/removing app YAMLs in Git automatically creates/deletes apps and their resources, making Git the full control plane.