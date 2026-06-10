# dvp-lab

Hands-on lab: Kubernetes, Terraform, GCP, CI/CD and infrastructure automation.

This repository documents my practical work with modern DevOps tools and cloud infrastructure. Each section contains working configurations, deployment manifests, and infrastructure code built while experimenting with real environments, local and cloud.

## Stack

- **Kubernetes / GKE** - workload management, deployments, services, ingress, autoscaling
- **Terraform** - infrastructure as code with Google Cloud Provider
- **Google Cloud Platform** - GKE, GCS, VPC, IAM, Workload Identity
- **CI/CD** - GitLab CI and GitHub Actions pipelines integrated with GKE deployments
- **Docker** - containerization and local development

## Structure

```
dvp-lab/
├── kubernetes/              # Manifests and experiments with Minikube and GKE
│   ├── deployments/
│   ├── services/
│   ├── ingress/
│   └── autoscaling/
├── terraform/               # GCP infrastructure as code
│   ├── modules/
│   │   ├── vpc/
│   │   ├── gke-cluster/
│   │   └── gcs-backend/
│   └── environments/
│       ├── dev/
│       └── prod/
├── ci-cd/                   # Pipeline configurations targeting GKE
│   ├── gitlab/
│   └── github-actions/
└── docs/                    # Notes and references
```

## Progress

### Kubernetes
- [x] Local cluster setup with Minikube
- [x] Deployments, ReplicaSets, rolling updates and rollbacks
- [x] Services: ClusterIP, NodePort, LoadBalancer
- [x] Ingress and path-based routing
- [x] ConfigMaps and Secrets management
- [ ] Persistent Volumes and StorageClass
- [ ] Namespace isolation and ResourceQuota
- [ ] Horizontal Pod Autoscaler
- [ ] GKE cluster on Google Cloud

### Terraform
- [ ] GCP provider setup and remote state on GCS
- [ ] VPC and subnet configuration
- [ ] GKE cluster provisioning
- [ ] IAM and Workload Identity
- [ ] Environment separation (dev/prod)
- [ ] Modular structure and reusable components

### CI/CD on GKE
- [ ] GitLab CI pipeline with GKE deployment stage
- [ ] GitHub Actions workflow with Workload Identity Federation (keyless auth)
- [ ] Environment-based promotion (dev -> prod)
- [ ] Automated rollback on failed deployment
- [ ] Pipeline-driven Terraform apply with state locking

## Environment

- Local: Minikube on macOS
- Cloud: Google Cloud Platform (GKE, GCS)
- IaC: Terraform with GCP provider
