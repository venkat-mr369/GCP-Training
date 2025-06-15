**GCP regions and zones** lab work

---

### 1. How to list all available regions in GCP?
**Answer:** Use the command to list all regions.  
**Command:**  
```bash
gcloud compute regions list
```
**Explanation:** This lists all available regions along with their quotas and current status (e.g., UP or DOWN)[1].

---

### 2. How to list all available zones in GCP?
**Answer:** Use the command to list all zones.  
**Command:**  
```bash
gcloud compute zones list
```
**Explanation:** This lists all zones available in your project with their status and associated region[1].

---

### 3. How to get detailed information about a specific region?
**Answer:** Describe the region to get detailed info.  
**Command:**  
```bash
gcloud compute regions describe REGION_NAME
```
**Explanation:** Replace `REGION_NAME` with the region you want details about. This shows quotas, status, and zones within the region[1].

---

### 4. How to get detailed information about a specific zone?
**Answer:** Describe the zone to get detailed info.  
**Command:**  
```bash
gcloud compute zones describe ZONE_NAME
```
**Explanation:** Replace `ZONE_NAME` with the zone you want to inspect. This shows the zone’s status and its parent region[1].

---

### 5. How to set a default region for your gcloud configuration?
**Answer:** Set the default region to avoid specifying it every time.  
**Command:**  
```bash
gcloud config set compute/region REGION_NAME
```
**Explanation:** This configures your gcloud CLI to use the specified region by default.

---

### 6. How to set a default zone for your gcloud configuration?
**Answer:** Set the default zone to avoid specifying it every time.  
**Command:**  
```bash
gcloud config set compute/zone ZONE_NAME
```
**Explanation:** This sets the default zone for your gcloud commands.

---

### 7. How to create a VM instance in a specific zone?
**Answer:** Use the `--zone` flag to specify the zone during VM creation.  
**Command:**  
```bash
gcloud compute instances create INSTANCE_NAME --zone=ZONE_NAME --machine-type=n1-standard-1 --image-family=debian-10 --image-project=debian-cloud
```
**Explanation:** This creates a VM in the specified zone with the chosen machine type and image.

---

### 8. How to move a VM instance to a different zone?
**Answer:** Create an image of the VM’s disk and launch a new VM in the target zone using that image.  
**Command:**  
```bash
gcloud compute images create IMAGE_NAME --source-disk=DISK_NAME --source-disk-zone=SOURCE_ZONE
gcloud compute instances create NEW_INSTANCE_NAME --zone=TARGET_ZONE --image=IMAGE_NAME
```
**Explanation:** VMs cannot be moved directly between zones. Creating an image and launching a new VM in the target zone is the recommended workaround.

---

### 9. How to check the current default region and zone in your gcloud config?
**Answer:** List the current gcloud configuration.  
**Command:**  
```bash
gcloud config list
```
**Explanation:** Displays the current default region, zone, and other configurations.

---

### 10. How to find zones that support a specific machine type?
**Answer:** List machine types filtered by name to see supported zones.  
**Command:**  
```bash
gcloud compute machine-types list --filter='name=TYPE_NAME'
```
**Explanation:** Replace `TYPE_NAME` with the machine type you want to check. This shows zones where that machine type is available[2].

---

### 11. How to find zones that support a specific GPU type?
**Answer:** List accelerator types filtered by GPU model.  
**Command:**  
```bash
gcloud compute accelerator-types list --filter='GPU_TYPE'
```
**Explanation:** Replace `GPU_TYPE` with the GPU model name (e.g., `nvidia-tesla-t4`). This shows zones offering that GPU[2].

---

### 12. How to set project-wide default region and zone metadata?
**Answer:** Add metadata to the project to specify default region and zone.  
**Command:**  
```bash
gcloud compute project-info add-metadata --metadata google-compute-default-region=REGION,google-compute-default-zone=ZONE
```
**Explanation:** This sets default region and zone metadata for the entire project.

---

### 13. How to list all regions with their quotas and status?
**Answer:** Use the regions list command.  
**Command:**  
```bash
gcloud compute regions list
```
**Explanation:** Displays quotas like CPUs, disk space, addresses, and the status of each region[1].

---

### 14. How to list all zones with their status and region?
**Answer:** Use the zones list command.  
**Command:**  
```bash
gcloud compute zones list
```
**Explanation:** Shows the status and region affiliation of each zone[1].

---

### 15. How to delete a default region or zone setting from gcloud config?
**Answer:** Unset the default region or zone in your gcloud config.  
**Command:**  
```bash
gcloud config unset compute/region
gcloud config unset compute/zone
```


