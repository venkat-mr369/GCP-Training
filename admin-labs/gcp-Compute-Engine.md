 **10 hands-on practice tasks** for **Google Cloud Platform (GCP) Compute Engine**
---

## ✅ **Task 1: Create a VM Instance**

**Question:**
How do you create a VM instance named `vm-practice-1` in the `us-central1-a` zone with an e2-micro machine type and Debian OS?

**Answer:**

```bash
gcloud compute instances create vm-practice-1 \
  --zone=us-central1-a \
  --machine-type=e2-micro \
  --image-family=debian-11 \
  --image-project=debian-cloud
```

---

## ✅ **Task 2: SSH into a VM**

**Question:**
How do you SSH into the `vm-practice-1` instance?

**Answer:**

```bash
gcloud compute ssh vm-practice-1 --zone=us-central1-a
```

---

## ✅ **Task 3: List All VM Instances**

**Question:**
How can you list all Compute Engine instances in the current project?

**Answer:**

```bash
gcloud compute instances list
```

---

## ✅ **Task 4: Start and Stop VM**

**Question:**
How do you stop and then start `vm-practice-1`?

**Answer:**

```bash
gcloud compute instances stop vm-practice-1 --zone=us-central1-a

gcloud compute instances start vm-practice-1 --zone=us-central1-a
```

---

## ✅ **Task 5: Create a VM with a Startup Script**

**Question:**
How can you create a VM that installs Apache during startup?

**Answer:**

```bash
gcloud compute instances create vm-webserver \
  --zone=us-central1-a \
  --machine-type=e2-micro \
  --metadata=startup-script='#! /bin/bash
    sudo apt update
    sudo apt install apache2 -y' \
  --image-family=debian-11 \
  --image-project=debian-cloud
```

---

## ✅ **Task 6: Attach External IP to VM**

**Question:**
How do you reserve a static IP and assign it to an existing VM?

**Answer:**

1. **Reserve static IP:**

```bash
gcloud compute addresses create my-static-ip \
  --region=us-central1
```

2. **Assign to VM:**

```bash
gcloud compute instances delete-access-config vm-practice-1 \
  --zone=us-central1-a

gcloud compute instances add-access-config vm-practice-1 \
  --zone=us-central1-a \
  --address=$(gcloud compute addresses describe my-static-ip --region=us-central1 --format='value(address)')
```

---

## ✅ **Task 7: Create a Custom Image from a VM**

**Question:**
How can you create a custom image from the boot disk of an existing VM?

**Answer:**

```bash
gcloud compute images create custom-image-1 \
  --source-disk=vm-practice-1 \
  --source-disk-zone=us-central1-a
```

---

## ✅ **Task 8: Create a VM from a Custom Image**

**Question:**
How can you create a new VM using the `custom-image-1` image?

**Answer:**

```bash
gcloud compute instances create vm-from-custom-image \
  --zone=us-central1-a \
  --machine-type=e2-micro \
  --image=custom-image-1 \
  --image-project=$(gcloud config get-value project)
```

---

## ✅ **Task 9: Add Tags and Firewall Rules**

**Question:**
How do you add a network tag `web-server` to a VM and create a firewall rule to allow HTTP?

**Answer:**

1. **Add tag to VM:**

```bash
gcloud compute instances add-tags vm-webserver \
  --tags=web-server \
  --zone=us-central1-a
```

2. **Create firewall rule:**

```bash
gcloud compute firewall-rules create allow-http \
  --allow=tcp:80 \
  --target-tags=web-server \
  --description="Allow HTTP traffic"
```

---

## ✅ **Task 10: Delete All VM Instances**

**Question:**
How do you delete all VMs in a project (use caution)?

**Answer:**

```bash
for instance in $(gcloud compute instances list --format="value(name)"); do
  gcloud compute instances delete $instance --zone=us-central1-a --quiet
done
```

---

Would you like this content in a **Word document with a GCP Visio-style diagram** included?
