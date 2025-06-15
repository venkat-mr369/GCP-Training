Hands on practice tasks involving **Google Cloud Platform** (GCP) **Compute Engine**

## 1. Create a VM with specific machine type and image
**Question:** Create a VM instance named `test-vm-1` with 4 CPUs using the Debian 10 image in zone `us-central1-a`.

**Answer:**
```bash
gcloud compute instances create test-vm-1 --machine-type=n1-standard-4 --image-family=debian-10 --image-project=debian-cloud --zone=us-central1-a
```
This command creates a VM with the specified machine type and image family in the given zone[1][2].

## 2. List all VM instances in a project filtered by zone
**Question:** List all Compute Engine instances in the zone `us-central1-b`.

**Answer:**
```bash
gcloud compute instances list --filter="zone:(us-central1-b)"
```
This filters the instances to only those in the specified zone[3].

## 3. SSH into a VM instance
**Question:** Connect via SSH to the instance `test-vm-1` in zone `us-central1-a`.

**Answer:**
```bash
gcloud compute ssh test-vm-1 --zone=us-central1-a
```
This command opens an SSH session to the VM[3].

## 4. Start and stop a VM instance
**Question:** Stop the VM `test-vm-1` in zone `us-central1-a`, then start it again.

**Answer:**
```bash
gcloud compute instances stop test-vm-1 --zone=us-central1-a
gcloud compute instances start test-vm-1 --zone=us-central1-a
```
These commands control the VM lifecycle[3].

## 5. Create a VM with a startup script
**Question:** Create a VM named `web-server` in `us-central1-a` that installs Apache HTTP server on startup.

**Answer:**
```bash
gcloud compute instances create web-server --zone=us-central1-a --metadata startup-script='#! /bin/bash
sudo apt-get update
sudo apt-get install -y apache2
sudo systemctl start apache2'
```
This uses the `--metadata startup-script` option to run a script on VM boot[1].

## 6. Add a firewall rule to allow HTTP traffic
**Question:** Allow incoming HTTP traffic on port 80 to all instances in the default network.

**Answer:**
```bash
gcloud compute firewall-rules create allow-http --allow tcp:80 --network default --direction INGRESS --priority 1000 --target-tags http-server
```
This creates a firewall rule to allow HTTP traffic[1].

## 7. Create a VM with a specific static external IP
**Question:** Reserve a static external IP named `my-static-ip` in `us-central1`, then create a VM `static-vm` using that IP.

**Answer:**
```bash
gcloud compute addresses create my-static-ip --region=us-central1
gcloud compute instances create static-vm --zone=us-central1-a --address=my-static-ip --image-family=debian-10 --image-project=debian-cloud
```
This reserves a static IP and assigns it to the VM when created[1].

## 8. Resize a VM's machine type
**Question:** Change the machine type of `test-vm-1` in `us-central1-a` to `n1-standard-2`.

**Answer:**
```bash
gcloud compute instances stop test-vm-1 --zone=us-central1-a
gcloud compute instances set-machine-type test-vm-1 --machine-type=n1-standard-2 --zone=us-central1-a
gcloud compute instances start test-vm-1 --zone=us-central1-a
```
You must stop the instance before changing its machine type[1].

## 9. Create a snapshot of a persistent disk
**Question:** Create a snapshot named `snapshot-1` of the disk `test-disk` in zone `us-central1-a`.

**Answer:**
```bash
gcloud compute disks snapshot test-disk --snapshot-names=snapshot-1 --zone=us-central1-a
```
This command creates a snapshot for backup or cloning purposes[1].

## 10. Delete a VM instance and its boot disk
**Question:** Delete the VM `test-vm-1` and ensure its boot disk is also deleted.

**Answer:**
```bash
gcloud compute instances delete test-vm-1 --zone=us-central1-a --delete-disks=boot
```
This deletes the instance and its associated boot disk to avoid orphaned resources.

---
