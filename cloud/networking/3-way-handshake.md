## **1. TCP (Transmission Control Protocol)**
* It's the **base connection** for most internet traffic.
* It's like a **delivery truck** that makes sure all the data reaches safely, in order, and without loss.
* Both **HTTP** and **HTTPS** run **on top of TCP**.
* TCP works at **Layer 4** (Transport Layer).
Example:
> Before a web browser talks to a server (via HTTP or HTTPS), a **TCP connection** is established first (3-way handshake: SYN, SYN-ACK, ACK).
---
## **2. HTTP (Hypertext Transfer Protocol)**
* It runs **on top of TCP**, usually on **port 80**.
* Used for **web communication** (like loading a website).
* Data is sent in **plain text** — anyone in the middle can read it.
* **Not secure** – anyone can sniff or modify the content.
Example:
> `http://example.com`
> Anyone can see your username or password if it’s sent over HTTP.
---
## **3. HTTPS (HTTP Secure)**
* It’s **HTTP + SSL/TLS encryption**.
* Runs on **port 443** (still uses TCP).
* Adds **encryption**, **data integrity**, and **authentication**.
* Used when you need a **secure connection** between client and server.
Example:
> `https://example.com`
> All data (like form input, passwords, session tokens) is encrypted before transmission.
---
### 🔐 SSL/TLS Termination in ALB
* With ALB, **HTTPS termination** happens at the **load balancer**.
* The connection between:
  * **Client ↔️ ALB** is **HTTPS** (encrypted).
  * **ALB ↔️ Backend** can be **HTTP** (optional) — useful inside private networks.
---
## 🔁 Summary Table
| Feature   | HTTP         | HTTPS              | TCP                |
| --------- | ------------ | ------------------ | ------------------ |
| Port      | 80           | 443                | Any (customizable) |
| Encrypted | ❌ No         | ✅ Yes              | ❌ No (by default)  |
| Runs on   | TCP          | TCP + TLS/SSL      | Transport Layer    |
| Used for  | Web traffic  | Secure web traffic | Base for both      |
| Secure    | ❌ Not Secure | ✅ Secure           | ❌ Not Secure       |
---
### **3-Way Handshake (TCP Connection Setup)**
The **3-way handshake** is the process used to **establish a TCP connection** between client and server.
---
### 🔁 Steps:
1. **SYN (Synchronize)**
   * Client sends a **SYN** packet to the server.
   * This says: "I want to connect, here’s my sequence number."
2. **SYN-ACK (Synchronize-Acknowledge)**
   * Server responds with a **SYN-ACK**.
   * This says: "Okay, I accept. Here’s my sequence number and I got yours."
3. **ACK (Acknowledge)**
   * Client sends an **ACK** to the server.
   * This says: "Thanks, I got your sequence number. Let’s start talking!"
✅ Now, the connection is **established** and data transfer can begin.
---
### 📦 Real-Life Example:
> You (client) knock on a door (SYN)
> The person inside says "Who’s there?" (SYN-ACK)
> You reply "It’s me!" (ACK)
> Now both start a conversation (data exchange)
---
### 🔒 Related to HTTPS?
Yes — before **HTTPS** communication, this **TCP 3-way handshake happens first**, then the **TLS handshake** (for encryption) begins.
---