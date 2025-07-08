### **Ports & Protocols**
---
### ✅ **What is a Port?**
* A **port** is a **16-bit number** (range: `0–65535`) used to identify specific processes or services on a system.
* It works at the **Transport Layer** (Layer 4) of the OSI model.
* Ports help the operating system determine **which application** should handle incoming/outgoing data.
---
### ✅ **What is a Protocol?**
* A **protocol** defines the set of rules for **formatting, transmitting, and receiving** data between devices.
* Protocols operate at different layers of the OSI or TCP/IP model (e.g., Transport, Application).
---
### 🔢 **Port Ranges**
| Range       | Use Type                                        |
| ----------- | ----------------------------------------------- |
| 0–1023      | Well-known ports (standard services)            |
| 1024–49151  | Registered ports (user-defined apps)            |
| 49152–65535 | Dynamic/ephemeral ports (temporary connections) |
---
### 📌 **Common Ports & Their Protocols**
| Port | Protocol      | Description                               |
| ---- | ------------- | ----------------------------------------- |
| 20   | FTP (Data)    | File Transfer Protocol (Data transfer)    |
| 21   | FTP (Control) | File Transfer Protocol (Control channel)  |
| 22   | SSH           | Secure remote login and command execution |
| 23   | Telnet        | Remote login (unencrypted)                |
| 25   | SMTP          | Mail sending service                      |
| 53   | DNS           | Domain name resolution                    |
| 67   | DHCP (Server) | IP assignment by DHCP server              |
| 68   | DHCP (Client) | DHCP request from client                  |
| 80   | HTTP          | Web traffic (unencrypted)                 |
| 110  | POP3          | Mail receiving (downloads emails)         |
| 123  | NTP           | Time synchronization                      |
| 143  | IMAP          | Mail receiving (keeps emails on server)   |
| 161  | SNMP          | Monitoring network devices                |
| 389  | LDAP          | Directory service access                  |
| 443  | HTTPS         | Secure web traffic (SSL/TLS)              |
| 3306 | MySQL         | MySQL database communication              |
| 5432 | PostgreSQL    | PostgreSQL database communication         |
| 6379 | Redis         | Redis key-value store                     |
| 8080 | HTTP Alt      | Alternate port for web apps               |
---