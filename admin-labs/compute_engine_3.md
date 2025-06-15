To troubleshoot common issues with GCP Compute Engine VM instances effectively using `gcloud` CLI techniques

## 1. Investigate VM Freezing or Unresponsiveness
- **Check VM logs for errors or kernel panics:**  
  Use the serial port output to capture boot and runtime logs which often reveal kernel panics or freezes:  
  ```bash
  gcloud compute connect-to-serial-port VM_NAME --zone=ZONE
  ```
  This lets you view low-level system messages for clues about freezes or crashes[1][2].

- **Check disk space and memory usage:**  
  SSH into the VM (if possible) to verify disk space and memory:  
  ```bash
  gcloud compute ssh VM_NAME --zone=ZONE
  df -h
  free -m
  ```
  Low disk space or memory exhaustion can cause freezing[1].

## 2. Diagnose Unexpected Shutdowns or Reboots
- **Query Cloud Audit Logs for shutdown/reboot events:**  
  Identify what triggered the reboot or shutdown (user, system, or billing issues):  
  ```bash
  gcloud logging read 'resource.type="gce_instance" AND resource.labels.instance_id="INSTANCE_ID" AND (logName=("logs/cloudaudit.googleapis.com/system_event") OR logName=("logs/cloudaudit.googleapis.com/activity"))' --limit=10 --format=json
  ```
  Or use the Logs Explorer in the Cloud Console to search for system events related to your VM[3].

- **Check for Shared VPC billing or network freezes:**  
  If multiple VMs shut down simultaneously, verify Shared VPC billing status and network interface state:  
  ```bash
  gcloud compute instances describe VM_NAME --zone=ZONE --format="flattened(networkInterfaces[].network)"
  ```
  Ensure billing is enabled on the host project to prevent mass shutdowns[3].

## 3. Troubleshoot VM Startup Failures
- **Check if a snapshot operation is blocking the VM start:**  
  If you see errors about snapshots in progress, wait for them to complete before starting the VM again[4].

- **Validate disk file system integrity:**  
  Attach the boot disk to a debug VM as a non-boot disk and check the file system:  
  ```bash
  gcloud compute instances attach-disk debug-instance --disk=DISK_NAME --zone=ZONE
  gcloud compute ssh debug-instance --zone=ZONE
  sudo fsck /dev/sdb1  # assuming /dev/sdb1 is the attached disk partition
  ```
  Repair any file system corruption found[4].

## 4. Use Serial Console for Debugging
- Enable and connect to the serial console to troubleshoot boot issues or when SSH is unavailable:  
  ```bash
  gcloud compute instances add-metadata VM_NAME --metadata serial-port-enable=1 --zone=ZONE
  gcloud compute connect-to-serial-port VM_NAME --zone=ZONE
  ```
  This is useful to see kernel messages or interact with the VM during boot[2][4].

## 5. Check IAM Permissions and Quotas
- **Verify IAM roles if permission errors occur:**  
  List current account and roles:  
  ```bash
  gcloud auth list
  gcloud projects get-iam-policy PROJECT_ID
  ```
  Adjust IAM roles to ensure the user/service account has necessary permissions[5].

- **Check resource quotas to avoid operation failures:**  
  ```bash
  gcloud compute regions describe REGION
  ```
  Look for quota limits and usage to identify if you hit limits causing failures[5].

## 6. Monitor Logs and Metrics Proactively
- Use Logs Explorer to filter VM logs by instance and severity:  
  ```bash
  gcloud logging read 'resource.type="gce_instance" AND resource.labels.instance_id="INSTANCE_ID"' --limit=50 --format="table(timestamp, severity, textPayload)"
  ```
  This helps identify warnings or errors in syslog or application logs[6].

- Build dashboards in Cloud Monitoring for VM uptime, CPU, memory, and disk metrics to catch issues early[3][6].

---

### Summary of Key `gcloud` Commands for Troubleshooting

| Purpose                               | Command Example                                                                                  |
|-------------------------------------|------------------------------------------------------------------------------------------------|
| Connect to serial console            | `gcloud compute connect-to-serial-port VM_NAME --zone=ZONE`                                    |
| SSH into VM                         | `gcloud compute ssh VM_NAME --zone=ZONE`                                                       |
| Check VM logs via Logging API       | `gcloud logging read 'resource.type="gce_instance" AND resource.labels.instance_id="ID"'`       |
| Describe VM network info             | `gcloud compute instances describe VM_NAME --zone=ZONE --format="flattened(networkInterfaces[].network)"` |
| Attach disk for debugging            | `gcloud compute instances attach-disk debug-instance --disk=DISK_NAME --zone=ZONE`              |
| Check IAM roles                     | `gcloud projects get-iam-policy PROJECT_ID`                                                    |
| Check quotas                       | `gcloud compute regions describe REGION`                                                       |
| Stop/Start VM                      | `gcloud compute instances stop VM_NAME --zone=ZONE` / `gcloud compute instances start VM_NAME --zone=ZONE` |

***URLS* 
[1] https://www.googlecloudcommunity.com/gc/Infrastructure-Compute-Storage/VM-instance-keeps-freezing/m-p/806571
[2] https://blog.nashtechglobal.com/creating-a-virtual-machine-in-google-cloud-console-cli/
[3] https://cloud.google.com/compute/docs/troubleshooting/troubleshooting-reboots
[4] https://cloud.google.com/compute/docs/troubleshooting/vm-startup
[5] https://www.linkedin.com/pulse/understanding-resolving-common-issues-google-cloud-gcp-panpatil-zcw8f
[6] https://www.cloudskillsboost.google/focuses/19183?parent=catalog
[7] https://cloud.google.com/compute/docs/troubleshooting/troubleshooting-performance
[8] https://status.cloud.google.com/summary
[9] https://www.youtube.com/watch?v=g2Il8cxNv18
[10] https://www.googlecloudcommunity.com/gc/Infrastructure-Compute-Storage/Problems-with-Edge-Server-and-Google-Cloud-Compute-Engine-Any/m-p/784371
[11] https://cloud.google.com/sdk/gcloud
[12] https://www.cloudskillsboost.google/focuses/563?parent=catalog
[13] https://www.youtube.com/watch?v=j274vq9a2Rs
[14] https://www.turbogeek.co.uk/gcloud-command-lists/
[15] https://status.cloud.google.com/incidents/SXRPpPwx2RZ5VHjTwFLx
[16] https://developer.harness.io/docs/chaos-engineering/use-harness-ce/chaos-faults/gcp/gcp-vm-instance-stop/
[17] https://www.googlecloudcommunity.com/gc/Infrastructure-Compute-Storage/VM-instance-is-currently-unavailable-not-acceptable-architecture/m-p/791098
[18] https://stackoverflow.com/questions/78225316/cloud-shell-editor-how-to-connect-and-debug-through-a-vm-instance
[19] https://www.youtube.com/watch?v=RO78WYoAr3k
