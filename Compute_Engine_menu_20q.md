Practice tasks (20) with questions & answers

Here are **20 Practice Tasks with Questions & Answers** for your GCP Compute Engine menu (based on your screenshots):
✅ Project: `approject-2025`
✅ Region: `us-east1`
✅ gcloud commands included
✅ Great for your course & real practice

---

# 🚀 **GCP Compute Engine Practice Tasks (20 Tasks)**

---

### 🏁 **Task 1:** List all VM instances

**Question:** How do you list all VM instances in project `approject-2025`?

**Answer:**

```bash
gcloud compute instances list --project=approject-2025
```

---

### 🏁 **Task 2:** Create a Linux VM instance in `us-east1-b`

```bash
gcloud compute instances create test-vm \
    --zone=us-east1-b \
    --machine-type=e2-medium \
    --image-family=debian-12 \
    --image-project=debian-cloud \
    --project=approject-2025
```

---

### 🏁 **Task 3:** Create an Instance Template

```bash
gcloud compute instance-templates create my-template \
    --machine-type=e2-medium \
    --image-family=debian-12 \
    --image-project=debian-cloud \
    --project=approject-2025
```

---

### 🏁 **Task 4:** List available Sole-tenant node templates

```bash
gcloud compute sole-tenancy node-templates list --project=approject-2025
```

---

### 🏁 **Task 5:** Create a Machine Image

```bash
gcloud compute machine-images create my-machine-image \
    --source-instance=test-vm \
    --zone=us-east1-b \
    --project=approject-2025
```

---

### 🏁 **Task 6:** List available TPUs

```bash
gcloud compute tpus list --zone=us-east1-b --project=approject-2025
```

---

### 🏁 **Task 7:** Create a VM reservation

```bash
gcloud compute reservations create my-reservation \
    --zone=us-east1-b \
    --machine-type=e2-medium \
    --vm-count=2 \
    --project=approject-2025
```

---

### 🏁 **Task 8:** List all Disks

```bash
gcloud compute disks list --project=approject-2025
```

---

### 🏁 **Task 9:** Create a Hyperdisk Balanced Disk (100 GB)

```bash
gcloud compute disks create hyperdisk-balanced-disk \
    --type=hyperdisk-balanced \
    --size=100GB \
    --zone=us-east1-b \
    --project=approject-2025
```

---

### 🏁 **Task 10:** Create a Snapshot

```bash
gcloud compute disks snapshot hyperdisk-balanced-disk \
    --snapshot-names=my-snapshot \
    --zone=us-east1-b \
    --project=approject-2025
```

---

### 🏁 **Task 11:** Create a Managed Instance Group

```bash
gcloud compute instance-groups managed create my-mig \
    --base-instance-name=my-vm \
    --template=my-template \
    --size=2 \
    --zone=us-east1-b \
    --project=approject-2025
```

---

### 🏁 **Task 12:** Create a Health Check

```bash
gcloud compute health-checks create http my-http-hc --project=approject-2025
```

---

### 🏁 **Task 13:** List all Patch jobs

```bash
gcloud compute os-config patch-jobs list --project=approject-2025
```

---

### 🏁 **Task 14:** List OS Policies

```bash
gcloud compute os-config os-policy-assignments list --project=approject-2025
```

---

### 🏁 **Task 15:** List Bare Metal Servers

```bash
gcloud bare-metal solution instances list --project=approject-2025
```

---

### 🏁 **Task 16:** List Bare Metal Networks

```bash
gcloud bare-metal solution networks list --project=approject-2025
```

---

### 🏁 **Task 17:** List Zones in `us-east1`

```bash
gcloud compute zones list --filter="region:us-east1" --project=approject-2025
```

---

### 🏁 **Task 18:** List Metadata for project

```bash
gcloud compute project-info describe --project=approject-2025
```

---

### 🏁 **Task 19:** List Network Endpoint Groups

```bash
gcloud compute network-endpoint-groups list --project=approject-2025
```

---

### 🏁 **Task 20:** List Compute Engine Operations

```bash
gcloud compute operations list --project=approject-2025
```

---

## 🎁 Bonus:

If you want, I can also prepare:

✅ Practice **Quiz Q\&A**
✅ 10 **Troubleshooting tasks**
✅ Word Doc of these 20 Tasks
✅ **Terraform lab** for compute+disk+MIG

Would you like me to generate those also? 🚀
