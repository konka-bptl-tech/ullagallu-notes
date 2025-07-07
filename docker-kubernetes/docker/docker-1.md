# Physical Machines

# Virtual Machines

# Containers

---
### 🐳 **Docker**
1. **Docker is a containerization tool.** It packages application code + dependencies + minimal OS + runtime + required libraries.
2. That package is called an **image**. It's a collection of layers stacked on top of each other. It's a **read-only template** used to create containers.
3. **Containers** are running instances of an image. When created, they get their own CPU, memory, and isolated process space.
4. **Docker containers are good for local testing**.
5. **Containers are lightweight** because they don’t have a full OS.
6. They **start very fast** compared to virtual machines.
7. **Scaling** is also fast because containers are light and quick to boot.
---
### ✅ **Benefits of Docker**
* No more “**it works on my machine**” issues. Containers give **consistent results** across environments.
* Containers are **lightweight**, as they don’t contain a full OS.
* Containers are **isolated** — each one behaves like a **separate entity**.
* Easy to package, move, and run anywhere (portable).
* Works well in CI/CD pipelines and Kubernetes environments.
---
### 🔊 Konka’s Line (Optional Add-on for Interview):
> “Docker helps me build once and run anywhere — it’s fast, reliable, and gives me full control over the app environment.”
---
# Docker & Architecture
Perfect, Konka — you're absolutely on the right track. Let's polish your explanation first and then answer your question:

---

### 🛠️ Docker Client-Server Architecture (Konka Style)

* Docker follows a **client-server model**.
* The **client** sends instructions (like `docker run`, `docker build`, `docker pull`) to the **Docker server (daemon)**.
* The **Docker server (dockerd)** receives those commands, performs the requested actions (like creating containers, pulling images, managing volumes), and responds back to the client.

---

### 🧩 Docker Server Handles:

* **Containers** – Create, run, stop, remove
* **containerd** - manages container life cycle
* **Images** – Build, pull, push, remove
* **Volumes** – Create, attach, manage storage
* **Networking** – Create bridge/network, attach containers

# Docker Basic Commands
- docker run[for containers creation] 
- docker stats[to see the cpu,memroy,network I/O,ContainerID/Name]
- docker inspect[Shows detailed JSON info about a container, image, volume, etc.]
- docker logs[View the logs printed by the container stdout/stderr.]
- docker ps -aq[Lists all container IDs, including stopped ones.]
---
### 🧱 **Container Lifecycle (Konka's Version - Slightly Polished)**
> “A container goes through several stages during its life — like **created**, **running**, **paused**, **unpaused**, **stopped**, and finally **removed**.”
---
### 🔄 **Stages Explained Briefly:**
1. **Created**
   * The container is defined and ready, but not yet running.
   * Happens when you run `docker create`.
2. **Running**
   * The container is active and the application inside is executing.
   * This is the most common state.
3. **Paused** *(optional)*
   * All processes inside the container are temporarily frozen.
   * Useful for debugging or saving resources.
4. **Unpaused**
   * Container resumes from pause and continues running as usual.
5. **Stopped / Exited**
   * The container process has ended — either completed or crashed.
   * Can be started again later.
6. **Removed**
   * Container is deleted from the system.
   * Resources are freed and the container cannot be restarted unless re-created.
---
### 🔊 One-liner for Interview:
> “Containers move through different states like running, paused, stopped, and removed. We can control these states using Docker CLI, which helps us manage app behavior during testing, deployment, and scaling.”
---
### 🧠 How Containers Work Internally (Konka Style)
* **Namespaces** and **cGroups** are **Linux kernel features** that make containers possible.
* These two features provide **isolation** and **resource control**.
---
### 🔒 **Namespaces = Isolation**
> Each container gets its **own view of the system**.
When a container runs, it gets isolated using namespaces for:
* 🧠 `PID namespace` → Own process tree
* 🌐 `Network namespace` → Own IP, ports, interfaces
* 💽 `Mount namespace` → Own filesystem
* 👤 `User namespace` → Own user/group IDs
* 💬 `IPC namespace` → Own inter-process communication (shared memory, semaphores)
* 🔁 `UTS namespace` → Own hostname
> So, even if containers run on the same host, they don’t see or affect each other.
---
### ⚙️ **cGroups = Resource Control**
> Containers share the kernel but don’t fight for CPU or memory.
* `cGroups` (Control Groups) make sure each container gets a **fair amount of CPU, memory, and disk I/O**.
* You can **limit** and **monitor** resource usage using cGroups.
Example: You can say:
`This container should not use more than 512MB RAM and 1 CPU core.`
---
### 🏃‍♂️ **Container is Just a Process**
* When a container runs, it’s just a **regular Linux process** on the host — but isolated using namespaces and limited using cGroups.
* That’s why containers are **lightweight and fast** compared to VMs.
---
### 🔊 Konka’s One-Liner for Interview:
> “In Linux, containers are just isolated processes created using namespaces and controlled with cGroups. That’s how they stay lightweight and secure.”
