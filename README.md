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

<img width="1122" height="631" alt="image" src="https://github.com/user-attachments/assets/e695aa04-d180-4cc4-b450-71435fcced81" />


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

```
<img width="889" height="632" alt="image" src="https://github.com/user-attachments/assets/f322fa35-752f-48c4-93af-e71cd1c4f073" />

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
```
Frontend
```
<img width="1125" height="633" alt="image" src="https://github.com/user-attachments/assets/512b044c-dd02-4340-b227-bbc07d922fea" />


<img width="1143" height="643" alt="image" src="https://github.com/user-attachments/assets/f0089914-def3-499c-9ce3-b37faf956fe5" />


<img width="1222" height="688" alt="image" src="https://github.com/user-attachments/assets/02d7fbbf-614b-4d78-86b2-a12b6acaf023" />


```
Backend API
```

<img width="1022" height="253" alt="image" src="https://github.com/user-attachments/assets/93687c7c-f19f-43d0-aa8a-ad564240d1a4" />


<img width="1132" height="637" alt="image" src="https://github.com/user-attachments/assets/e0660e9a-6195-450e-b853-81dcf5f5c962" />


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
