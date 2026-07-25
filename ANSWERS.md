# HW4 Short Answers

## 1. Why maxUnavailable: 0?

maxUnavailable: 0 keeps all 3 pods available during updates by requiring new pods to pass readiness checks before old ones shut down. This prevents dropped requests. At maxUnavailable: 1, one pod can terminate early, speeding up the rollout but risking capacity loss if the new pod fails health checks.

## 2. Why /health instead of /predict?

/health is lightweight and doesn't touch Redis or the ML model, keeping probes fast and cheap. /predict is expensive and user-facing, so using it for probes wastes resources and adds unnecessary load to real traffic.

## 3. HPA at capacity

If traffic doubles, CPU rises above 40% and HPA scales up toward 8 replicas. Once maxed out, HPA can't scale further, so latency increases and requests queue until demand drops.

## Azure deployment blocker

Part 3 (Azure deployment) failed due to subscription region restrictions. The free tier subscription only allows certain regions (westus3, canadacentral, northcentralus, southcentralus), and deploy_azure.sh defaults to centralus and eastus2, both blocked. Parts 1 and 2 (local Kubernetes with rolling update and blue-green deployment) are complete with screenshots.
