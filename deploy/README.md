# 🚀 Deploy Manager — Tier 4.5: GitOps Deployer

Welcome to the **deploy-manager** dashboard. This repository serves as the Tier 4.5 GitOps Deployer for the **RPDevs-Vault** organization, containing configuration maps, Docker Compose files, Ansible playbooks, and scripts to push release distributions to physical target servers.

* **Tier 0 Global Cockpit:** [github-manager](https://github.com/RPDevs-Vault/github-manager)
* **Tier 1 Governance Hub:** [vault-manager](https://github.com/RPDevs-Vault/vault-manager)
* **Tier 2 Builder Fleet:** [container-manager](https://github.com/RPDevs-Vault/container-manager)
* **Tier 3 Project Roadmap:** [project-manager](https://github.com/RPDevs-Vault/project-manager)
* **Tier 4 Release Gateway:** [distributor-manager](https://github.com/RPDevs-Vault/distributor-manager)
* **Tier 5 Knowledge Core:** [thought-manager](https://github.com/RPDevs-Vault/thought-manager)

---

## 🏛️ Structure

- **`/ansible/`**: Server provisioning and target playbook mappings.
- **`/compose/`**: Hardened production docker compose configuration matrices.
- **`/scripts/`**: Orchestration and deployment runner shell scripts.

---

## 🛠️ Usage

### Triggering Deployments Manually
You can run the deployment sequence locally by passing the service target:
```bash
./scripts/deploy_service.sh my-service
```
