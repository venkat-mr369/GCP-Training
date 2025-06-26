## gcloud Commands for Network Firewall Policies

To implement the example described above using Google Cloud's gcloud CLI, you'll use the `gcloud compute network-firewall-policies` and related subcommands. Below are the steps and example commands:

**1. Create a Global Network Firewall Policy**

```sh
gcloud compute network-firewall-policies create org-wide-firewall-policy \
  --description="Organization-wide firewall policy" \
  --global
```
This command creates a global firewall policy named `org-wide-firewall-policy` with a description[4].

**2. Add Rules to the Firewall Policy**

- **Allow HTTP/HTTPS Traffic from Anywhere:**

```sh
gcloud compute network-firewall-policies rules create 900 \
  --action=allow \
  --description="Allow SSH from 192.168.1.0/24" \
  --layer4-configs=tcp:22 \
  --firewall-policy=org-wide-firewall-policy \
  --src-ip-ranges=192.168.1.0/24 \
  --global-firewall-policy \
  --enable-logging
```

- **Deny SSH from All Except a Specific Subnet:**

```sh
gcloud compute network-firewall-policies rules create 1000 \
  --action=deny \
  --description="Deny SSH from all other sources" \
  --layer4-configs=tcp:22 \
  --firewall-policy=org-wide-firewall-policy \
  --src-ip-ranges=0.0.0.0/0 \
  --global-firewall-policy \
  --enable-logging
```
*(Adjust `--target-ip-ranges` as needed based on your policy logic.)*

**3. Associate the Firewall Policy with a VPC Network**

```sh
gcloud compute network-firewall-policies associations create \
  --firewall-policy=org-wide-firewall-policy \
  --network=YOUR-VPC-NETWORK-NAME \
  --global-firewall-policy
```
Replace `YOUR-VPC-NETWORK-NAME` with the actual name of your VPC network[4].

**4. View the Policy and Rules**

```sh
gcloud compute network-firewall-policies describe org-wide-firewall-policy --global
```
This will display the rules and details of your firewall policy[4].

**Summary Table: Key gcloud Commands**

| Purpose                         | Command Example                                                                                          |
|---------------------------------|----------------------------------------------------------------------------------------------------------|
| Create firewall policy          | `gcloud compute network-firewall-policies create org-wide-firewall-policy --description="..." --global`  |
| Add rule to policy              | `gcloud compute network-firewall-policies rules create ...`                                              |
| Associate policy to network     | `gcloud compute network-firewall-policies associations create --firewall-policy=... --network=...`       |
| View policy                     | `gcloud compute network-firewall-policies describe org-wide-firewall-policy --global`                    |

**Summary Table**

| Priority | Action | Source IP Range     | Description                      |
|----------|--------|---------------------|----------------------------------|
| 900      | allow  | 192.168.1.0/24      | Allow SSH from trusted subnet    |
| 1000     | deny   | 0.0.0.0/0           | Deny SSH from all other sources  |



