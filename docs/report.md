# DevOps CA2 Report – PBL Backend (Fix My City auth API)

## Architecture
React frontend -> Express/Node API (JWT auth, bcrypt) -> MongoDB. The API exposes `/health` and `/metrics`.
Containerised with Docker, deployed on Kubernetes (3 replicas, RollingUpdate), observed by Prometheus + Grafana, host configured via Ansible, delivered by GitHub Actions.

## Pipeline flow
push -> test -> build image -> push to GHCR -> deploy to kind cluster -> verify rollout. See `pipeline-diagram.md`.

## Challenges (edit with your real ones)
- Keeping `package-lock.json` in sync so `npm ci` works in CI.
- Secrets: `.env` was in the repo; moved to Kubernetes Secret / env vars and `.gitignore`.
- Making readiness probes pass before MongoDB was ready.

## Lessons learned (edit)
- Zero-downtime updates need readiness probes and maxUnavailable: 0.
- Metrics (histogram + counter) give uptime, latency and error rate with 3 PromQL queries.
- IaC makes the environment reproducible.
