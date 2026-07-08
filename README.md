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
git clone https://github.com/aakashrsethi39/Travel_Blog.git

cd Travel_Blog
```

---

# Build and Run

```bash
docker compose up --build -d [this will create a new image or else it will use cache ]
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

```bash
docker exec -it 7f  mongoimport --db wanderlust --collection posts --file ./data/sample_posts.json --jsonArray
```
```
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/ddbafe32-b337-4e6d-80c4-fc1e7baf4084" />

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
VITE_API_PATH="http://<EC2_PUBLIC_IP>:5000"  
```

Backend

```
MONGODB_URI="mongodb://mongodb/wanderlust" [mongo container name]
CORS_ORIGIN="http://<EC2_PUBLIC_IP>:5173"  [if u get the cors error]

<img width="940" height="500" alt="image" src="https://github.com/user-attachments/assets/d0ee9dab-d1ff-4adc-ba7d-af4a0366e779" />

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

# AWS Security Group

Allow the following inbound rules

| Port | Purpose |
|-------|----------|
| 22 | SSH |
| 5000 | Backend API |
| 5173 | Frontend (Development) |

MongoDB (27017) should remain private and should **not** be exposed publicly.

---

# Access Application

Frontend

```
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/ca273d87-bc7b-4618-8f4d-eb85d3c3118a" />

```
```
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/6e5a8c29-ed03-420d-b667-00c1b62c0f6f" />
```
```
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/bafb1773-ea25-4386-88cd-02914213ccd1" />

```

Backend API

```
<img width="940" height="233" alt="image" src="https://github.com/user-attachments/assets/ada29365-9e11-47f1-b59f-15a985083df3" />

```
```
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/6d56e104-fe64-4a77-8bd5-33afa6ee3e1c" />

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
