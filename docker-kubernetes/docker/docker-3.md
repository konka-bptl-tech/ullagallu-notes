### 📝 **Docker Volumes – Konka Style**
> By default, **containers are ephemeral** — meaning when a container dies, **all data inside it is lost**.
> To **persist data**, Docker provides a solution called **volumes**.
---
### 🔹 1. **Named Volumes**
* Created and managed by Docker.
* Survive container restarts and deletions (until manually removed).
* Useful when you want reusable persistent storage.
```bash
docker volume create siva
```
```bash
docker run -v siva:/app/data myimage
```
---
### 🔹 2. **Anonymous Volumes**
* You mount a volume **without giving it a name**.
* Docker assigns a random name.
* Still persistent, but **not easy to manage or reuse**.
* Often created when you use:
```bash
docker run -v /app/data myimage
```
Docker sees no named volume → creates anonymous one.
---
### 🔹 3. **Host Path Volume (Bind Mounts)**
* You mount a **host directory directly into the container**.
* Useful for development (live code editing, logs, etc.).
```bash
docker run -v /host/path:/container/path myimage
```
* Changes inside the container will reflect on the host, and vice versa.
---
### ✅ Summary Table
| Volume Type      | Persistent? | Reusable? | Managed by Docker?  | Use Case                      |
| ---------------- | ----------- | --------- | ------------------- | ----------------------------- |
| Named Volume     | ✅ Yes       | ✅ Yes     | ✅ Yes               | Persistent storage            |
| Anonymous Volume | ✅ Yes       | ❌ No      | ✅ Yes               | Temporary, auto-created       |
| Host Path Volume | ✅ Yes       | ✅ Yes     | ❌ No (your control) | Dev/test, logs, config mounts |
---
### 🔊 Konka’s One-Liner:
> “Docker containers are ephemeral. To persist data, I use volumes — either named, anonymous, or bind mounts — depending on the use case.”
---
### 🧠 **How Docker Networking Works (Konka Style)**
> Docker networking defines **how containers communicate** with each other, with the host, and with the outside world.
When we run containers, Docker automatically connects them to a **network** — and this decides **how they talk to each other**.
---
### 🔗 **Docker Network Modes**
1. ### 🔹 **Bridge (default)**
   * Used when no network is specified.
   * Containers get a **private IP**, NATed to access the outside.
   * Containers can talk to each other **using IP**, but **no DNS resolution** (unless you create a custom bridge).
   * Example:
     ```bash
     docker run -d --name app1 nginx
     docker exec -it app1 ping app2  # ❌ name resolution won't work on default bridge
     ```
2. ### 🔹 **Host**
   * Container shares the **host's network stack**.
   * No isolation — it uses the host IP, ports.
   * Useful for performance or when app needs full network access.
   * Example:
     ```bash
     docker run --network host myimage
     ```
3. ### 🔹 **None**
   * No network at all. Container is isolated from everything.
   * Used in security-sensitive or testing cases.
   * Example:
     ```bash
     docker run --network none myimage
     ```
4. ### 🔹 **Custom Bridge**
   * Manually created bridge network.
   * Supports **DNS resolution by container name**.
   * Recommended way for containers to communicate with each other by name.
   ```bash
   docker network create mynet
   docker run --network mynet --name app1 nginx
   docker run --network mynet --name app2 busybox
   docker exec -it app2 ping app1  # ✅ DNS works here
   ```
