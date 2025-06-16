**advanced and troubleshooting-based practical tasks** on **GCP Identity and Access Management (IAM)**

* **Real-world scenario (question)**
* **Explanation**
* **`gcloud` command(s)**
* **Troubleshooting insights**

These are tailored for experienced users and interview/training prep.

---

## ✅ 1. **Fix: Permission Denied for `gcloud compute` Commands**

**❓Question:**
You're getting: `PERMISSION_DENIED: Request had insufficient authentication scopes.` What’s wrong?

**📘 Explanation:**
The service account or user lacks proper roles or OAuth scopes.

**✅ Fix:**
Check account and role:

```bash
gcloud auth list
gcloud projects get-iam-policy PROJECT_ID
```

Check scopes if using GCE:

```bash
gcloud compute instances describe INSTANCE_NAME --zone=ZONE
```

**✅ Solution:**
Assign `roles/compute.admin` or recreate instance with full scopes:

```bash
gcloud compute instances create vm1 \
  --zone=ZONE \
  --scopes=https://www.googleapis.com/auth/cloud-platform
```

---

## ✅ 2. **Troubleshoot: IAM Binding Doesn’t Apply**

**❓Question:**
You assign a role, but the user still gets access denied. Why?

**📘 Explanation:**
Might be IAM policy propagation delay or overriding resource-level IAM.

**✅ Command:**

```bash
gcloud projects get-iam-policy PROJECT_ID
```

Check if the same user has explicit deny via org policy (rare) or resource-level restrictions.

---

## ✅ 3. **Detect: Who Changed IAM Policy**

**❓Question:**
You need to know who added a new IAM binding in the project. How?

**📘 Explanation:**
Use Cloud Audit Logs to track IAM modifications.

**✅ Command:**

```bash
gcloud logging read \
  'protoPayload.methodName="SetIamPolicy"' \
  --project=PROJECT_ID \
  --limit=5 \
  --format=json
```

---

## ✅ 4. **Service Account Can’t Access Resource**

**❓Question:**
You created a service account but it gets `403` on BigQuery access.

**📘 Explanation:**
The service account may not have the right role or impersonation is incorrect.

**✅ Fix:**

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="serviceAccount:sa-name@PROJECT_ID.iam.gserviceaccount.com" \
  --role="roles/bigquery.dataViewer"
```

Validate permissions:

```bash
gcloud projects get-iam-policy PROJECT_ID
```

---

## ✅ 5. **Fix: Invalid IAM Member Format**

**❓Question:**
You get: `Invalid value for field 'resource.bindings.member': 'john@example.com'`

**📘 Explanation:**
You’re missing the member type prefix.

**✅ Correct Format:**

```bash
user:john@example.com
group:devs@example.com
serviceAccount:account@project.iam.gserviceaccount.com
```

---

## ✅ 6. **IAM Role Assigned but No Effect**

**❓Question:**
A user has `roles/storage.admin` but can’t upload to a bucket. Why?

**📘 Explanation:**
Check if the role is assigned at project level, but the bucket has its own IAM.

**✅ Fix:**

```bash
gcloud storage buckets get-iam-policy gs://bucket-name
```

Add bucket-level role if needed:

```bash
gcloud storage buckets add-iam-policy-binding gs://bucket-name \
  --member="user:john@example.com" \
  --role="roles/storage.objectAdmin"
```

---

## ✅ 7. **Troubleshoot: Service Account Key Expires or is Deleted**

**❓Question:**
Your app fails authentication; key is missing or expired.

**📘 Explanation:**
Check if keys exist or were rotated/deleted.

**✅ Commands:**

```bash
gcloud iam service-accounts keys list \
  --iam-account=SA_NAME@PROJECT_ID.iam.gserviceaccount.com
```

**✅ Solution:**

Regenerate key:

```bash
gcloud iam service-accounts keys create key.json \
  --iam-account=SA_NAME@PROJECT_ID.iam.gserviceaccount.com
