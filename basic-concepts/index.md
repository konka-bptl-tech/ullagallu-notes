# Basic Concepts
- Idempotency
- Mutability & Immutability
- DRY principle

# Networking
- ISO/OSI & TCP/IP
- DNS & DHCP
- Forward & Reverse Proxy
- Nat
- Subnetting & CIDR & Subnet Mask
- SSL/TLS Basics & SSL Termination and 3-way hand shake
- Caching
- Loadbalacing
- Scalability

# DevOps
- SDLC
- Waterfall, Agile Methodology
- DevOps Practices
- CI/CD
- DecSecOps
- GitOps


















---

## 🧠 Basic Concepts

### **Idempotency**

* **Definition**: An operation is *idempotent* if performing it multiple times has the same effect as doing it once.
* **Example**: `DELETE /user/123` is idempotent — deleting the user once or many times results in the same state (user is gone).

### **Mutability & Immutability**

* **Mutable**: An object or data that can be changed after creation.
* **Immutable**: Once created, it cannot be changed. Instead, a new object is created.
* **Example**: In Python, lists are mutable (`list.append()`), but tuples are immutable.

### **DRY Principle (Don't Repeat Yourself)**

* **Definition**: DRY is a software engineering principle aimed at reducing repetition by abstracting repeated logic or data into a single place.
* **Purpose**: Improves maintainability, reduces bugs, and enhances readability.
* **Example (Bad - Repetition)**:

  ```bash
  cp /source/file1 /dest/
  cp /source/file2 /dest/
  ```
* **Example (Good - DRY)**:

  ```bash
  for file in file1 file2; do
    cp "/source/$file" /dest/
  done
  ```
---

## 🌐 Networking

### **ISO/OSI vs. TCP/IP**

* **OSI Model (7 layers)**:

  1. Physical
  2. Data Link
  3. Network
  4. Transport
  5. Session
  6. Presentation
  7. Application
* **TCP/IP Model (4 layers)**:

  1. Network Interface
  2. Internet
  3. Transport
  4. Application
* **Purpose**: Both describe how data moves across networks; OSI is more theoretical, TCP/IP is practical.

### **DNS & DHCP**

* **DNS (Domain Name System)**: Resolves domain names (like `example.com`) to IP addresses.
* **DHCP (Dynamic Host Configuration Protocol)**: Automatically assigns IP addresses and network settings to devices.

### **Forward & Reverse Proxy**

* **Forward Proxy**: Sits between client and server, typically used for filtering or caching (e.g., accessing blocked websites).
* **Reverse Proxy**: Sits in front of servers, used for load balancing, SSL termination, etc. (e.g., Nginx, HAProxy).

### **NAT (Network Address Translation)**

* Converts private IPs to a public IP (and vice versa), allowing multiple devices to share one public IP.
* Common in home/office networks for security and IP conservation.

### **Subnetting, CIDR, and Subnet Mask**

* **Subnetting**: Dividing a network into smaller sub-networks.
* **CIDR (Classless Inter-Domain Routing)**: Notation like `192.168.1.0/24` to define IP ranges.
* **Subnet Mask**: A binary mask used to separate IP address into network and host parts.

### **Caching**

* Temporarily storing data for faster access.
* Types: Browser cache, CDN cache, server-side cache, database cache (e.g., Redis).

### **Load Balancing**

* Distributes incoming traffic across multiple servers to improve performance and availability.
* Types: Round-robin, least connections, IP-hash.

### **Scalability**

* **Vertical Scaling**: Increasing power of a single machine (CPU/RAM).
* **Horizontal Scaling**: Adding more machines to handle load.
* Goal: Handle more users/data efficiently.

No worries at all — that’s actually a **very important concept**, especially in networking, security, and DevOps. Here's a clear explanation:

---

## 🔒 SSL/TLS Basics

### **What is SSL/TLS?**

* **SSL (Secure Sockets Layer)** and **TLS (Transport Layer Security)** are cryptographic protocols used to **secure communication** over the internet.
* TLS is the modern, secure version. SSL is **deprecated**, but the term "SSL" is still commonly used.
* Used in **HTTPS**, email, VoIP, etc.

---

## 🔁 TLS Handshake (Including 3-Way TCP Handshake)

There are **two handshakes** to understand:

---

### ✅ **1. TCP 3-Way Handshake** (Before TLS)

This sets up a **TCP connection** before any secure communication can begin.

1. **SYN**: Client → Server (`I want to connect`)
2. **SYN-ACK**: Server → Client (`Okay, let's connect`)
3. **ACK**: Client → Server (`Thanks, connected!`)

➡️ At this point, the TCP connection is established. Then comes TLS.

---

### ✅ **2. TLS Handshake (Simplified)**

Steps may vary by version, but here’s a basic TLS 1.2+ handshake:

1. **Client Hello**:

   * Client sends supported cipher suites, TLS version, and a random number.

2. **Server Hello**:

   * Server picks a cipher, sends its **digital certificate** (usually X.509 with public key), and its random number.

3. **Key Exchange**:

   * Client verifies the certificate (via CA).
   * Client creates a **pre-master secret**, encrypts it with the server’s **public key**, and sends it to the server.
   * Both sides use this to generate the **session key**.

4. **Finished Messages**:

   * Both sides send a "Finished" message encrypted with the session key to verify successful handshake.

➡️ Now, both parties can securely exchange data using **symmetric encryption** (fast).

---

## 🛑 SSL Termination

### **What is it?**

* SSL termination is the process where **SSL/TLS is decrypted at a proxy or load balancer** instead of at the backend server.

### **Why Use It?**

* Offloads encryption/decryption from your app servers.
* Centralizes SSL management (e.g., one certificate on Nginx).
* Improves performance and simplifies certificate handling.

### **Common SSL Terminators**

* Nginx
* HAProxy
* AWS ELB (Elastic Load Balancer)

---

## 🔁 Putting It All Together

1. **User → Internet**: Makes HTTPS request.
2. **TCP Handshake**: TCP connection established (3-way handshake).
3. **TLS Handshake**: Secure tunnel is created.
4. **SSL Termination**: TLS ends at load balancer or proxy.
5. **Plain HTTP** may be used internally (unless end-to-end TLS is enforced).

---

## ⚙️ DevOps

### **SDLC (Software Development Life Cycle)**

* Phases: Requirements → Design → Implementation → Testing → Deployment → Maintenance
* Ensures systematic software development and quality.

### **Waterfall vs. Agile**

* **Waterfall**: Linear and sequential (plan everything upfront).
* **Agile**: Iterative and incremental, adapting to changes; popular frameworks include Scrum and Kanban.

### **DevOps Practices**

* Bridge between **development** and **operations** for faster, reliable delivery.
* Includes automation, monitoring, infrastructure as code (IaC), and collaboration tools.

### **CI/CD (Continuous Integration/Continuous Deployment)**

* **CI**: Automatically build and test code on every commit.
* **CD**: Automatically deploy code to production (or staging) after CI passes.

### **DevSecOps**

* Extension of DevOps that integrates **security** into CI/CD pipelines.
* Shifts security checks left (early) in the development process.

### **GitOps**

* Infrastructure and application deployments managed using **Git** as the single source of truth.
* Combines Git + CI/CD + Kubernetes (often).

---