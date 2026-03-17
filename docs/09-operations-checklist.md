# Operations Checklist

## Baseline Daily Checks
These are the kinds of checks we found useful on a compact OpenShift AI application cluster.

### Cluster
```bash
oc get clusterversion version
oc get co
oc get nodes
oc get mcp
```

### Application namespace
```bash
oc -n private-ai-app get pods
oc -n private-ai-app get deploy
oc -n private-ai-app get hpa
oc -n private-ai-app get pdb
```

### Routes and health
```bash
curl -fsS https://chat.example.com/health
curl -fsS https://chat.example.com/api/meta
```

### Logs
```bash
oc -n private-ai-app logs deploy/ai-web --since=30m
oc -n private-ai-app logs deploy/ai-inference-gateway --since=30m
```

### OpenShift AI
```bash
oc -n private-ai-app get inferenceservice
oc -n ai-notebooks get notebooks
```

## Deployment Checklist
Before a deployment:
1. confirm git state is clean
2. confirm the target version is updated
3. confirm the namespace and overlay are correct
4. confirm any config changes are intended

After a deployment:
1. wait for rollout completion
2. verify route health
3. verify logs for fresh errors and warnings
4. verify the serving lane if model-serving code changed
5. verify cluster operators stayed healthy

## Upgrade Checklist
Before a cluster upgrade:
1. validate cluster operators and nodes are healthy
2. validate PDBs for app workloads
3. confirm no workloads are unintentionally pinned to nodes that cannot drain
4. review recent alerts and decide which are real versus historical noise

After a cluster upgrade:
1. verify app routes
2. verify storage health
3. verify serving endpoints
4. review pod restarts and operator alerts for upgrade-era noise
5. silence only the alerts that are clearly historical or environmentally accepted

## Demo-Call Checklist
1. confirm the public app route loads
2. confirm the app metadata endpoint responds
3. confirm the InferenceService is ready
4. confirm the notebook/demo workspace loads if you plan to show it
5. run one short prompt and one longer prompt before the meeting
6. keep one cluster view and one app view available for context