```

---

## ✅ 8. **Role Inheritance Not Working Across Folder/Org**

**❓Question:**
You gave a user a role at the folder level but it doesn’t reflect in the project.

**📘 Explanation:**
Check the resource hierarchy and ensure inheritance isn’t blocked.

**✅ Commands:**

```bash
gcloud resource-manager folders get-iam-policy FOLDER_ID
gcloud projects get-ancestors PROJECT_ID
```

---

## ✅ 9. **Fix: Missing Required Permission for Deployment Manager**

**❓Question:**
Deployment Manager fails with permission errors.

**📘 Explanation:**
Needs broad permissions like `roles/editor` or custom roles with create/update permissions.

**✅ Fix:**

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="user:you@example.com" \
  --role="roles/editor"
```

---

## ✅ 10. **Service Account Can't Be Used for Impersonation**

**❓Question:**
You get: `Caller is not authorized to impersonate the service account.`

**📘 Explanation:**
User needs `roles/iam.serviceAccountTokenCreator` on the service account.

**✅ Command:**

```bash
gcloud iam service-accounts add-iam-policy-binding SA_NAME@PROJECT_ID.iam.gserviceaccount.com \
  --member="user:you@example.com" \
  --role="roles/iam.serviceAccountTokenCreator"
```

---

## ✅ 11. **Limit IAM Role to Specific Resources (Conditional IAM)**

**❓Question:**
How do you restrict a role to only apply when accessing a specific bucket?

**📘 Explanation:**
Use **IAM Conditions**.

**✅ Command:**

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="user:john@example.com" \
  --role="roles/storage.objectViewer" \
  --condition='expression=request.resource.name.startsWith("projects/_/buckets/my-bucket"),title=LimitToBucket,description=Restrict access to my-bucket'
```

---

## ✅ 12. **Revoke All Roles from a User**

**❓Question:**
How to cleanly revoke all IAM roles from a user?

**📘 Explanation:**
List roles and remove one by one.

**✅ Automation Example:**

```bash
gcloud projects get-iam-policy PROJECT_ID --format=json > policy.json

# Use jq or a script to identify all bindings with the user and remove
```

Or remove manually:

```bash
gcloud projects remove-iam-policy-binding PROJECT_ID \
  --member="user:john@example.com" \
  --role="roles/editor"
```

---

## ✅ 13. **Restrict Access Using Organization Policy**

**❓Question:**
How do you restrict service account key creation org-wide?

**📘 Explanation:**
Use `constraints/iam.disableServiceAccountKeyCreation`

**✅ Command:**

```bash
gcloud org-policies set-policy sa_key_disable.yaml --project=PROJECT_ID
```

`sa_key_disable.yaml`:

```yaml
constraint: constraints/iam.disableServiceAccountKeyCreation
listPolicy:
  deniedValues: ["true"]
```

---

## ✅ 14. **Detect Overprivileged Service Accounts**

**❓Question:**
How to audit service accounts with admin-level access?

**📘 Explanation:**
Look for bindings with `roles/editor`, `roles/owner`, or custom roles.

**✅ Command:**

```bash
gcloud projects get-iam-policy PROJECT_ID \
  --flatten="bindings[].members" \
  --filter="bindings.members:serviceAccount" \
  --format="table(bindings.role, bindings.members)"
```

---

## ✅ 15. **Simulate IAM Permissions (Test Permissions API)**

**❓Question:**
How to verify what permissions a user or service account has?

**📘 Explanation:**
Use `testIamPermissions` to simulate access.

**✅ Command:**

```bash
gcloud iam service-accounts test-iam-permissions \
  automation-sa@PROJECT_ID.iam.gserviceaccount.com \
  --permissions=bigquery.datasets.get,storage.objects.list
```

---

### ✅ Summary:

| Task                   | Area             |
| ---------------------- | ---------------- |
| Policy audit logs      | Security         |
| Impersonation          | Delegation       |
| Conditional roles      | Fine-grained IAM |
| Role propagation       | Troubleshooting  |
| Resource-level IAM     | Precision access |
| Org policy constraints | Governance       |

---


