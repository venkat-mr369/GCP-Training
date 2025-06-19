 **GCP Compute Engine services**, their components, and related `gcloud` commands:

---

## ☁️ **What is GCP Compute Engine?**

**Compute Engine** is the **Infrastructure-as-a-Service (IaaS)** of GCP.
It allows you to create and manage **Virtual Machines (VMs)** that run on Google’s infrastructure.

You can choose:
✅ VM size (CPU, memory)
✅ OS (Linux, Windows)
✅ Disk type (SSD, Standard HDD)
✅ Network (VPC, firewall, load balancer)
✅ Security options (IAM roles, Shielded VM)
✅ Automation (startup scripts, instance templates)

---

## 🔥 **Core Services / Features of Compute Engine**

### 1️⃣ **VM Instances**

* Virtual Machines (Linux or Windows)
* Custom or predefined machine types
* Ephemeral or persistent

```bash
# List VM instances
gcloud compute instances list
```

```bash
# Create a VM
gcloud compute instances create my-vm --zone=us-central1-a --machine-type=e2-medium --image-family=debian-12 --image-project=debian-cloud
```

---

### 2️⃣ **Instance Templates**

* Define a reusable VM config (template)
* Useful for creating Managed Instance Groups (MIGs)

```bash
# Create instance template
gcloud compute instance-templates create my-template --machine-type=e2-medium --image-family=debian-12 --image-project=debian-cloud
```

---

### 3️⃣ **Instance Groups**

* **Managed Instance Group (MIG):** Auto-healing, auto-scaling VMs
* **Unmanaged Instance Group:** Manual group of VMs

```bash
# Create Managed Instance Group (MIG)
gcloud compute instance-groups managed create my-group --base-instance-name=my-vm --template=my-template --size=3 --zone=us-central1-a
```

```bash
# Resize MIG
gcloud compute instance-groups managed resize my-group --size=5 --zone=us-central1-a
```

---

### 4️⃣ **Disks (Storage)**

* Persistent Disk: SSD / HDD
* Local SSD (faster, local to VM)

```bash
# List disks
gcloud compute disks list

# Create new disk
gcloud compute disks create my-disk --size=100GB --zone=us-central1-a --type=pd-ssd

# Attach disk to VM
gcloud compute instances attach-disk my-vm --disk=my-disk --zone=us-central1-a
```

---

### 5️⃣ **Snapshots**

* Back up disk as snapshot
* Useful for restore, cloning

```bash
# Create snapshot
gcloud compute disks snapshot my-disk --snapshot-names=my-snapshot --zone=us-central1-a
```

---

### 6️⃣ **Images**

* Custom images from snapshot
* For cloning or fleet deployment

```bash
# Create image from snapshot
gcloud compute images create my-image --source-snapshot=my-snapshot
```

---

### 7️⃣ **Networks & Firewall Rules**

* Control traffic to/from VMs
* Works with VPCs

```bash
# List firewall rules
gcloud compute firewall-rules list

# Create firewall rule
gcloud compute firewall-rules create allow-http --allow tcp:80 --target-tags http-server --direction=INGRESS --priority=1000 --network=default
```

---

### 8️⃣ **Load Balancers**

* Distribute traffic across VMs
* Global HTTP(S), TCP/UDP LBs

```bash
# Example: Backend Service for LB
gcloud compute backend-services create my-backend --protocol=HTTP --port-name=http --global
```

---

### 9️⃣ **Startup Scripts**

* Automate VM setup (install software, config)
* Example using metadata

```bash
# Create VM with startup script
gcloud compute instances create my-vm --zone=us-central1-a --metadata=startup-script='#!/bin/bash
apt-get update && apt-get install -y nginx'
```

---

### 10️⃣ **Shielded VMs & Confidential VMs**

* Advanced security & isolation

```bash
# Enable Shielded VM options during create
gcloud compute instances create my-secure-vm --shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --zone=us-central1-a
```

---

### 11️⃣ **Labels & Tags**

* Useful for automation, billing, firewall targeting

```bash
# Add label to VM
gcloud compute instances add-labels my-vm --labels=env=dev,team=backend --zone=us-central1-a
```

---

### 12️⃣ **Preemptible VMs / Spot VMs**

* Cheaper, short-lived VMs (good for batch jobs)

```bash
# Create Spot VM (preemptible)
gcloud compute instances create my-spot-vm --zone=us-central1-a --preemptible
```

---

## 📚 Summary of Compute Engine Key Services:

| Feature                    | Purpose                            |
| -------------------------- | ---------------------------------- |
| VM Instances               | Linux/Windows VMs                  |
| Instance Templates         | Define reusable VM configs         |
| Instance Groups (MIG)      | Scale, heal VM groups              |
| Persistent Disk            | Durable storage for VMs            |
| Snapshots & Images         | Backups and cloning                |
| Local SSD                  | Ultra-fast storage, attached to VM |
| VPC Firewall Rules         | Network access control             |
| Load Balancers             | Distribute incoming traffic        |
| Startup Scripts            | Automate VM setup                  |
| Shielded / Confidential VM | Advanced security features         |
| Preemptible / Spot VMs     | Low-cost short-duration workloads  |

---

### 🎁 BONUS: Enable Compute Engine API

```bash
gcloud services enable compute.googleapis.com --project=my-gcp-project
```

