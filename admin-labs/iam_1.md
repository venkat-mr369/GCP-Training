Here are **15 practical tasks** on **GCP Identity and Access Management (IAM)**
---

## ✅ **Task 1: View IAM Policy for a Project**

**❓Question:**
How do you list all users and roles assigned in your GCP project?

**📘 Explanation:**
This helps in auditing who has access and what permissions they hold.

**✅ Command:**

```bash
gcloud projects get-iam-policy PROJECT_ID
```

---

## ✅ **Task 2: Add IAM Role to a User**

**❓Question:**
How do you give the user `john@example.com` the `Viewer` role in your project?

**📘 Explanation:**
Viewer role provides read-only access to resources.

**✅ Command:**

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="user:john@example.com" \
  --role="roles/viewer"
```

---

## ✅ **Task 3: Remove IAM Role from a User**

**❓Question:**
How do you remove the `Viewer` role from the same user?

**📘 Explanation:**
Used for access revocation or user offboarding.

**✅ Command:**

```bash
gcloud projects remove-iam-policy-binding PROJECT_ID \
  --member="user:john@example.com" \
  --role="roles/viewer"
```

---

## ✅ **Task 4: Grant Role to a Group**

**❓Question:**
How do you assign the `Storage Admin` role to a Google group?

**📘 Explanation:**
This gives all group members permission to manage Cloud Storage.

**✅ Command:**

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="group:devops-team@example.com" \
  --role="roles/storage.admin"
```

---

## ✅ **Task 5: Create a Custom Role**

**❓Question:**
How do you create a custom role named `CustomViewer` with read access to Compute resources?

**📘 Explanation:**
Custom roles provide fine-grained control tailored to organizational needs.

**✅ Command:**

```bash
gcloud iam roles create CustomViewer \
  --project=PROJECT_ID \
  --title="Custom Viewer" \
  --description="Read-only access to Compute Engine" \
  --permissions=compute.instances.get,compute.instances.list \
  --stage=GA
```

---

## ✅ **Task 6: Bind Custom Role to a User**

**❓Question:**
How do you assign the newly created `CustomViewer` role to a user?

**✅ Command:**

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="user:alice@example.com" \
  --role="projects/PROJECT_ID/roles/CustomViewer"
```

---

## ✅ **Task 7: Create a Service Account**

**❓Question:**
How do you create a service account named `automation-sa`?

**📘 Explanation:**
Service accounts are used by applications or services, not end-users.

**✅ Command:**

```bash
gcloud iam service-accounts create automation-sa \
  --display-name="Automation Service Account"
```

---

## ✅ **Task 8: Grant Role to a Service Account**

**❓Question:**
How do you give `automation-sa` permission to read from BigQuery?

**✅ Command:**

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="serviceAccount:automation-sa@PROJECT_ID.iam.gserviceaccount.com" \
  --role="roles/bigquery.dataViewer"
```

---

## ✅ **Task 9: Generate Service Account Key**

**❓Question:**
How do you generate a key file (JSON) for `automation-sa`?

**📘 Explanation:**
The key is used for programmatic access (e.g., via SDKs or CI tools).

**✅ Command:**

```bash
gcloud iam service-accounts keys create key.json \
  --iam-account=automation-sa@PROJECT_ID.iam.gserviceaccount.com
```

---

## ✅ **Task 10: Impersonate a Service Account**

**❓Question:**
How do you impersonate a service account using `gcloud`?

**📘 Explanation:**
Useful for CI/CD pipelines or delegated access scenarios.

**✅ Command:**

```bash
gcloud auth print-access-token \
  --impersonate-service-account=automation-sa@PROJECT_ID.iam.gserviceaccount.com
```

---

## ✅ **Task 11: Audit IAM Changes**

**❓Question:**
How do you view IAM changes over time for auditing purposes?

**📘 Explanation:**
Enable Cloud Audit Logs and filter logs related to IAM changes.

**✅ Command:**

```bash
gcloud logging read "protoPayload.methodName:('SetIamPolicy')" \
  --project=PROJECT_ID \
  --limit=10 \
  --format=json
```

---

## ✅ **Task 12: List All Roles in a Project**

**❓Question:**
How do you list all predefined and custom roles available in your project?

**✅ Command:**

```bash
gcloud iam roles list --project=PROJECT_ID
```

---

## ✅ **Task 13: Disable/Delete a Service Account**

**❓Question:**
How do you disable or delete an unused service account?

**📘 Explanation:**
Reduces security risks by deactivating unused identities.

**✅ Disable Command:**

```bash
gcloud iam service-accounts disable automation-sa@PROJECT_ID.iam.gserviceaccount.com
```

**✅ Delete Command:**

```bash
gcloud iam service-accounts delete automation-sa@PROJECT_ID.iam.gserviceaccount.com
```

---

## ✅ **Task 14: Grant Role at Folder Level**

**❓Question:**
How do you grant a role to a user at a folder level?

**📘 Explanation:**
Folder-level roles apply to all projects under the folder.

**✅ Command:**

```bash
gcloud resource-manager folders add-iam-policy-binding FOLDER_ID \
  --member="user:manager@example.com" \
  --role="roles/editor"
```

---

## ✅ **Task 15: View What Permissions a User Has**

**❓Question:**
How do you test which permissions a user has on a project?

**📘 Explanation:**
Use IAM policy simulation to verify permissions before assigning.

**✅ Command:**

```bash
gcloud iam list-testable-permissions //cloudresourcemanager.googleapis.com/projects/PROJECT_ID
```

---
