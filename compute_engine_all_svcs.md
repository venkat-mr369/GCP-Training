✅ Options from **Compute Engine** menu
✅ gcloud commands for each (Project = `approject-2025`, Region = `us-east1`)
✅ Usable for course content & practice

---

# 🚀 GCP Compute Engine Menu - Detailed Notes & gcloud Commands

**Project:** `approject-2025`
**Region:** `us-east1`

---

## 🖥️ **Virtual Machines Section**

---

### **1. VM Instances**

Run virtual machines (Linux, Windows).
You can choose CPU, memory, OS, network, and disk.

**gcloud command:**

```bash
gcloud compute instances list --project=approject-2025
```

Create:

```bash
gcloud compute instances create my-vm \
    --zone=us-east1-b \
    --machine-type=e2-medium \
    --image-family=debian-12 \
    --image-project=debian-cloud \
    --project=approject-2025
```

---

### **2. Instance Templates**

Reusable templates to create VMs with the same configuration.

**gcloud command:**

```bash
gcloud compute instance-templates list --project=approject-2025
```

Create:

```bash
gcloud compute instance-templates create my-template \
    --machine-type=e2-medium \
    --image-family=debian-12 \
    --image-project=debian-cloud \
    --project=approject-2025
```

---

### **3. Sole-tenant Nodes**

Dedicated physical server (no VM sharing) for compliance or licensing.

**gcloud command:**

```bash
gcloud compute sole-tenancy node-templates list --project=approject-2025
```

---

### **4. Machine Images**

Full VM image (OS + disk + metadata) to quickly clone.

**gcloud command:**

```bash
gcloud compute machine-images list --project=approject-2025
```

---

### **5. TPUs (Tensor Processing Units)**

Accelerators for ML workloads.

**gcloud command:**

```bash
gcloud compute tpus list --zone=us-east1-b --project=approject-2025
```

---

### **6. Committed Use Discounts**

Commit to 1-year/3-year usage → save \$.

Manage via Billing → no direct gcloud CLI.

---

### **7. Reservations**

Reserve vCPUs and memory in specific zones.

**gcloud command:**

```bash
gcloud compute reservations list --project=approject-2025
```

---

### **8. Migrate to Virtual Machines**

Migration tool → helps move workloads from on-premises to GCP.

No direct gcloud — done via **Migrate for Compute Engine** UI.

---

## 💾 **Storage Section**

---

### **1. Disks**

Persistent Disks (standard, SSD, Hyperdisk).

**gcloud command:**

```bash
gcloud compute disks list --project=approject-2025
```

---

### **2. Storage Pools**

New! Groups multiple disks into one performance pool (mainly for Hyperdisk ML & Throughput).

**gcloud command:**

```bash
gcloud compute storage-pools list --project=approject-2025
```

---

### **3. Snapshots**

Backup image of disks.

**gcloud command:**

```bash
gcloud compute snapshots list --project=approject-2025
```

Create:

```bash
gcloud compute disks snapshot my-disk \
    --snapshot-names=my-snapshot \
    --zone=us-east1-b \
    --project=approject-2025
```

---

### **4. Images**

Custom VM images.

**gcloud command:**

```bash
gcloud compute images list --project=approject-2025
```

---

### **5. Async Replication**

Async disk replication across regions (Disaster Recovery).

**gcloud command:**

```bash
gcloud compute disks replication list --project=approject-2025
```

---

## ⚙️ **Instance Groups Section**

---

### **1. Instance Groups**

Group of VMs → auto-healing, scaling.

**gcloud command:**

```bash
gcloud compute instance-groups list --project=approject-2025
```

Create:

```bash
gcloud compute instance-groups managed create my-group \
    --base-instance-name=my-vm \
    --template=my-template \
    --size=2 \
    --zone=us-east1-b \
    --project=approject-2025
```

---

### **2. Health Checks**

Probe to monitor VM health.

**gcloud command:**

```bash
gcloud compute health-checks list --project=approject-2025
```

---

## 🛠️ **VM Manager Section**

---

### **1. Patch**

Schedule and manage OS patches (Linux & Windows).

**gcloud command:**

```bash
gcloud compute os-config patch-jobs list --project=approject-2025
```

---

### **2. OS Policies**

Define compliance rules for OS config.

**gcloud command:**

```bash
gcloud compute os-config os-policy-assignments list --project=approject-2025
```

---

## 🖥️ **Bare Metal Solution Section**

---

### **1. Servers**

Bare-metal servers (for Oracle, SAP, VMware).

```bash
gcloud bare-metal solution instances list --project=approject-2025
```

---

### **2. Networks**

Networking for Bare Metal.

```bash
gcloud bare-metal solution networks list --project=approject-2025
```

---

### **3. VRFs (Virtual Routing & Forwarding)**

Network isolation — New in Bare Metal.

```bash
gcloud bare-metal solution vrfs list --project=approject-2025
```

---

### **4. Volumes**

Storage volumes for Bare Metal.

```bash
gcloud bare-metal solution volumes list --project=approject-2025
```

---

### **5. NFS Shares**

NFS storage for Bare Metal.

```bash
gcloud bare-metal solution nfs-shares list --project=approject-2025
```

---

### **6. Procurements**

Ordering new Bare Metal services.

```bash
gcloud bare-metal solution procurements list --project=approject-2025
```

---

### **7. Maintenance Events**

Upcoming hardware/software maintenance.

```bash
gcloud compute maintenance-policies list --project=approject-2025
```

---

## ⚙️ **Settings Section**

---

### **1. Metadata**

Project-wide metadata key/value pairs.

```bash
gcloud compute project-info describe --project=approject-2025
```

---

### **2. Zones**

Available zones in region.

```bash
gcloud compute zones list --filter="region:us-east1" --project=approject-2025
```

---

### **3. Network Endpoint Groups (NEG)**

Used in Load Balancers.

```bash
gcloud compute network-endpoint-groups list --project=approject-2025
```

---

### **4. Operations**

Async operation logs for compute.

```bash
gcloud compute operations list --project=approject-2025
```

---

### **5. Settings**

General settings (done in UI) — no gcloud command.

---

## ✅ Final Summary

| Section          | Key Functions                   |
| ---------------- | ------------------------------- |
| Virtual Machines | VM management, templates, TPUs  |
| Storage          | Disks, Snapshots, Storage Pools |
| Instance Groups  | MIGs, Health Checks             |
| VM Manager       | Patch mgmt, OS policies         |
| Bare Metal       | Servers, NFS, Volumes           |
| Settings         | Zones, Metadata, Operations     |

---

