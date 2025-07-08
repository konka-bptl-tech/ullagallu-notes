# ELB (Elastic Load Balancing)
✅ ELB is a **fully managed load balancer** offered by AWS. It handles:
* **Upgrades**
* **Patches**
* **Maintenance**
* **High Availability**
* **Scalability**
* The basic functionality of loadbalancers are distributes incoming traffic to backend targets such as **EC2 instances**, **containers**, **Lambda functions**, or even **ALBs**.
* It performs **health checks** and routes traffic **only to healthy targets**.
* ELB **automatically scales** the load balancer when incoming traffic increases.
* Load balancer acts as a **reverse proxy**, meaning **backend IPs are hidden** — this adds a layer of security.
* It supports **SSL termination**, where SSL is terminated at the load balancer, and traffic from LB to backend can be over **HTTP** (if internal).
---
## Types of ELBs (4):
1. **Classic Load Balancer (CLB)** – *L4 & L7* – (Deprecated for most use cases)
2. **Application Load Balancer (ALB)** – *L7* – (HTTP/HTTPS, modern web apps)
3. **Network Load Balancer (NLB)** – *L4* – (TCP/UDP, high performance)
4. **Gateway Load Balancer (GWLB)** – *L3* – (Used for security appliances, packet filtering)
---
## ALB & Components
* ALB is a **Layer 7** load balancer that works with **HTTP/HTTPS protocols**.
* It serves as the **single point of contact** for clients and improves **application availability**.
* ALB supports **path-based routing** and **host-based routing**:
  * Path-based: `/api/*`, `/images/*`
  * Host-based: `app.example.com`, `api.example.com`
* ALB distributes incoming traffic across **multiple targets** (like EC2, ECS) in **multiple Availability Zones**.
* You configure **listeners** (e.g., on port 80 or 443) to receive and route incoming requests.
* Requests are distributed using the **Round Robin** algorithm to all **healthy targets** in a target group.
---
### **Listeners & Rules**
* A **listener** checks for incoming requests from users on the port and protocol you set (like HTTP on port 80 or HTTPS on port 443).
* **Rules** decide how to forward that request based on conditions like path or hostname.
* Each rule has:
  * **Priority** (decides order)
  * **Conditions** (what to check for, like `/api` path)
  * **Actions** (where to send the request)
* Every listener must have a **default rule** (used when no other rule matches).
* You can add **more rules** for advanced traffic routing.
---
### **Target Groups**
* A **target group** is a set of servers (like EC2s, ECS tasks, or IPs) that receive traffic.
* You choose **protocol and port** (like HTTP on port 80) when setting up the group.
* One target (like an EC2) can belong to **multiple target groups**.
* **Health checks** are set at the target group level to monitor if the targets are working.
* If a target is unhealthy, traffic is not sent to it.
---
### **Attributes**
1. **Cross-Zone Load Balancing**
   * Distributes traffic across all targets in all Availability Zones evenly.
   * Helps with better performance and high availability.
2. **Monitoring**
   * **Access Logs** – Save details about requests handled by the load balancer (like IP, path, response code).
   * **Connection Logs** – Capture info about TCP connections (available for NLB).
---
### **Integrations**
1. **Amazon Application Recovery Controller (ARC)**
   * Helps monitor and control application availability during failures.
   * Works with ALB to switch traffic to healthy regions in case of outages.
2. **Amazon CloudFront + AWS Web Application Firewall (WAF)**
   * CloudFront is a global CDN that caches content close to users for low latency.
   * WAF protects your app by filtering harmful traffic (e.g., SQL injection, XSS).
   * Together with ALB, they improve security and performance.
3. **AWS Global Accelerator**
   * Improves performance by routing user traffic through AWS’s global network.
   * Provides static IPs and routes traffic to the optimal ALB/NLB endpoint.
4. **AWS Config**
   * Tracks configuration changes to your load balancer resources.
   * Useful for compliance and auditing.
5. **AWS Web Application Firewall (WAF)**
   * Protects ALB by blocking malicious requests based on rules.
   * Can filter traffic based on IP, headers, URI strings, or query parameters.

# NLB


# SSL Termination
# 3-way Handshake
# ISO/OSI Layer
# TCP/IP model

