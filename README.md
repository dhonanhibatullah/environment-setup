# **Environment Setup**

This repository contains the local development setup for the Bengkel Inovasi infrastructure, orchestrating multiple services using Docker Compose and Makefiles.

## **A. Architecture**

*   **Shared Network**: All containers are connected to a shared external Docker network named `donspace-net`, allowing direct communication between services using container names (e.g., `donspace-postgres`, `donspace-mqtt`).

*   **Log Collection**: The Grafana stack (Alloy, Loki) collects logs from all containers labeled with `collect_logs: "true"`.

## **B. Prerequisites**

*   Docker Engine: [link](https://docs.docker.com/engine/)
*   Make: [link](https://www.gnu.org/software/make/)

## **C. Services Installation**

1.  Copy and modify the `.env.example` with

    ```bash
    cp .env.example .env
    nano .env
    ```

2.  After adjusting the `.env`, each service directory contains a `Makefile` with the following targets:

    *   `make install`: Checks dependencies, creates the network, and starts the service.

    *   `make stop`: Stops the service containers.

    *   `make uninstall`: Stops and removes containers and **all volumes** (data loss).

## **D. Cloudflared Settings**

### **1. Cloudflared Client-Side Installation**

On Powershell, execute:

```bash
winget install Cloudflare.cloudflared
```

### **2. SSH**

Add proxy rule on `C:\Users\User\.ssh\config`:

```bash
Host {CLOUDFLARE_ROUTE}
    ProxyCommand "C:\Program Files (x86)\cloudflared\cloudflared.exe" access ssh --hostname %h
```

### **3. RDP**

On Powershell, run `cloudflared access` with:

```bash
cloudflared.exe access tcp --hostname {CLOUDFLARE_ROUTE} --url localhost:3389
```
