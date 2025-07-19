# GCP Load Balancer: Cloud Run, MIG, Unmanaged Groups, and Cloud Buckets

## Overview

A **Google Cloud Load Balancer** allows you to distribute traffic across multiple compute resources, like Cloud Run services, Managed Instance Groups (MIG), Unmanaged Instance Groups, or Cloud Storage Buckets. Its workflow generally involves:
- **User requests** → **Load Balancer** → **Backend (Cloud Run/MIG/Unmig/Bucket)**
- The load balancer regularly checks the health of backends to ensure only healthy services handle traffic.

## 1. Components Explained

### 1.1 Cloud Run as Backend
- **Cloud Run** runs containers in a serverless way.
- Needs a load balancer for custom domains, advanced routing, and secure HTTPS.

**Commands:**
```sh
gcloud run deploy my-service --image=gcr.io/project/image --platform=managed --region=us-central1
gcloud compute network-endpoint-groups create my-neg \
  --region=us-central1 \
  --network-endpoint-type=serverless \
  --cloud-run-service=my-service
```

### 1.2 Managed Instance Group (MIG)
- **MIG**: Group of identical VMs managed by instance templates.
- Supports auto-healing, autoscaling, and easy load balancer integration.

**Commands:**
```sh
gcloud compute instance-templates create my-template \
  --machine-type=e2-medium --image-family=debian-11 --image-project=debian-cloud

gcloud compute instance-groups managed create my-mig \
  --base-instance-name=my-instance --template=my-template --size=2 --zone=us-central1-a
```

### 1.3 Unmanaged Instance Group (UnMIG)
- **Unmanaged Groups**: Group of ANY VMs (manual management).
- Basic load balancing (no auto-healing/scaling).

**Commands:**
```sh
gcloud compute instance-groups unmanaged create my-unmig --zone=us-central1-a
gcloud compute instance-groups unmanaged add-instances my-unmig \
  --instances=vm1,vm2 --zone=us-central1-a
```

### 1.4 Google Cloud Storage Bucket as Backend
- Use for serving static files (images, JS, CSS, PDFs, etc.).

**Commands:**
```sh
gsutil mb gs://my-unique-bucket/
```

## 2. Adding Backends to the Load Balancer

**Backend Service Creation:**
```sh
gcloud compute backend-services create my-backend \
  --protocol=HTTP --port-name=http --global
```

**Add MIG to Backend:**
```sh
gcloud compute backend-services add-backend my-backend \
  --instance-group=my-mig \
  --instance-group-zone=us-central1-a --global
```

**Add Unmanaged Group to Backend:**
```sh
gcloud compute backend-services add-backend my-backend \
  --instance-group=my-unmig \
  --instance-group-zone=us-central1-a --global
```

**Add Cloud Run NEG Backend:**
```sh
gcloud compute backend-services add-backend my-backend \
  --network-endpoint-group=my-neg \
  --network-endpoint-group-region=us-central1 --global
```

**Add Storage Bucket as Static Backend:**
- Buckets are added through the console as "backend buckets".
- To upload files:
  ```sh
  gsutil cp localfile gs://my-unique-bucket/
  ```

## 3. Health Checks

Health checks ensure only healthy (working) backends receive traffic.

**Command:**
```sh
gcloud compute health-checks create http hc-http-lb --port 80
```

**Parameters (simplified):**
- `--check-interval` (how often to check, e.g., 10s)
- `--healthy-threshold` (number of successes before marked healthy)
- `--unhealthy-threshold` (number of failures before marked unhealthy)

**Example:**
```sh
gcloud compute health-checks create http hc-http-lb \
  --port 80 --check-interval 10s --healthy-threshold 2 --unhealthy-threshold 3
```

**Associate Health Check with Backend:**
```sh
gcloud compute backend-services update my-backend \
  --health-checks=hc-http-lb --global
```

## 4. Workflow with Flowchart (Text-Based)

Below is a simplified representation of the flow across components:

```
            +---------+
   Users -->| Load    |
            |Balancer |      
            +----+----+
                 |
         +-------+-------+----------+-----------+
         |               |          |           |
   [Cloud Run]    [MIG/VMs]    [UnMIG VMs]  [Bucket]
         |               |          |           |
   <-----+----------Health Check----+-----------+
```

**Legend:**
- **Arrow from LoadBalancer to Backend**: User request routed to available backend.
- **Arrow from Health Check to Backend**: LB periodically probes backend health before sending traffic.

## 5. Typical Setup Sequence

1. **Deploy backend (Cloud Run/MIG/VMs/Bucket)**
2. **Create health check**
3. **Create backend service and attach health check**
4. **Add backend (NEG/MIG/UnMIG/Bucket) to backend service**
5. **Set up URL map, target proxy, and forwarding rule**
6. **Test (LB only sends requests to healthy backends)**

## 6. Table: Key GCP Load Balancing Elements

| Backend Type           | Autoscaling | Autoheal | Load Balancing | Suitable For           | Health Check |
|------------------------|-------------|----------|----------------|------------------------|--------------|
| Cloud Run              | Yes         | Yes      | Yes            | APIs, web services     | Yes          |
| Managed Instance Group | Yes         | Yes      | Yes            | VMs, legacy software   | Yes          |
| Unmanaged Instance Grp | No          | No       | Yes            | Custom VMs, test setup | Yes          |
| Cloud Bucket           | N/A         | N/A      | Yes            | Static files           | N/A          |

**Note:** Adjust region, backend, and resource names to your environment when running commands. Managers should ensure firewall allows health check IPs, and set proper IAM permissions.

