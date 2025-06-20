---

## 📄 **Explanation of OS Policies (GCP OSConfig)**

---

### What is OS Policies?

* **OS Policies** in GCP allow you to **enforce compliance and configurations** on your VM instances.
* It works with **OS Config Agent** installed on VMs.
* You can define **rules and configurations** for:

  * Software installation
  * Security patches
  * File integrity
  * System settings
* You can define policies at:

  * **Zonal level**
  * **Global level**

---

### UI Tabs in Screenshot:

| Tab                      | Meaning                                          |
| ------------------------ | ------------------------------------------------ |
| **PROJECTS**             | Shows summary of OS policy compliance by project |
| **OS POLICIES**          | Define individual OS policy objects (YAML/JSON)  |
| **VM INSTANCES**         | Lists individual VMs and their OS policy status  |
| **ZONAL ASSIGNMENTS**    | Assign policies to VMs in specific zones         |
| **GLOBAL ORCHESTRATORS** | Manage global orchestration of policies          |

---

### Fields in UI Table:

| Field               | Meaning                                      |
| ------------------- | -------------------------------------------- |
| **Project**         | GCP project name                             |
| **Total VMs**       | Total VM instances                           |
| **Monitored VMs**   | VMs with OSConfig agent reporting            |
| **VMs with policy** | VMs assigned an OS policy                    |
| **Compliant**       | VMs compliant with policy                    |
| **Non-compliant**   | VMs that failed policy check                 |
| **Unknown**         | VMs without OSConfig agent or unknown status |

---

## 🚀 **Gcloud Commands for OS Policies**

---

### 1️⃣ **List OS policy assignments**

```bash
gcloud compute os-config os-policy-assignments list \
    --project=approject-2025
```

---

### 2️⃣ **Create a zonal OS policy assignment**

```bash
gcloud compute os-config os-policy-assignments create my-zonal-assignment \
    --project=approject-2025 \
    --os-policy path_to_policy.yaml \
    --location=us-east1-b \
    --instance-filter-inventory-os-short-name debian \
    --instance-filter-inventory-os-version-id 10
```

✅ `path_to_policy.yaml`: Your OS policy file (YAML or JSON)

---

### 3️⃣ **Describe OS policy assignment**

```bash
gcloud compute os-config os-policy-assignments describe my-zonal-assignment \
    --project=approject-2025 \
    --location=us-east1-b
```

---

### 4️⃣ **Delete OS policy assignment**

```bash
gcloud compute os-config os-policy-assignments delete my-zonal-assignment \
    --project=approject-2025 \
    --location=us-east1-b
```

---

### 5️⃣ **List OS policies**

```bash
gcloud compute os-config os-policies list \
    --project=approject-2025
```

---

### 6️⃣ **List VM compliance status**

```bash
gcloud compute os-config guest-policies list-effective \
    --project=approject-2025 \
    --zone=us-east1-b \
    --instance=vm-instance-name
```

---

## 📌 **How it works (flow)**:

1. Create OS policy (YAML/JSON)
2. Create policy assignment (zonal or global)
3. OSConfig agent on VM checks policy
4. Reports compliance status
5. Admin views status in Console or via gcloud

---
Here is a **sample OS Policy YAML** — you can use this in your course document:

---

### 📄 Sample OS Policy YAML — Install `nginx`

```yaml
id: install-nginx-policy
mode: ENFORCEMENT
resourceGroups:
- resources:
  - id: install-nginx
    pkg:
      desiredState: INSTALLED
      apt:
        name: nginx
```

---

### 📄 Sample OS Policy YAML — Ensure File Exists with Contents

```yaml
id: file-content-check
mode: ENFORCEMENT
resourceGroups:
- resources:
  - id: check-welcome-file
    file:
      path: /var/www/html/index.html
      state: PRESENT
      contents: |
        Welcome to my VM!
```

---

### 📄 Sample OS Policy YAML — Set Sysctl Parameter

```yaml
id: set-sysctl-param
mode: ENFORCEMENT
resourceGroups:
- resources:
  - id: vm-swappiness
    sysctl:
      name: vm.swappiness
      value: '10'
```

---

### How to Apply this Policy:

```bash
gcloud compute os-config os-policy-assignments create nginx-assignment \
    --project=approject-2025 \
    --os-policy=install-nginx.yaml \
    --location=us-east1-b \
    --instance-filter-inventory-os-short-name debian
```

---


