# Sanitized OpenShift Examples

These examples show the general shape we found useful for a private AI application on OpenShift.

They are sanitized by design:
- no real domains
- no real cluster names
- no real namespaces from the private platform
- no private model catalogs or product logic

Files included:
- `namespace.yaml`: generic namespace and labels
- `configmap.yaml`: non-secret runtime settings
- `deployment-web.yaml`: web/API deployment with streaming-friendly Gunicorn settings
- `deployment-inference-gateway.yaml`: generic inference gateway deployment
- `service-and-route.yaml`: service and route examples
- `inferenceservice-amd-vulkan.yaml`: generic KServe example for a dedicated AI worker
- `hpa-and-pdb.yaml`: baseline HPA and PDB examples
- `kustomization.yaml`: simple composition entrypoint

What these examples are meant to teach:
- keep the app plane and inference plane distinct
- label and pin dedicated AI-serving nodes intentionally
- use HPA and PDB conservatively on compact clusters
- always pair manifests with rollout verification and logs
