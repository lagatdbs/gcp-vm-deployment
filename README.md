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

#!/bin/bash

set -e  # Exit on error

# Variables
VM_NAME="my-vm-project-two"
ZONE="us-central1-a"
REGION="us-central1"
MACHINE_TYPE="n2-standard-2"
BOOT_DISK_SIZE="250GB"
IMAGE_FAMILY="ubuntu-2004-lts"
IMAGE_PROJECT="ubuntu-os-cloud"
TAG="http-server"

# Clean up existing resources if any
echo " Cleaning up existing resources if any..."
gcloud compute addresses delete ${VM_NAME}-ip --region=$REGION --quiet || true
gcloud compute instances delete $VM_NAME --zone=$ZONE --quiet || true
gcloud compute firewall-rules delete ${VM_NAME}-allow-http-ssh --quiet || true

# Reserve a static IP
echo " Reserving a static IP address..."
gcloud compute addresses create ${VM_NAME}-ip --region=$REGION

# Create VM instance
echo " Creating the VM instance..."
gcloud compute instances create $VM_NAME \
  --zone=$ZONE \
  --machine-type=$MACHINE_TYPE \
  --image-family=$IMAGE_FAMILY \
  --image-project=$IMAGE_PROJECT \
  --boot-disk-size=$BOOT_DISK_SIZE \
  --tags=$TAG \
  --address=${VM_NAME}-ip

# Set up firewall rules for HTTP and SSH access
echo " Creating firewall rules for HTTP and SSH access..."
gcloud compute firewall-rules create ${VM_NAME}-allow-http-ssh \
  --allow tcp:80,tcp:22 \
  --target-tags=$TAG \
  --description="Allow HTTP and SSH traffic"

# Install Apache if not already installed
echo " Installing Apache if not already installed..."
sudo apt update -y apache
sudo apt install -y apache2

# Start Apache service and enable it to start on boot
echo " Starting Apache service..."
sudo systemctl start apache2
sudo systemctl enable apache2

# Fetch the external IP of the VM
EXTERNAL_IP=$(gcloud compute instances describe $VM_NAME --zone=$ZONE --format='get(networkInterfaces[0].accessConfigs[0].natIP)')

# Output the external IP address
echo " VM is successfully deployed. Access your VM at: http://$EXTERNAL_IP"
