# 🚀 Delivery Manager — Tier 3: Release & GitOps Core

Welcome to the **Delivery Manager**, the central control tower for software release packaging, host provisioning, and GitOps target deployments across the RPDevs ecosystem.

This repository consolidates two legacy modules:
1. `distributor-manager` (Release building, publishing gateways, and cross-platform releases orchestration).
2. `deploy-manager` (Ansible deployment inventory, docker-compose blueprint mappings, and local service deployment scripts).

---

## 🏛️ Directory Structure & Layout

```
├── .github/workflows/         # GHA workflows (Publishing gateway and GitOps deploy triggers)
├── deploy/                    # GitOps Provisioning & Blueprints (Legacy deploy-manager)
│   ├── ansible/               # Ansible Playbooks & Host Inventories
│   │   └── hosts.ini          # Inventory mapping targets (e.g. T430-Runner, local-host)
│   ├── compose/               # Target service Docker Compose configuration blueprints
│   │   └── runner.compose.yml # Fleet runner orchestrator compose template
│   └── scripts/               # Automation scripts for target rolling deployments
│       └── deploy_service.sh  # Staged container initialization utility
└── release/                   # Release Catalog & Triggers (Legacy distributor-manager)
    └── README.md              # Documented manual release procedures
```

---

## ⚙️ Core Deployment & Release Workflows

Automated release and provisioning jobs are executed via workflows under `.github/workflows/`:

| Workflow File | Trigger Condition | Role / Process |
| :--- | :--- | :--- |
| **`publish-release.yml`** | Tag pushes (`v*`) or Manual | Compiles production-ready build artifacts, runs target package packaging, uploads release files, and creates GitHub Releases. |
| **`gitops-trigger.yml`** | Repository Dispatch (`deploy_target`) | Triggers automated host-level provisioning via Ansible playbooks to deploy target services defined inside `/deploy/compose/`. |

---

## 🚀 Execution & Operational Guide

### 1. Triggering Service Deployments Locally
Local rolling service deployments can be executed via the `deploy_service.sh` script:
```bash
cd deploy/scripts

# Usage: ./deploy_service.sh <service_name> [dry_run_true_false]
# Example (Dry Run):
./deploy_service.sh runner true

# Example (Execute):
./deploy_service.sh runner false
```
The script performs the following operations:
1. **Validation**: Validates the syntax of the corresponding Docker Compose configuration inside `/deploy/compose/<service_name>.compose.yml`.
2. **Layer Pulling**: Authenticates and pulls latest OCI layers from GHCR.
3. **Rolling Containers**: Gracefully restarts/upgrades containers to avoid downtime and checks system initialization logs.

### 2. Ansible Host Provisioning
Ansible configurations are managed under `deploy/ansible/`. The `hosts.ini` file catalogs our target environments:
```ini
[runners]
T430-Runner ansible_host=192.168.1.150 ansible_user=llmuser
local-host ansible_host=localhost ansible_connection=local
```
To run playbooks manually:
```bash
ansible-playbook -i deploy/ansible/hosts.ini deploy/ansible/site.yml
```

---

## 🔒 Security & Deployment Heuristics

* **Decoupled Architecture**: Absolute separation is enforced between the build phase (`builder-manager`) and the distribution phase (`delivery-manager`). Build servers have no direct shell access or SSH keys to production deployment targets.
* **Staged Container Init**: Containers are hardcoded to boot sequentially, ensuring databases are fully online, migrated, and accepting queries before dependent application containers are initialized.
* **Symmetric Age Secrets**: Hardened deployments retrieve decrypted environment configurations dynamically in-memory via physical FIDO2 keys, preventing persistent secret exposures on build runners.
