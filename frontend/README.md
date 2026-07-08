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

# Project Structure

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

sudo apt update
sudo apt upgrade -y

Install Git

sudo apt install git -y

Install Docker

sudo apt install docker.io -y

Enable Docker

sudo systemctl enable docker
sudo systemctl start docker

Allow current user to run Docker

sudo usermod -aG docker $USER
newgrp docker

Install Docker Compose

sudo apt install docker-compose-v2 -y

Verify installation

docker --version
docker compose version

# Clone Repository

git clone https://github.com/aakashrsethi39/Travel_Blog.git
cd Travel_Blog

# Build and Run

docker compose up --build -d [This creates new Docker images or else it will make use of the stored data in the cache]
docker ps

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

# Persistent Database Storage

MongoDB data is persisted using bind mounts.

volumes:
  - ./backend/data:/data

Database files remain available even if the container is restarted.

# Import Sample Data

After the MongoDB container is running, import the sample dataset.

docker exec -it 37de  mongoimport --db wanderlust --collection posts --file ./data/sample_posts.json --jsonArray

# Health Checks

Health checks are configured using Docker Compose.

Backend

healthcheck:
  test: ["CMD-SHELL","curl -f http://localhost:5000 || exit 1"]
  interval: 10s
  timeout: 5s
  retries: 5

MongoDB

healthcheck:
  test: ["CMD","mongosh","--eval","db.adminCommand('ping')"]
  interval: 10s
  timeout: 5s
  retries: 5

# Restart Policy

Containers automatically restart after failures.

restart: always

# Environment Variables

Frontend

VITE_API_PATH="http://13.233.124.176:5000"

Backend

MONGODB_URI="mongodb://mongodb/wanderlust"
CORS_ORIGIN="http://13.233.124.176:5173"

---

# Logging

Backend logs

docker logs backend

MongoDB logs

docker logs mongo

Frontend logs

docker logs frontend


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
