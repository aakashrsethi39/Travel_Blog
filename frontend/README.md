# 🌍 Travel Blog - Dockerized Full Stack Application

## 📌 Project Overview

Travel Blog is a full-stack web application containerized using Docker and deployed on AWS EC2.

### Technology Stack

- **Frontend:** React + Vite
- **Backend:** Node.js + Express.js
- **Database:** MongoDB
- **Containerization:** Docker
- **Orchestration:** Docker Compose
- **Cloud Platform:** AWS EC2
- **Database Persistence:** Docker Volumes

---

# Project Structure

```
Travel_Blog/
│
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   └── ...
│
├── backend/
│   ├── Dockerfile
│   ├── data/
│   │     └── sample_posts.json
│   ├── package.json
│   └── ...
│
├── docker-compose.yml
└── README.md
```

---

# Architecture

```
                Internet
                    │
                    │
            AWS Security Group
                    │
          Public IP (EC2 Instance)
                    │
         ┌──────────────────────────┐
         │     Docker Compose       │
         │                          │
         │   Frontend (React/Vite)  │
         │           │              │
         │           ▼              │
         │ Backend (Node/Express)   │
         │           │              │
         │           ▼              │
         │      MongoDB             │
         └──────────────────────────┘
```

---

# Prerequisites

- AWS Account
- Ubuntu EC2 Instance
- Git
- Docker
- Docker Compose

---

# EC2 Setup

Update packages

```bash
sudo apt update
sudo apt upgrade -y
```

Install Git

```bash
sudo apt install git -y
```

Install Docker

```bash
sudo apt install docker.io -y
```

Enable Docker

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Allow current user to run Docker

```bash
sudo usermod -aG docker $USER
newgrp docker
```

Install Docker Compose

```bash
sudo apt install docker-compose-v2 -y
```

Verify installation

```bash
docker --version
docker compose version
```

---

# Clone Repository

```bash
git clone <repository-url>

cd Travel_Blog
```

---

# Build and Run

```bash
docker compose up --build -d
```

Check containers

```bash
docker ps
```

---

# Docker Compose Features

The project uses Docker Compose for:

- Multi-container deployment
- Automatic networking
- Persistent storage
- Environment variables
- Restart policies
- Health checks

---

# Docker Network

All services communicate over an internal Docker network.

Instead of localhost, containers communicate using service names.

Example

```
mongodb://mongo:27017/wanderlust
```

---

# Persistent Database Storage

MongoDB data is persisted using bind mounts.

```yaml
volumes:
  - ./backend/data:/data
```

Database files remain available even if the container is restarted.

---

# Import Sample Data

After the MongoDB container is running, import the sample dataset.

Execute:

```bash
docker exec -it <mongodb-container-name> bash
```

Run:

```bash
mongoimport \
  --db wanderlust \
  --collection posts \
  --file /data/sample_posts.json \
  --jsonArray
```

Verify:

```bash
mongosh
```

```javascript
use wanderlust

db.posts.find()
```

---

# Health Checks

Health checks are configured using Docker Compose.

Backend

```yaml
healthcheck:
  test: ["CMD-SHELL","curl -f http://localhost:5000 || exit 1"]
  interval: 10s
  timeout: 5s
  retries: 5
```

MongoDB

```yaml
healthcheck:
  test: ["CMD","mongosh","--eval","db.adminCommand('ping')"]
  interval: 10s
  timeout: 5s
  retries: 5
```

---

# Restart Policy

Containers automatically restart after failures.

```yaml
restart: always
```

---

# Environment Variables

Frontend

```
VITE_API_PATH=http://<EC2_PUBLIC_IP>:5000
```

Backend

```
ATLASDB_URL=mongodb://mongo:27017/wanderlust

PORT=5000
```

---

# Logging

View all logs

```bash
docker compose logs
```

Live logs

```bash
docker compose logs -f
```

Backend logs

```bash
docker logs backend
```

MongoDB logs

```bash
docker logs mongo
```

Frontend logs

```bash
docker logs frontend
```

---

# Useful Docker Commands

Build

```bash
docker compose build
```

Start

```bash
docker compose up -d
```

Stop

```bash
docker compose down
```

Restart

```bash
docker compose restart
```

List containers

```bash
docker ps
```

Remove unused Docker resources

```bash
docker system prune -a
```

---

# AWS Security Group

Allow the following inbound rules

| Port | Purpose |
|-------|----------|
| 22 | SSH |
| 5000 | Backend API |
| 5173 | Frontend (Development) |
| 80 | Nginx (Optional) |

MongoDB (27017) should remain private and should **not** be exposed publicly.

---

# Access Application

Frontend

```
http://<EC2_PUBLIC_IP>:5173
```

Backend API

```
http://<EC2_PUBLIC_IP>:5000
```

---

# Troubleshooting

## Docker Build Fails

Remove Docker cache

```bash
docker system prune -a
docker builder prune -a
```

---

## Disk Full

Check disk usage

```bash
df -h
```

Increase EBS storage if required.

Expand filesystem

```bash
sudo growpart /dev/nvme0n1 1
sudo resize2fs /dev/nvme0n1p1
```

---

## MongoDB Connection Error

Ensure the backend connects using the Docker Compose service name.

Correct

```
mongodb://mongo:27017/wanderlust
```

Incorrect

```
mongodb://localhost:27017/wanderlust
```

---

# Assignment Objectives Covered

- Dockerized full-stack application
- Separate Dockerfiles for frontend and backend
- Official MongoDB Docker image
- Docker Compose orchestration
- Internal Docker networking
- Persistent MongoDB storage
- Health checks
- Automatic container restart
- Logging
- AWS EC2 deployment
- Security Group configuration
- Sample MongoDB data import
- Clean and documented deployment process

---

# Author

**Aakash Sethi**
