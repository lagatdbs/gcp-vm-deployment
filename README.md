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
# Check if required tools are installed
if ! command -v git &> /dev/null || ! command -v gcloud &> /dev/null; then
  echo "Error: Required tools (git, gcloud) are not installed."
  exit 1
fi

# Initialize a new Git repository
git init

# Add the remote repository
git remote add origin https://github.com/lagatdbs/gcp-vm-deployment.git

# Add the deployment script to staging
git add deploy_vm.sh

# Commit the changes with a message
git commit -m "Add deployment script"

# Rename the branch to match the remote default branch
git branch -M main

# Push the changes to the remote repository
git push -u origin main

chmod +x deploy_vm.sh
./deploy_vm.sh

# Wait for the VM to be ready
echo "Waiting for VM to be ready..."
sleep 30

gcloud compute ssh gcp-demo-vm --zone=us-central1-a

# Replace <STATIC_IP> with the actual static IP of the VM
STATIC_IP="192.168.1.100" # Replace with the actual static IP
echo "Access your application at: http://$STATIC_IP"
