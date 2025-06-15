Here are ** practical lab work** on **GCP Regions & Zones**, each with a **question, explanations, and `gcloud` command** for hands-on practice.
---

### ✅ **Task 1: List All Available Regions**

**❓Question:**
How do you list all the available regions in your current project?

**📘 Explanation:**
Each region represents a specific geographical location; this command helps identify all available deployment regions.

**✅ Command:**

```bash
gcloud compute regions list
```

---

### ✅ **Task 2: List All Available Zones**

**❓Question:**
How do you list all zones available in a specific region like `us-central1`?

**📘 Explanation:**
Zones are isolated locations within regions. This helps plan for high availability and redundancy.

**✅ Command:**

```bash
gcloud compute zones list --filter="region:(us-central1)"
```

---

### ✅ **Task 3: Describe a Specific Region**

**❓Question:**
How do you get more details about the `asia-south1` region?

**📘 Explanation:**
Useful for knowing which zones are active and available in a region.

**✅ Command:**

```bash
gcloud compute regions describe asia-south1
```

---

### ✅ **Task 4: Set a Default Region and Zone**

**❓Question:**
How can you set the default region and zone to `us-central1` and `us-central1-a`?

**📘 Explanation:**
Setting defaults simplifies commands by avoiding repeated flags.

**✅ Command:**

```bash
gcloud config set compute/region us-central1
gcloud config set compute/zone us-central1-a
```

---

### ✅ **Task 5: Get Current Default Region and Zone**

**❓Question:**
How do you check your current default region and zone?

**📘 Explanation:**
Important when working in multi-region deployments.

**✅ Command:**

```bash
gcloud config get-value compute/region
gcloud config get-value compute/zone
```

---

### ✅ **Task 6: Create VM in a Different Region**

**❓Question:**
How do you create a VM in `europe-west1-b`?

**📘 Explanation:**
Deploying resources in different regions improves latency for local users.

**✅ Command:**

```bash
gcloud compute instances create vm-europe \
  --zone=europe-west1-b \
  --machine-type=e2-micro \
  --image-family=debian-11 \
  --image-project=debian-cloud
```

---

### ✅ **Task 7: Compare Zone Availability Across Regions**

**❓Question:**
How can you list which zones are UP in all regions?

**📘 Explanation:**
This helps avoid scheduling in unavailable zones.

**✅ Command:**

```bash
gcloud compute zones list --filter="status:UP"
```

---

### ✅ **Task 8: Create a Disk in One Region, Attach in Same Region**

**❓Question:**
Can you create and attach a regional disk within a region like `us-east1`?

**📘 Explanation:**
Regional disks provide higher availability by replicating across two zones.

**✅ Command:**

```bash
gcloud compute disks create regional-disk-1 \
  --region=us-east1 \
  --replica-zones=us-east1-b,us-east1-c \
  --size=100GB \
  --type=pd-standard
```

---

### ✅ **Task 9: Launch Instance Group Across Zones**

**❓Question:**
How do you deploy a managed instance group in `us-central1` across multiple zones?

**📘 Explanation:**
Instance groups can provide high availability and load balancing.

**✅ Command:**

```bash
gcloud compute instance-groups managed create mig-us-central \
  --base-instance-name=mig-instance \
  --template=INSTANCE_TEMPLATE_NAME \
  --size=2 \
  --zones=us-central1-a,us-central1-b
```

---

### ✅ **Task 10: Create Cloud Storage Bucket in Specific Region**

**❓Question:**
How to create a storage bucket in `asia-east1`?

**📘 Explanation:**
Bucket location affects data access latency and cost.

**✅ Command:**

```bash
gsutil mb -l asia-east1 gs://your-unique-bucket-name/
```

---

### ✅ **Task 11: Move VM from One Zone to Another (Snapshot + Create)**

**❓Question:**
How do you move a VM from `us-central1-a` to `us-central1-b`?

**📘 Explanation:**
You can’t directly move, but you can snapshot the disk and recreate in another zone.

**✅ Command:**

```bash
# Snapshot
gcloud compute disks snapshot vm-disk \
  --zone=us-central1-a \
  --snapshot-names=vm-disk-snap

# Create new disk from snapshot
gcloud compute disks create vm-disk-new \
  --zone=us-central1-b \
  --source-snapshot=vm-disk-snap

# Create new VM with new disk
gcloud compute instances create vm-new \
  --zone=us-central1-b \
  --disk=name=vm-disk-new,boot=yes,auto-delete=yes
```

---

### ✅ **Task 12: Deploy Load Balancer Across Zones**

**❓Question:**
How do you configure a GCP Load Balancer across multiple zones?

**📘 Explanation:**
Spreads load across zones for high availability.

**✅ Command (Outline):**

```bash
# Backend service creation includes instance group across zones
gcloud compute backend-services create my-service \
  --protocol=HTTP \
  --global

# Add backend (e.g., MIGs from multiple zones)
gcloud compute backend-services add-backend my-service \
  --instance-group=my-mig \
  --instance-group-zone=us-central1-a \
  --global
```

---

### ✅ **Task 13: Create Subnet in Custom Region**

**❓Question:**
How to create a custom subnet in `europe-west1`?

**📘 Explanation:**
Subnet control helps in fine-tuning IP ranges per region.

**✅ Command:**

```bash
gcloud compute networks subnets create custom-subnet \
  --network=default \
  --region=europe-west1 \
  --range=10.20.0.0/16
```

---

### ✅ **Task 14: Compare Pricing Between Regions**

**❓Question:**
How can you compare pricing for VM types across two regions?

**📘 Explanation:**
While `gcloud` doesn’t directly show pricing, you can inspect regions for availability and use [pricing calculator](https://cloud.google.com/products/calculator).

**✅ Tip:**
Use the below to compare machine types:

```bash
gcloud compute machine-types list --filter="zone~^us|europe" --format="table(name, zone, guestCpus, memoryMb)"
```

---

### ✅ **Task 15: Delete All Resources in a Region**

**❓Question:**
How to clean up all instances in `asia-south1-a`?

**📘 Explanation:**
Useful for teardown scripts in specific zones/regions.

**✅ Command:**

```bash
for vm in $(gcloud compute instances list --filter="zone:asia-south1-a" --format="value(name)"); do
  gcloud compute instances delete $vm --zone=asia-south1-a --quiet
done
```

---