---
### 🧰 **Useful Docker Network Commands**
| Task                           | Command                                       |
| ------------------------------ | --------------------------------------------- |
| List networks                  | `docker network ls`                           |
| Inspect a network              | `docker network inspect <name>`               |
| Create a custom bridge network | `docker network create mynet`                 |
| Connect container to network   | `docker network connect mynet mycontainer`    |
| Disconnect container           | `docker network disconnect mynet mycontainer` |
---
### 🔊 Konka's Interview Line:
> “Docker supports multiple networking modes like bridge, host, none, and custom bridge. I usually use a **custom bridge** so that containers can communicate by name using Docker's built-in DNS.”
---
### 🧩 **What is Docker Compose? (Konka Style)**
> Docker Compose is a tool used to **run multiple containers at once** using a simple **YAML file**.
---
### 📦 Why Docker Compose?
* When you have a multi-container app (like frontend, backend, DB), running them manually with `docker run` becomes hard.
* With Compose, you define **all containers and configs in one file**, and bring them up with one command.
---
### 💡 Example: 3-Tier App
Let’s say you have:
* Frontend (React)
* Backend (Node.js)
* Database (MySQL)
With Compose, you define all in `docker-compose.yml`:
```yaml
version: "3.8"
services:
  frontend:
    image: my-frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend

  backend:
    image: my-backend
    ports:
      - "4000:4000"
    depends_on:
      - db

  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: root
```
Then just run:
```bash
docker-compose up -d
```
All 3 containers start together — properly linked.
---
### ✅ Benefits:
* Easy to manage multi-container apps
* Networking is built-in (they talk via service names)
* Supports environment variables, volume mounts, healthchecks
* `depends_on` helps manage container startup order
---
### 🔧 Common Commands:
| Task                 | Command                  |
| -------------------- | ------------------------ |
| Start all containers | `docker-compose up -d`   |
| Stop all containers  | `docker-compose down`    |
| View logs            | `docker-compose logs -f` |
| Rebuild images       | `docker-compose build`   |
---
### 🏆 **Docker Best Practices (Konka Style)**
---
### 🔹 1. **Use Small Base Images**
* Use Alpine, Distroless, or minimal OS
* Reduces image size, attack surface
```Dockerfile
FROM alpine:latest
```
---
### 🔹 2. **Use Multi-Stage Builds**
* Keeps the final image clean and light
* Separate build tools from runtime
```Dockerfile
FROM golang:1.21 AS builder
# build app...
FROM alpine
# copy only binary from builder
```
---
### 🔹 3. **Set a Non-Root USER**
* Avoid running as `root`
* Improve security
```Dockerfile
USER appuser
```
---
### 🔹 4. **Minimize Layers**
* Combine `RUN` commands with `&&` to reduce layers
```Dockerfile
RUN apt-get update && \
    apt-get install -y nginx && \
    rm -rf /var/lib/apt/lists/*
```
---
### 🔹 5. **Keep Changing Instructions at the Bottom**
* Helps Docker use layer cache
* Speeds up rebuilds
```Dockerfile
# static installs first
# COPY source code and RUN last
```
---
### 🔹 6. **Use `.dockerignore`**
* Avoid copying unnecessary files into the image (like `.git`, `node_modules`, etc.)
Create a `.dockerignore` file:
```
.git
node_modules
*.log
```
---
### 🔹 7. **Use `ENTRYPOINT` + `CMD` Together**
* `ENTRYPOINT` defines the main process
* `CMD` passes default arguments
```Dockerfile
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
```
---
### 🔹 8. **Tag Your Images Properly**
* Avoid using only `latest`
* Use meaningful tags like `v1.0.0`, `stable`, `prod`
---
### 🔹 9. **Use Volumes for Persistent Data**
* Don’t store data inside containers
```bash
docker run -v myvolume:/data myimage
```
---
### 🔹 10. **Scan Images for Vulnerabilities**
* Use tools like Docker Scout, Trivy, or Snyk
```bash
docker scout quickview myimage
```
---
### 🔊 Konka’s One-Liner:
> “I follow Docker best practices like using small images, multi-stage builds, `.dockerignore`, and non-root users to build secure, fast, and production-ready containers.”
---
### 🔍 **Trivy – Docker Image Scanning Tool (Konka Style)**
---
### 🧠 What is Trivy?
> **Trivy** is an open-source vulnerability scanner that checks for:
* OS package vulnerabilities (e.g., in Alpine, Debian)
* Application dependencies (Node.js, Python, Java, Go)
* Misconfigurations (in Dockerfiles, Kubernetes, Terraform)
* Exposed secrets
---
### 🚀 Why Trivy?
| Feature            | Benefit                              |
| ------------------ | ------------------------------------ |
| Fast & lightweight | Easy to integrate in CI/CD           |
| Easy to use        | Just one command to scan             |
| Wide coverage      | Scans images, filesystems, Git repos |
| Active community   | Regular updates and support          |
---
### 🛠️ How to Install Trivy
#### Mac (Homebrew):
```bash
brew install aquasecurity/trivy/trivy
```
#### Linux (Script):
```bash
sudo apt install wget
wget https://github.com/aquasecurity/trivy/releases/latest/download/trivy_Linux-64bit.tar.gz
tar -xvf trivy_Linux-64bit.tar.gz
sudo mv trivy /usr/local/bin/
```
---
### 🔧 How to Use Trivy
#### ✅ Scan Docker Image:
```bash
trivy image nginx:latest
```
Scans the `nginx` image and shows CVEs (vulnerabilities) with severity.
#### ✅ Scan Local Filesystem:
```bash
trivy fs .
```
Useful to scan source code folders.
#### ✅ Scan Dockerfile or Kubernetes YAML:
```bash
trivy config ./Dockerfile
trivy config ./k8s/deployment.yaml
```
Checks for **misconfigurations**.
---
### ⚙️ Sample Output
```
nginx:latest (debian 11)
=========================
Total: 5 (HIGH: 2, MEDIUM: 2, LOW: 1)
- CVE-2023-XXXX: openssl
- CVE-2023-YYYY: libssl
```
---
### 💡 Best Practice:
> Run Trivy in your **CI/CD pipeline** to catch vulnerabilities before deployment.
---
### 🔊 Konka’s One-Liner:
> “I use Trivy to scan my Docker images and configs for vulnerabilities and misconfigurations. It’s fast, easy, and fits well in my CI/CD process.”
---