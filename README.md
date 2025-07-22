🚀 Draft Infrastructure Setup for Python Microservices Startup (CI/CD, GitOps, Monitoring, Logging)

This guide describes how to deploy and operate a production-grade yet cost-effective infrastructure for your Python-based applications using:

- Kubernetes on DigitalOcean
- GitHub Actions for CI/CD
- ArgoCD for GitOps
- DigitalOcean Container Registry for Docker images
- PostgreSQL via Helm
- Prometheus + Grafana for monitoring
- Loki + Promtail for centralized logging

The setup includes separate environments (test and prod) and supports automatic deployments from different Git branches.

---

1. ☁️ Cloud Infrastructure: DigitalOcean Kubernetes

- Use DigitalOcean Kubernetes (DOKS) for managed, cost-efficient K8s
- Start with:
  - 1 control plane (free)
  - 2 worker nodes (2vCPU / 4GB RAM) ~ $48/month
- Namespaces: test, prod
- Optional: Use separate node pools per environment for better isolation

---

2. 🔐 GitHub Secrets Setup

Create these secrets in your GitHub repo under Settings → Secrets → Actions:

- DOCKER_REGISTRY
- DOCKER_USERNAME
- DOCKER_PASSWORD
- GIT_EMAIL
- GIT_USERNAME
- ARGOCD_AUTH_TOKEN (optional)

---

3. ⚙️ GitHub Actions CI/CD Pipeline

Create `.github/workflows/deploy.yml` with build, push, and Helm values update steps. ArgoCD will sync the image tag and deploy.

---

4. 🔁 ArgoCD GitOps Setup

- Install ArgoCD in your cluster
- Access UI via port-forward or configure ingress
- Get initial password via kubectl

---

5. 🎯 ArgoCD Application Manifests

- Create `test-app.yaml` and `prod-app.yaml`
- Point to Git paths and Helm value files
- Apply with `kubectl apply -f`

---

6. 📦 Helm Chart Structure (Python App)

Example `values.yaml` includes image repo, tag, port, and environment variables.

---

7. 🐘 PostgreSQL Installation (via Helm)

Install PostgreSQL in each namespace using the Bitnami chart.

---

8. 🔐 Secrets Management

- Store application secrets via `kubectl create secret`
- Reference them in Helm templates

---

9. 📊 Monitoring Setup — Prometheus + Grafana

Install with:
  helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring

Access Grafana via port-forward on port 3000 or configure ingress

---

10. 🔍 Logging Setup — Loki + Promtail

Install with:
  helm install loki grafana/loki-stack -n logging

Add Loki as a data source to Grafana for centralized logging.

---

💰 Estimated Monthly Cost

| Component                  | Cost Estimate      |
|----------------------------|--------------------|
| 2 Kubernetes Worker Nodes  | ~$48               |
| Container Registry         | Free (up to 5,000 pulls/month) |
| PostgreSQL (via Helm)      | ~$0 (runs in cluster) |
| ArgoCD                     | Free               |
| Prometheus + Grafana       | Free               |
| Loki + Promtail            | Free               |
| **Total**                  | **~$50–60/month**  |

---

✅ Summary

You now have:

- Git-driven CI/CD with GitHub Actions and ArgoCD
- Automated test/prod deployments
- Private container registry
- PostgreSQL databases per environment
- Monitoring and logging with open-source tools

All with low cost and scalable architecture.
