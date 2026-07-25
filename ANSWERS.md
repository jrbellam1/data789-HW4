# HW4 — Short Answers

Answer each question in 2–3 sentences.

## 1. Rolling-update safety
Why does the Deployment use `maxUnavailable: 0`, and what would change if it were `maxUnavailable: 1`?

> With `maxUnavailable: 0`, Kubernetes must bring up new pods and wait until they pass readiness checks before removing any old pod, so all 3 replicas stay available throughout the rollout and no in-flight requests are dropped. If it were `maxUnavailable: 1`, one old pod could be terminated before its replacement is ready, which allows a faster rollout but risks temporarily reducing capacity and losing requests if the new pod fails its health checks.

## 2. Health probes
Why do the liveness/readiness probes target `/health` instead of `/predict`?

> `/health` is a lightweight check that reports the pod is up without touching Redis or the ML model, so it stays fast and cheap for the kubelet to call frequently. `/predict` is the expensive, user-facing endpoint that loads the model and calls out to Redis, so using it for probes would add unnecessary load, slow down real traffic, and could cause false failures unrelated to actual pod health.


## Note on rolling-update test observation

> During two separate rolling-update test runs (each sending one request per second via a curl loop pod), one single request out of ~148 total returned a connection error (HTTP 000) instead of 200, even with `maxUnavailable: 0`. This appears consistent across both runs, suggesting a brief kube-proxy/iptables routing gap during pod termination rather than random chance. 147/148 (and 147/148 on the repeat run) requests succeeded with no downtime otherwise observed.
