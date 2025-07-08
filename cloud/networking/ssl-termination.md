### **SSL Termination**
* SSL Termination means **decrypting** the incoming **HTTPS request** at the **load balancer level**.
* After termination, the traffic from Load Balancer to backend can go as **HTTP** (unencrypted) or **HTTPS** (optional).
* This helps to **offload SSL work** from backend servers, so they don't need to handle encryption/decryption.
---
### 🔁 Flow Example (with ALB):
1. Client → ALB:
   Sends request over **HTTPS** (encrypted)
2. ALB:
   **Terminates SSL** (decrypts request)
3. ALB → Backend:
   Sends traffic over **HTTP** (or HTTPS if needed)
---
### ✅ Benefits:
* **Reduces CPU load** on backend servers
* **Simplifies certificate management** (SSL certificate only on ALB)
* Can still keep **secure communication** inside network if needed
---
### 🔐 Optional: End-to-End Encryption
* You can choose to **re-encrypt** traffic from ALB to backend using HTTPS.
* Useful when backend is in a different VPC, region, or security is very critical.
---
