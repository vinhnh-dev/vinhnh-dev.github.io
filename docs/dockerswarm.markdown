---
layout: page
title: Docker Swarm Note
permalink: /docker-swarm/
parent: Miscellaneous
nav_order: 6
---

# 🐳 Docker Swarm — Complete Study Notes

> **Reference:** [Docker Official Docs](https://docs.docker.com/engine/swarm/)  
> **Updated:** 2025

---

## 📋 Table of Contents

1. [Why Do We Need Container Orchestration?](#1-why-do-we-need-container-orchestration)
2. [What is Docker Swarm?](#2-what-is-docker-swarm)
3. [Docker Swarm Components](#3-docker-swarm-components)
4. [Manager & Worker Nodes](#4-manager--worker-nodes)
5. [Node Status](#5-node-status)
6. [Initializing & Managing a Swarm](#6-initializing--managing-a-swarm)
7. [Services — Create & Manage](#7-services--create--manage)
8. [Scaling Up & Down](#8-scaling-up--down)
9. [Rolling Updates & Rollback](#9-rolling-updates--rollback)
10. [Managing Nodes](#10-managing-nodes)
11. [Docker Swarm Config](#11-docker-swarm-config)
12. [Docker Swarm Secrets](#12-docker-swarm-secrets)
13. [Health Check](#13-health-check)
14. [Full Cheat Sheet](#14-full-cheat-sheet)

---

## 1. Why Do We Need Container Orchestration?

When running **hundreds of containers**, manual management becomes impractical. A **Container Orchestration** system like Docker Swarm solves this by automating the entire cluster lifecycle.

| # | Feature | Description |
|---|---------|-------------|
| ❤️ | **Health Checks** | Automatically monitor and restart failed containers |
| 🚀 | **Deploying Fixed Containers** | Ensure the correct number of replicas are always running |
| 📐 | **Scaling** | Increase or decrease container count on demand |
| 🔄 | **Rolling Updates** | Update containers with zero downtime |
| ⚖️ | **Load Balancing** | Distribute traffic evenly across containers |

---

## 2. What is Docker Swarm?

> **Docker Swarm** is a technique that **joins multiple Docker Engines** running on different hosts and uses them together as a **unified cluster**.

- 💡 Simply **declare your requirements as stacks of services**, and Docker Swarm handles the rest.
- The entire cluster maintains a **shared STATE** — ensuring consistency across all nodes.

---

## 3. Docker Swarm Components

### 🌐 Swarm
> A **mode** consisting of multiple Docker hosts running in a **cluster**.

### 🖥️ Node
> Each **node** is an **individual Docker Engine** participating in the swarm.

### ⚙️ Service
> A **service** is the definition of tasks to be executed on swarm nodes.

A service defines:
| | Information |
|-|-------------|
| 🖼️ | Which **Image** to use |
| 🔢 | **Number of containers** to run |
| 💻 | **Commands** to execute inside the container |
| 🌐 | **Ports, Volumes, Networks** to use |

### 📦 Task
> A **task** carries a Docker container and the commands to run inside it.

- 🔒 Once a task is **assigned to a node**, it **cannot be moved** to another node.
- ✅ It can only **run on its assigned node** — or **fail**.

---

## 4. Manager & Worker Nodes

### 🔴 Manager Nodes
- The host we **directly communicate with**
- **Assigns tasks** to other nodes
- Maintains the **cluster state**
- **Schedules** services across the cluster
- Serves **Swarm mode HTTP API endpoints**

### 🟢 Worker Nodes
- Where **containers actually run**
- Receives and executes **tasks** assigned by the Manager

```
          ┌─────────────────── SWARM CLUSTER ───────────────────┐
          │                                                       │
     ┌────┴────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐     │
     │ Manager │   │ Worker  │   │ Worker  │   │ Worker  │     │
     │ (Leader)│   │[Task→🐳]│   │[Task→🐳]│   │[Task→🐳]│     │
     └─────────┘   └─────────┘   └─────────┘   └─────────┘     │
          │              Shared STATE across all nodes            │
          └───────────────────────────────────────────────────────┘
```

---

## 5. Node Status

| STATUS | MEANING |
|--------|---------|
| *(blank)* | Worker Node |
| **LEADER** | Primary Manager Node |
| **REACHABLE** | Candidate to become Leader (can be Promoted) |
| **UNAVAILABLE** | Manager Node that cannot communicate with other nodes |
| **DRAIN** | Node will not accept new container assignments |

---

## 6. Initializing & Managing a Swarm

> Docker Engine has **swarm mode disabled by default**. You need to create a new swarm or join an existing one to enable it.

```bash
# Initialize a swarm
$ docker swarm init

# Initialize with a specific IP address
$ docker swarm init --advertise-addr <IP>

# Worker joins an existing swarm cluster
$ docker swarm join --token <TOKEN> <MANAGER-IP>

# List all nodes in the swarm
$ docker node ls
```

---

## 7. Services — Create & Manage

### Basic Syntax

```bash
$ docker service create --name <NAME> --replicas <#> -p <HOST_PORT:CONTAINER_PORT> IMAGE
```

### Practical Examples

```bash
# Simple nginx service
$ docker service create -p 80:80 --name web nginx

# nginx service with 3 replicas
$ docker service create --replicas 3 -p 80:80 --name web nginx
```

### View & Inspect Services

```bash
# List all services
$ docker service ls

# List tasks of a service
$ docker service ps <ServiceName>

# Inspect a service in a readable format
$ docker service inspect --pretty <ServiceName>

# Remove a service
$ docker service rm <ServiceName>
```

---

## 8. Scaling Up & Down

```bash
# Scale service "web" to 5 replicas
$ docker service scale web=5

# Or use update command
$ docker service update --replicas=5 web
```

---

## 9. Rolling Updates & Rollback

> Control **how many containers are updated simultaneously** and how to handle failures.

```bash
# Update service to a new image
$ docker service update --image <NEW_IMAGE> <ServiceName>

# Change the number of replicas
$ docker service update --replicas=5 <ServiceName>

# Rollback to the previous version
$ docker service update --rollback <ServiceName>

# Automatically rollback if update fails
$ docker service update --update-failure-action=rollback <ServiceName>
```

---

## 10. Managing Nodes

```bash
# Inspect details of a node
$ docker node inspect --pretty <NODE>

# Promote Worker → Manager
$ docker node promote <NODE>

# Demote Manager → Worker
$ docker node demote <NODE>

# Drain a node (prevent new container assignments)
$ docker node update --availability drain <NODE>

# Reactivate a node to receive tasks
$ docker node update --availability active <NODE>
```

---

## 11. Docker Swarm Config

### 📌 What is Config?

> **Docker Config** allows storing **non-sensitive configuration data** outside of images or containers — for example: nginx config files, application configuration files, etc.

**Characteristics:**
- ✅ Stored **unencrypted** (unlike Secrets)
- ✅ Mounted directly into the container filesystem (not RAM disk)
- ✅ Replicated across all Manager nodes (high availability)
- ✅ Can be added/removed from a service at any time
- ✅ Maximum size: **500 KB**
- ⚠️ Only works with **Swarm services**, not standalone containers

**Default mount location:**
- Linux: `/<config-name>`
- Windows: `C:\<config-name>`

---

### 🔧 Docker Config Commands

```bash
# Create config from stdin
$ echo "server { listen 80; }" | docker config create my-config -

# Create config from a file
$ docker config create site.conf ./site.conf

# List all configs
$ docker config ls

# Inspect a config
$ docker config inspect <config-name>

# Remove a config (only when no service is using it)
$ docker config rm <config-name>
```

---

### 🛠️ Using Config with a Service

```bash
# Attach config to a service (mounted at /<config-name>)
$ docker service create \
    --name redis \
    --config my-config \
    redis:alpine

# Attach config with custom path and permissions
$ docker service create \
    --name nginx \
    --config source=site.conf,target=/etc/nginx/conf.d/site.conf,mode=0440 \
    -p 80:80 \
    nginx:latest

# Remove config from a running service
$ docker service update --config-rm my-config redis

# Add a new config to a running service
$ docker service update \
    --config-add source=site-v2.conf,target=/etc/nginx/conf.d/site.conf \
    nginx
```

---

### 🔄 Rotating (Updating) a Config

> Configs are **immutable** — they cannot be edited directly. To update: create a new config → update the service → remove the old config.

```bash
# Step 1: Create a new config with updated content
$ docker config create site-v2.conf ./site.conf

# Step 2: Update the service to use the new config and remove the old one
$ docker service update \
    --config-rm site.conf \
    --config-add source=site-v2.conf,target=/etc/nginx/conf.d/site.conf,mode=0440 \
    nginx

# Step 3: Remove the old config
$ docker config rm site.conf
```

---

### 📄 Config in Docker Compose / Stack

```yaml
version: "3.9"
services:
  nginx:
    image: nginx:latest
    configs:
      - source: nginx_config
        target: /etc/nginx/conf.d/default.conf

configs:
  nginx_config:
    file: ./nginx.conf
```

---

## 12. Docker Swarm Secrets

### 🔐 What is a Secret?

> **Docker Secret** stores **sensitive data** such as passwords, SSH keys, TLS certificates, etc., in a **fully encrypted and secure** manner.

**Config vs Secret Comparison:**

| Criteria | Config | Secret |
|----------|--------|--------|
| Data type | Non-sensitive | Sensitive |
| Encrypted | ❌ No | ✅ Yes (at rest & in transit) |
| Mount type | Direct filesystem | RAM disk (tmpfs) |
| Default location (Linux) | `/<config-name>` | `/run/secrets/<secret-name>` |
| Max size | 500 KB | 500 KB |

**Security characteristics of Secrets:**
- 🔒 Encrypted at rest in the **Raft log**
- 🔒 Encrypted in transit via **TLS connection**
- 🔒 Only mounted into containers as **in-memory (tmpfs)**
- 🔒 Automatically **wiped from memory** when the container stops
- 🔒 Not persisted if `docker commit` is used

---

### 🔧 Docker Secret Commands

```bash
# Create a secret from stdin
$ printf "my_password_123" | docker secret create my_secret -

# Create a secret from a file
$ docker secret create site.key ./site.key

# List all secrets
$ docker secret ls

# Inspect a secret (content is NOT displayed)
$ docker secret inspect <secret-name>

# Remove a secret (only when no service is using it)
$ docker secret rm <secret-name>
```

---

### 🛠️ Using Secrets with a Service

```bash
# Attach a secret to a service
$ docker service create \
    --name myapp \
    --secret my_secret \
    myapp:latest

# Attach secret with custom path and permissions
$ docker service create \
    --name myapp \
    --secret source=my_secret,target=/run/secrets/db_password,mode=0400 \
    myapp:latest

# Remove a secret from a running service
$ docker service update --secret-rm my_secret myapp

# Add a new secret to a running service
$ docker service update \
    --secret-add source=my_secret_v2,target=/run/secrets/db_password \
    myapp
```

---

### 📄 Secrets in Docker Compose / Stack

```yaml
version: "3.9"
services:
  myapp:
    image: myapp:latest
    secrets:
      - db_password

secrets:
  db_password:
    file: ./db_password.txt
    # Or use an existing swarm secret:
    # external: true
```

---

## 13. Health Check

### 🏥 What is a Health Check?

> **Health Check** is a mechanism that allows Docker to **periodically test** whether a container is still working correctly.

- 🟢 **healthy** — container is working normally
- 🟡 **starting** — container is starting up (grace period)
- 🔴 **unhealthy** — container has failed the health check; Docker Swarm will **automatically restart** it

---

### ⚙️ Health Check Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `--interval` | Time between each health check | `30s` |
| `--timeout` | Max time to wait for a response | `30s` |
| `--start-period` | Grace period before checks begin (container startup time) | `0s` |
| `--retries` | Number of consecutive failures before marking unhealthy | `3` |

---

### 🔧 Health Check in Dockerfile

```dockerfile
# Basic syntax
HEALTHCHECK --interval=<duration> --timeout=<duration> --retries=<n> \
  CMD <command>

# Example: Check nginx every 30s, timeout 10s, retry 3 times
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost/ || exit 1

# Example: Check a Node.js app
HEALTHCHECK --interval=15s --timeout=5s --start-period=10s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

# Disable health check (override parent image)
HEALTHCHECK NONE
```

---

### 🛠️ Health Check in Docker Service / Compose

```bash
# Add health check when creating a service
$ docker service create \
    --name web \
    --replicas 3 \
    --health-cmd "curl -f http://localhost/ || exit 1" \
    --health-interval 30s \
    --health-timeout 10s \
    --health-retries 3 \
    --health-start-period 15s \
    nginx
```

```yaml
# Health check in Docker Compose / Stack file
version: "3.9"
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 15s
    deploy:
      replicas: 3
      restart_policy:
        condition: on-failure
```

---

### 🔍 Checking Container Health Status

```bash
# View health status of all containers
$ docker ps

# Inspect detailed health check logs
$ docker inspect --format='{{json .State.Health}}' <container_id> | jq

# View service tasks (shows health in swarm)
$ docker service ps <ServiceName>
```

---

### 🔄 How Docker Swarm Handles Unhealthy Containers

```
Container starts
      │
      ▼
[starting] ── start-period grace time ──►
      │
      ▼
[healthy] ◄──── Check passes ────┐
      │                          │
      ▼                          │
 Check runs every --interval     │
      │                          │
      ├── PASS ──────────────────┘
      │
      └── FAIL (retries exceeded)
            │
            ▼
       [unhealthy]
            │
            ▼
   Docker Swarm automatically
   stops & restarts the task/container
```

---

### ⚠️ Health Check Best Practices

- ✅ Always define a `--start-period` to give containers time to boot before checks begin
- ✅ Use lightweight commands for health checks (e.g., `curl`, `wget`, `pg_isready`)
- ✅ Prefer checking an actual endpoint (e.g., `/health`, `/ping`) over just pinging the process
- ✅ Keep `--timeout` shorter than `--interval` to avoid overlapping checks
- ✅ Combine health checks with Swarm's `restart_policy` for full auto-recovery
- ⚠️ Avoid heavy operations in health check commands — they run frequently

---

## 14. Full Cheat Sheet

```bash
# ============================================================
# 🐳 SWARM INITIALIZATION
# ============================================================
docker swarm init
docker swarm init --advertise-addr <IP>
docker swarm join --token <TOKEN> <MANAGER-IP>

# ============================================================
# 🖥️ NODE MANAGEMENT
# ============================================================
docker node ls
docker node inspect --pretty <NODE>
docker node promote <NODE>                          # Worker → Manager
docker node demote <NODE>                           # Manager → Worker
docker node update --availability drain <NODE>      # Stop assigning tasks
docker node update --availability active <NODE>     # Re-enable node

# ============================================================
# ⚙️ SERVICE MANAGEMENT
# ============================================================
docker service create --name <NAME> --replicas <#> -p <HP:CP> <IMAGE>
docker service ls
docker service ps <ServiceName>
docker service inspect --pretty <ServiceName>
docker service scale <ServiceName>=<#>
docker service rm <ServiceName>

# ============================================================
# 🔄 ROLLING UPDATE & ROLLBACK
# ============================================================
docker service update --image <NEW_IMAGE> <ServiceName>
docker service update --replicas=5 <ServiceName>
docker service update --rollback <ServiceName>
docker service update --update-failure-action=rollback <ServiceName>

# ============================================================
# 📄 CONFIG MANAGEMENT
# ============================================================
docker config create <config-name> <file>
docker config ls
docker config inspect <config-name>
docker config rm <config-name>
docker service update --config-rm <config-name> <ServiceName>
docker service update --config-add source=<config-name>,target=<path> <ServiceName>

# ============================================================
# 🔐 SECRET MANAGEMENT
# ============================================================
printf "<value>" | docker secret create <secret-name> -
docker secret create <secret-name> <file>
docker secret ls
docker secret inspect <secret-name>
docker secret rm <secret-name>
docker service update --secret-rm <secret-name> <ServiceName>
docker service update --secret-add source=<secret-name>,target=<path> <ServiceName>

# ============================================================
# ❤️ HEALTH CHECK
# ============================================================
docker service create \
  --name <NAME> \
  --health-cmd "curl -f http://localhost/ || exit 1" \
  --health-interval 30s \
  --health-timeout 10s \
  --health-retries 3 \
  --health-start-period 15s \
  <IMAGE>

docker ps                                                         # View health status
docker inspect --format='{{json .State.Health}}' <container_id>  # Detailed health logs
docker service ps <ServiceName>                                   # Swarm task health
```

---

### 📄 Complete Docker Stack File Example

```yaml
version: "3.9"

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    configs:
      - source: nginx_config
        target: /etc/nginx/conf.d/default.conf
    secrets:
      - db_password
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 15s
    deploy:
      replicas: 3
      restart_policy:
        condition: on-failure
      update_config:
        parallelism: 1
        delay: 10s
        failure_action: rollback

configs:
  nginx_config:
    file: ./nginx.conf

secrets:
  db_password:
    file: ./db_password.txt
```

---

## 🗺️ Docker Swarm — Full Architecture Overview

```
                        👤 Admin / Developer
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Manager Node      │
                    │  ┌───────────────┐  │
                    │  │  Swarm API    │  │
                    │  │  Scheduler    │  │
                    │  │  Orchestrator │  │
                    │  └───────────────┘  │
                    └────────┬────────────┘
                             │ Assigns Tasks
           ┌─────────────────┼─────────────────┐
           ▼                 ▼                  ▼
  ┌────────────────┐ ┌──────────────┐ ┌──────────────┐
  │  Worker Node 1 │ │ Worker Node 2│ │ Worker Node 3│
  │  [🐳][🐳][🐳]  │ │ [🐳][🐳]    │ │ [🐳][🐳][🐳] │
  └────────────────┘ └──────────────┘ └──────────────┘
           │                 │                  │
           └─────────────────┴──────────────────┘
                             │
                    ┌────────▼────────┐
                    │  Shared STATE   │
                    │  (Raft Consensus│
                    │   across nodes) │
                    └─────────────────┘
```

---

> 🎯 **Conclusion:** Docker Swarm provides a complete toolkit to **create, manage, scale, update, rollback, configure, secure, and health-monitor** services across a distributed cluster — all through simple commands from the Manager Node.

> 📌 **Hierarchy:** `Swarm` → `Nodes` → `Services` → `Tasks` → `Containers`

      
