# Softy Pinko Docker Project

A comprehensive Docker containerization project that progressively builds a multi-container web application with Flask backend, Nginx frontend, and advanced features like reverse proxy and load balancing.

## Project Structure

```
holbertonschool-softy-pinko-docker/
├── task0/
├── task1/
├── task2/
├── task3/
├── task4/
├── task5/
└── task6/
```

## Prerequisites

- Docker installed and running
- Docker Compose installed
- Git for cloning repositories
- Basic understanding of Docker, Python Flask, and Nginx

## Task Overview

### Task 0: Create Your First Docker Image

**Objective:** Build a basic Docker image that outputs "Hello, World!"

**Files:**
- `task0/Dockerfile`

**Key Concepts:**
- Basic Dockerfile structure
- Using Ubuntu base image
- Running shell commands with CMD

**Commands:**
```bash
cd task0
docker build -f ./Dockerfile -t softy-pinko:task0 .
docker run -it --rm --name softy-pinko-task0 softy-pinko:task0
```

**Expected Output:** "Hello, World!"

---

### Task 1: Python Flask API Server

**Objective:** Create a Flask API server running in Docker

**Files:**
- `task1/Dockerfile`
- `task1/api.py`

**Key Concepts:**
- Installing Python and pip in Docker
- Handling Python's EXTERNALLY-MANAGED environment
- Setting up a working directory
- Exposing ports for network access

**Commands:**
```bash
cd task1
docker build -f ./Dockerfile -t softy-pinko:task1 .
docker run -p 5252:5252 -it --rm --name softy-pinko-task1 softy-pinko:task1
```

**Test:** Navigate to `http://localhost:5252/api/hello`

---

### Task 2: Frontend with Nginx

**Objective:** Serve a static frontend using Nginx and organize project structure

**Directory Structure:**
```
task2/
├── back-end/
│   ├── Dockerfile
│   └── api.py
└── front-end/
    ├── Dockerfile
    ├── softy-pinko-front-end.conf
    └── softy-pinko-front-end/
        └── (cloned repository files)
```

**Key Concepts:**
- Using Nginx base image
- Configuring Nginx for static content
- Project reorganization

**Commands:**
```bash
cd task2
# Clone the frontend repository
cd front-end
git clone https://github.com/atlas-school/softy-pinko-front-end

# Build and run frontend
docker build -f ./front-end/Dockerfile -t softy-pinko-front-end:task2 ./front-end
docker run -p 9000:9000 -it --rm --name softy-pinko-front-end-task2 softy-pinko-front-end:task2
```

**Test:** Navigate to `http://localhost:9000`

---

### Task 3: Connecting Frontend and Backend

**Objective:** Enable communication between frontend and backend containers using CORS

**Key Changes:**
- Added `flask-cors` to backend
- Modified `index.html` to include dynamic content
- Added AJAX script to fetch data from backend

**Files Modified:**
- `task3/back-end/Dockerfile` - Added flask-cors installation
- `task3/back-end/api.py` - Enabled CORS
- `task3/front-end/softy-pinko-front-end/index.html` - Added dynamic content and AJAX

**Commands:**
```bash
cd task3
# Terminal 1 - Backend
docker build -f ./back-end/Dockerfile -t softy-pinko-back-end:task3 ./back-end
docker run -p 5252:5252 -it --rm --name softy-pinko-back-end-task3 softy-pinko-back-end:task3

# Terminal 2 - Frontend
docker build -f ./front-end/Dockerfile -t softy-pinko-front-end:task3 ./front-end
docker run -p 9000:9000 -it --rm --name softy-pinko-front-end-task3 softy-pinko-front-end:task3
```

**Test:** Navigate to `http://localhost:9000` - "Hello, World!" should appear on the page

---

### Task 4: Docker Compose Orchestration

**Objective:** Simplify multi-container management with Docker Compose

**New File:**
- `task4/docker-compose.yml`

**Key Concepts:**
- Service definitions
- Port mapping
- Service dependencies
- Networking between containers

**Commands:**
```bash
cd task4
docker-compose build
docker-compose up
```

**Benefits:**
- Single command to start all services
- Automatic network creation
- Service dependency management

---

### Task 5: Reverse Proxy Implementation

**Objective:** Add a proxy server as a single entry point for all services

**New Components:**
```
task5/
└── proxy/
    ├── Dockerfile
    └── proxy.conf
```

**Key Changes:**
- Only proxy exposes ports to host
- Frontend and backend use `expose` instead of `ports`
- JavaScript uses relative URLs (`/api/hello`)
- Nginx routes traffic based on URL paths

**Docker Compose Configuration:**
- Proxy: Port 80 (or 8080) exposed to host
- Frontend: Internal port 9000
- Backend: Internal port 5252

**Commands:**
```bash
cd task5
docker-compose build
docker-compose up
```

**Test:** 
- `http://localhost` - Should work (through proxy)
- `http://localhost:9000` - Should NOT work (not exposed)
- `http://localhost:5252` - Should NOT work (not exposed)

---

### Task 6: Load Balancing with Multiple API Servers

**Objective:** Scale the backend service with automatic load balancing

**New File:**
- `task6/2-api-servers.txt` - Contains the scaling command

**Key Concept:**
- Docker Compose scaling with `--scale` flag
- Nginx automatic round-robin load balancing

**Commands:**
```bash
cd task6
docker-compose build

# Run with 2 backend instances
docker-compose up --scale back-end=2

# Or run with 5 backend instances
docker-compose up --scale back-end=5
```

**Test:** 
- Refresh `http://localhost` multiple times
- Watch terminal logs to see requests distributed across backend instances

---

## Common Issues and Solutions

### Port Already in Use
```bash
# Check what's using the port
sudo lsof -i :PORT_NUMBER

# Stop all Docker containers
docker stop $(docker ps -q)
```

### ContainerConfig Error
```bash
# Clean up and rebuild
docker-compose down -v
docker-compose build --no-cache
docker-compose up
```

### Permission Denied
```bash
# Add user to docker group
sudo usermod -aG docker $USER
newgrp docker
```

## Project Learning Outcomes

1. **Docker Basics:** Creating Dockerfiles, building images, running containers
2. **Multi-stage Development:** Progressive complexity from single container to multi-container
3. **Networking:** Container communication, port mapping, internal networks
4. **Web Technologies:** Flask API development, Nginx configuration, CORS
5. **Orchestration:** Docker Compose for multi-container management
6. **Architecture Patterns:** Reverse proxy, load balancing, microservices
7. **Scaling:** Horizontal scaling of services with automatic load distribution

## Next Steps

- Implement health checks for containers
- Add environment variable configuration
- Implement persistent data volumes
- Add container restart policies
- Explore Kubernetes for production orchestration
- Implement SSL/TLS with Let's Encrypt
- Add monitoring and logging solutions

## Author

Fernando Lockwood

