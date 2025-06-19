Here are **20 Practical Hands-on Exercises** for **GCP Compute Engine** (using your project `dev-project33`)
✅ All with `gcloud` commands
✅ Realistic tasks
✅ Useful for learning & practice

---

## 🚀 GCP Compute Engine Hands-on Tasks (Project: `dev-project33`)

---

### 🏁 **Task 1: Enable Compute Engine API**

```bash
gcloud services enable compute.googleapis.com --project=dev-project33
```

---

### 🏁 **Task 2: List existing VM instances**

```bash
gcloud compute instances list --project=dev-project33
```

---

### 🏁 **Task 3: Create a basic Linux VM**

```bash
gcloud compute instances create my-vm-1 \
  --zone=us-central1-a \
  --machine-type=e2-medium \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --project=dev-project33
```

---

### 🏁 **Task 4: SSH into VM**

```bash
gcloud compute ssh my-vm-1 --zone=us-central1-a --project=dev-project33
```

---

### 🏁 **Task 5: Create a VM with a startup script**

```bash
gcloud compute instances create my-vm-2 \
  --zone=us-central1-a \
  --metadata=startup-script='#!/bin/bash
sudo apt-get update
sudo apt-get install -y nginx' \
  --project=dev-project33
```

---

### 🏁 **Task 6: Create an Instance Template**

```bash
gcloud compute instance-templates create my-template \
  --machine-type=e2-medium \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --project=dev-project33
```

---

### 🏁 **Task 7: Create Managed Instance Group (MIG)**

```bash
gcloud compute instance-groups managed create my-mig \
  --base-instance-name=my-vm \
  --template=my-template \
  --size=2 \
  --zone=us-central1-a \
  --project=dev-project33
```

---

### 🏁 **Task 8: Resize the Instance Group**

```bash
gcloud compute instance-groups managed resize my-mig \
  --size=5 \
  --zone=us-central1-a \
  --project=dev-project33
```

---

### 🏁 **Task 9: Create a Persistent Disk**

```bash
gcloud compute disks create my-disk \
  --size=100GB \
  --type=pd-ssd \
  --zone=us-central1-a \
  --project=dev-project33
```

---

### 🏁 **Task 10: Attach Disk to VM**

```bash
gcloud compute instances attach-disk my-vm-1 \
  --disk=my-disk \
  --zone=us-central1-a \
  --project=dev-project33
```

---

### 🏁 **Task 11: Create Snapshot of Disk**

```bash
gcloud compute disks snapshot my-disk \
  --snapshot-names=my-snapshot-1 \
  --zone=us-central1-a \
  --project=dev-project33
```

---

### 🏁 **Task 12: Create Custom Image from Snapshot**

```bash
gcloud compute images create my-image-1 \
  --source-snapshot=my-snapshot-1 \
  --project=dev-project33
```

---

### 🏁 **Task 13: Create VM from Custom Image**

```bash
gcloud compute instances create my-vm-custom \
  --zone=us-central1-a \
  --machine-type=e2-medium \
  --image=my-image-1 \
  --project=dev-project33
```

---

### 🏁 **Task 14: Create Firewall Rule (Allow HTTP)**

```bash
gcloud compute firewall-rules create allow-http \
  --allow tcp:80 \
  --target-tags=http-server \
  --direction=INGRESS \
  --priority=1000 \
  --network=default \
  --project=dev-project33
```

---

### 🏁 **Task 15: Create Spot (Preemptible) VM**

```bash
gcloud compute instances create my-spot-vm \
  --zone=us-central1-a \
  --machine-type=e2-medium \
  --preemptible \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --project=dev-project33
```

---

### 🏁 **Task 16: Add Labels to VM**

```bash
gcloud compute instances add-labels my-vm-1 \
  --labels=env=dev,team=infra \
  --zone=us-central1-a \
  --project=dev-project33
```

---

### 🏁 **Task 17: Create Shielded VM**

```bash
gcloud compute instances create my-shielded-vm \
  --zone=us-central1-a \
  --shielded-secure-boot \
  --shielded-vtpm \
  --shielded-integrity-monitoring \
  --machine-type=e2-medium \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --project=dev-project33
```

---

### 🏁 **Task 18: List Available Machine Types**

```bash
gcloud compute machine-types list --zones=us-central1-a --project=dev-project33
```

---

### 🏁 **Task 19: List Available Images**

```bash
gcloud compute images list --project=debian-cloud
```

---

### 🏁 **Task 20: Delete VM**

```bash
gcloud compute instances delete my-vm-1 \
  --zone=us-central1-a \
  --quiet \
  --project=dev-project33
```

---

### 📚 Summary:

✅ 20 Practical GCP Compute Engine exercises
✅ Real `gcloud` CLI examples
✅ Useful for day-to-day cloud operations
✅ Reusable for automation/scripts/DevOps

---

If you want, I can also prepare:
✅ Word document version (with sections)
✅ Visio-style architecture for **Compute Engine + MIG + LB**
✅ Advanced Tasks (Load Balancer, Autoscaler, MIG with health checks)

Would you like me to generate those too? 🚀
