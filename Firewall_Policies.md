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
gcloud compute network-firewall-policies rules create 1000 \
  --action=allow \
  --description="Allow HTTP/HTTPS from anywhere" \
  --layer4-configs=tcp:80,tcp:443 \
  --firewall-policy=org-wide-firewall-policy \
  --src-ip-ranges=0.0.0.0/0 \
  --global-firewall-policy \
  --enable-logging
```

- **Deny SSH from All Except a Specific Subnet:**

```sh
gcloud compute network-firewall-policies rules create 900 \
  --action=deny \
  --description="Deny SSH except from 192.168.1.0/24" \
  --layer4-configs=tcp:22 \
  --firewall-policy=org-wide-firewall-policy \
  --src-ip-ranges=0.0.0.0/0 \
  --global-firewall-policy \
  --enable-logging \
  --target-ip-ranges=192.168.1.0/24
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

| Purpose                        | Command Example                                                                                           |
|---------------------------------|----------------------------------------------------------------------------------------------------------|
| Create firewall policy          | `gcloud compute network-firewall-policies create org-wide-firewall-policy --description="..." --global`  |
| Add rule to policy              | `gcloud compute network-firewall-policies rules create ...`                                               |
| Associate policy to network     | `gcloud compute network-firewall-policies associations create --firewall-policy=... --network=...`       |
| View policy                     | `gcloud compute network-firewall-policies describe org-wide-firewall-policy --global`                    |

