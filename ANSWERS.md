# HW4 Short Answers

## 1. Why maxUnavailable: 0?

maxUnavailable: 0 keeps all 3 pods available during updates by requiring new pods to pass readiness checks before old ones shut down. This prevents dropped requests. At maxUnavailable: 1, one pod can terminate early, speeding up the rollout but risking capacity loss if the new pod fails health checks.

## 2. Why /health instead of /predict?

/health is lightweight and doesn't touch Redis or the ML model, keeping probes fast and cheap. /predict is expensive and user-facing, so using it for probes wastes resources and adds unnecessary load to real traffic.

## 3. HPA at capacity

If traffic doubles, CPU rises above 40% and HPA scales up toward 8 replicas. Once maxed out, HPA can't scale further, so latency increases and requests queue until demand drops.

## Azure deployment notes

Part 3 (Azure deployment) required editing config.env to use canadacentral region due to subscription restrictions on eastus2, eastus, and centralus. Successfully deployed to Azure Container Apps and verified /predict endpoint returned 200 with fraud predictions.
