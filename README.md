# gcp-vm-deployment

This project contains a script to automate the deployment of a Virtual Machine (VM) and supporting resources on **Google Cloud Platform (GCP)** using the `gcloud` CLI. It is built as part of a practical lab exercise to demonstrate cloud automation and deployment skills.

--

# Project Overview

The script performs the following:
- Creates a VM instance with:
  - 2 vCPUs and 8 GB RAM
  - 250 GB boot disk
  - Ubuntu 20.04 LTS image
- Assigns a static external IP address
- Sets up firewall rules to allow:
  - HTTP (port 80)
  - SSH (port 22)
- Installs a web server (Apache or Nginx) and displays a “Hello World” page

--

# Prerequisites

Before running the script, ensure the following are set up:

# Installed Software
- [Google Cloud SDK (gcloud)](https://cloud.google.com/sdk/docs/install)
- Git
- Bash or Linux/MacOS terminal (or WSL on Windows)

# Google Cloud Configuration
- A Google Cloud Project created
- Billing account linked to the project
- Permissions:
  - `roles/owner`
  - `roles/compute.admin`
  - `roles/billing.admin` (for billing config if needed)

--

# Project Files

| File | Description |
|------|-------------|
| `deploy_vm.sh` | Bash script to deploy the VM and firewall rules |
| `README.md` | This file |

--

# How to Run the Script

1. **Clone the Repository**:

```bash
git clone https://github.com/lagatdbs/gcp-vm-deployment.git
cd gcp-vm-deployment
