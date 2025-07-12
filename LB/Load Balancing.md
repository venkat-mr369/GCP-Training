## Google Cloud Load Balancing: Simple Guide with Examples

### What Is Load Balancing?

**Load balancing** means spreading user traffic across multiple servers or resources. This helps your applications handle more users, avoid slowdowns, and stay available even if one server fails.

**Google Cloud Load Balancing** is a fully managed service that automatically distributes traffic for you, using the same technology Google uses for its own services.

### Key Features (in Simple Terms)

- **Single IP Address:** You get one IP for your app, no matter how many servers or regions you use.
- **Automatic Scaling:** Handles sudden spikes in traffic—no setup needed.
- **Global and Regional:** Can balance traffic across the world or just within one region.
- **Supports Many Protocols:** Works for web (HTTP/HTTPS), TCP, UDP, and more.
- **External or Internal:** Can serve users on the internet or just inside your private network.

### Types of Load Balancers

| Type                          | Layer | Used For         | Example Use Case                                |
|-------------------------------|-------|------------------|-------------------------------------------------|
| Application Load Balancer     | 7     | HTTP/HTTPS       | Websites, APIs, web apps                        |
| Proxy Network Load Balancer   | 4     | TCP/SSL          | Databases, custom TCP apps                      |
| Passthrough Network Load Balancer | 4 | TCP/UDP/Other IP | Gaming servers, legacy apps, preserving client IP|

### Example Scenarios

#### 1. Website with Global Users

- Use an **Application Load Balancer** for a website with users worldwide.
- Google routes users to the nearest healthy server, even across continents.

#### 2. Internal Business App

- Use an **Internal Load Balancer** to let only your company’s employees access an app within your VPC (private network).

#### 3. Gaming Server

- Use a **Passthrough Network Load Balancer** to handle UDP traffic and keep the original client IP address.

### Basic Components

- **Frontend:** The IP address and port users connect to.
- **Backend:** The servers or services running your app.
- **Health Check:** Makes sure only healthy servers get traffic.
- **Firewall Rules:** Allow or block traffic as needed.

### Example: Create a Global HTTP(S) Load Balancer

**Step 1: Create a Backend Service**
```sh
gcloud compute backend-services create my-backend-service \
  --protocol=HTTP \
  --port-name=http \
  --global
```

**Step 2: Add Instance Group to Backend**
```sh
gcloud compute backend-services add-backend my-backend-service \
  --instance-group=my-instance-group \
  --instance-group-zone=us-central1-a \
  --global
```

**Step 3: Create a Health Check**
```sh
gcloud compute health-checks create http my-health-check \
  --port 80
```

**Step 4: Create a URL Map**
```sh
gcloud compute url-maps create my-url-map \
  --default-service my-backend-service
```

**Step 5: Create a Target HTTP Proxy**
```sh
gcloud compute target-http-proxies create my-http-proxy \
  --url-map=my-url-map
```

**Step 6: Reserve a Global IP Address**
```sh
gcloud compute addresses create my-lb-ipv4 \
  --ip-version=IPV4 \
  --global
```

**Step 7: Create a Forwarding Rule**
```sh
gcloud compute forwarding-rules create my-http-rule \
  --address=my-lb-ipv4 \
  --global \
  --target-http-proxy=my-http-proxy \
  --ports=80
```

### Example: Create an Internal TCP Load Balancer

**Step 1: Create a Backend Service**
```sh
gcloud compute backend-services create my-internal-backend \
  --protocol=TCP \
  --region=us-central1
```

**Step 2: Add Backend**
```sh
gcloud compute backend-services add-backend my-internal-backend \
  --instance-group=my-instance-group \
  --instance-group-zone=us-central1-a \
  --region=us-central1
```

**Step 3: Create a Health Check**
```sh
gcloud compute health-checks create tcp my-tcp-health-check \
  --port 80
```

**Step 4: Reserve an Internal IP**
```sh
gcloud compute addresses create my-internal-ip \
  --region=us-central1 \
  --subnet=my-subnet
```

**Step 5: Create a Forwarding Rule**
```sh
gcloud compute forwarding-rules create my-internal-tcp-rule \
  --address=my-internal-ip \
  --region=us-central1 \
  --ip-protocol=TCP \
  --ports=80 \
  --backend-service=my-internal-backend \
  --subnet=my-subnet
```

### Choosing the Right Load Balancer

- **For websites and APIs:** Use Application Load Balancer (HTTP/HTTPS).
- **For TCP/SSL apps:** Use Proxy Network Load Balancer.
- **For UDP, gaming, or legacy protocols:** Use Passthrough Network Load Balancer.
- **For internal-only apps:** Use Internal Load Balancer.

### Summary Table

| Need                            | Load Balancer Type               | Quick Command Example                                  |
|----------------------------------|----------------------------------|--------------------------------------------------------|
| Public website                   | Application Load Balancer        | `gcloud compute backend-services create ... --global`  |
| Private app (inside VPC)         | Internal Load Balancer           | `gcloud compute backend-services create ... --region`  |
| TCP/SSL service                  | Proxy Network Load Balancer      | `gcloud compute target-tcp-proxies create ...`         |
| UDP or preserve client IP        | Passthrough Network Load Balancer| `gcloud compute forwarding-rules create ...`           |

Google Cloud Load Balancing is powerful, flexible, and easy to automate with the `gcloud` CLI. You can scale your apps worldwide, keep them reliable.

[1] https://cloud.google.com/load-balancing/docs/load-balancing-overview
