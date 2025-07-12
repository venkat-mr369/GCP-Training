dev-app@opportune-baton-461416-m3.iam.gserviceaccount.com# GCP IAM Scenario-Based Interview Questions with Solutions (gcloud)

Below are practical, scenario-based Google Cloud IAM interview questions, using **venkat@mycompany.com** as the user and **dev-app@opportune-baton-461416-m3.iam.gserviceaccount.com** as the service account. Each scenario includes concise solutions and relevant `gcloud` commands.

## 1. Grant a User Viewer Access to a Project

**Scenario:**  
Give **venkat@mycompany.com** read-only access to all resources in the project `my-gcp-project`.

**Solution:**
```bash
gcloud projects add-iam-policy-binding my-gcp-project \
  --member="user:venkat@mycompany.com" \
  --role="roles/viewer"
```

## 2. List All IAM Roles Assigned to a Service Account

**Scenario:**  
Audit which roles are assigned to the service account **dev-app@opportune-baton-461416-m3.iam.gserviceaccount.com** in `my-gcp-project`.

**Solution:**
```bash
gcloud projects get-iam-policy my-gcp-project \
  --flatten="bindings[].members" \
  --format='table(bindings.role)' \
  --filter="bindings.members:serviceAccount:dev-app@opportune-baton-461416-m3.iam.gserviceaccount.com"
```

## 3. Create and Assign a Custom Role

**Scenario:**  
Create a custom role in `my-gcp-project` that allows only listing and reading storage buckets. Assign it to **venkat@mycompany.com**.

**Solution:**

1. **Create a YAML file (`custom_role.yaml`):**
   ```yaml
   title: "Storage Bucket Reader"
   description: "Can list and get storage buckets"
   stage: "GA"
   includedPermissions:
     - storage.buckets.get
     - storage.buckets.list
   ```

2. **Create the custom role:**
   ```bash
   gcloud iam roles create storageBucketReader \
     --project=my-gcp-project \
     --file=custom_role.yaml
   ```

3. **Assign the role:**
   ```bash
   gcloud projects add-iam-policy-binding my-gcp-project \
     --member="user:venkat@mycompany.com" \
     --role="projects/my-gcp-project/roles/storageBucketReader"
   ```

## 4. Grant Temporary Access to a Resource

**Scenario:**  
A contractor (**venkat@mycompany.com**) needs `roles/storage.objectViewer` access to a bucket for one week.

**Solution:**
```bash
gcloud projects add-iam-policy-binding my-gcp-project \
  --member="user:venkat@mycompany.com" \
  --role="roles/storage.objectViewer"
```
To remove after a week:
```bash
gcloud projects remove-iam-policy-binding my-gcp-project \
  --member="user:venkat@mycompany.com" \
  --role="roles/storage.objectViewer"
```

## 5. Audit IAM Changes in a Project

**Scenario:**  
Find out who changed IAM permissions in your project last week.

**Solution:**
```bash
gcloud logging read \
  'resource.type="project" protoPayload.methodName:"SetIamPolicy"' \
  --project=my-gcp-project \
  --limit=10 \
  --format="table(timestamp, protoPayload.authenticationInfo.principalEmail, protoPayload.methodName)"
```

## 6. Remove a User’s Access from a Project

**Scenario:**  
An employee (**venkat@mycompany.com**) has left. Remove all their IAM roles from `my-gcp-project`.

**Solution:**
First, identify all roles:
```bash
gcloud projects get-iam-policy my-gcp-project \
  --flatten="bindings[].members" \
  --format='table(bindings.role)' \
  --filter="bindings.members:user:venkat@mycompany.com"
```
Then, for each role:
```bash
gcloud projects remove-iam-policy-binding my-gcp-project \
  --member="user:venkat@mycompany.com" \
  --role="roles/roleName"
```

## 7. Assign a Role to a Group

**Scenario:**  
All members of the group `devs@mycompany.com` should have the `roles/editor` role in the project.

**Solution:**
```bash
gcloud projects add-iam-policy-binding my-gcp-project \
  --member="group:devs@mycompany.com" \
  --role="roles/editor"
```

## 8. Restrict Access Using IAM Conditions

**Scenario:**  
Grant **venkat@mycompany.com** access to a resource only if connecting from a specific IP range.

**Solution:**
```bash
gcloud projects add-iam-policy-binding my-gcp-project \
  --member="user:venkat@mycompany.com" \
  --role="roles/viewer" \
  --condition="expression=request.remoteIp.startsWith('203.0.113.'),title=AllowFromCorpNetwork,description=Allow only from corporate IP"
```

## 9. List All Members with a Specific Role

**Scenario:**  
See who has the `roles/owner` role in your project.

**Solution:**
```bash
gcloud projects get-iam-policy my-gcp-project \
  --flatten="bindings[].members" \
  --format='table(bindings.members)' \
  --filter="bindings.role:roles/owner"
```

## 10. Assign a Role to a Service Account

**Scenario:**  
A Compute Engine VM needs to write logs to Stackdriver. Assign `roles/logging.logWriter` to the service account **dev-app@opportune-baton-461416-m3.iam.gserviceaccount.com**.

**Solution:**
```bash
gcloud projects add-iam-policy-binding my-gcp-project \
  --member="serviceAccount:dev-app@opportune-baton-461416-m3.iam.gserviceaccount.com" \
  --role="roles/logging.logWriter"
```

**Tip:**  
For more details on `gcloud` IAM commands, refer to the official gcloud CLI documentation.

