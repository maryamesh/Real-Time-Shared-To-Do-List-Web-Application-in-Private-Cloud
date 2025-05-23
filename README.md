# Real-Time-Shared-To-Do-List-Web-Application-in-Private-Cloud
## Overview

This project is a **real-time, collaborative to-do list web application** designed to run within a private cloud environment. It enables multiple users on different devices to create, update, and synchronize tasks instantly, providing seamless collaboration without page reloads.

The system leverages **containerized microservices orchestrated by Docker Swarm** to ensure scalability, fault tolerance, and high availability within a single VM setup. Core technologies include:

- **Flask** for backend API and real-time logic  
- **Socket.IO** for bidirectional, event-driven communication between clients and servers  
- **Redis** as a centralized message broker using the Pub/Sub pattern for synchronizing backend replicas  
- **NGINX** as a reverse proxy and load balancer for HTTP and WebSocket traffic  

---

## Table of Contents

- [Features](#features)  
- [Architecture](#architecture)  
- [Prerequisites](#prerequisites)  
- [Installation and Setup](#installation-and-setup)  
- [Usage](#usage)  
- [How It Works](#how-it-works)  
- [Fault Tolerance and Scalability](#fault-tolerance-and-scalability)  
- [Future Work](#future-work)  
- [Contributing](#contributing)  
- [License](#license)  

---

## Features

- Real-time task creation, update, and deletion with instant synchronization across multiple clients  
- Multi-replica backend services managed by Docker Swarm for fault tolerance  
- Load balancing of incoming HTTP and WebSocket requests via NGINX  
- Centralized Redis Pub/Sub for event broadcasting between backend replicas  
- Scalable architecture supporting thousands of concurrent WebSocket connections using Eventlet  
- Accessible through a single IP address within a private network  

---

## Architecture

The application consists of the following components running as Docker containers:

| Component        | Role                                                    | Number of Replicas |
|------------------|---------------------------------------------------------|--------------------|
| **NGINX**        | Reverse proxy and load balancer for HTTP/WebSocket traffic | 3                  |
| **Flask Backend**| Handles task management, WebSocket connections, and real-time event handling | 3                  |
| **Redis**        | In-memory data store and Pub/Sub message broker to synchronize backend replicas | 1                  |
| **Socket.IO**    | Enables real-time bidirectional communication between clients and backend | Integrated in Flask backend |

All containers are orchestrated using Docker Swarm on a single VM, which acts as a private cloud node.

---

## Prerequisites

- A Linux-based server or VM with Docker and Docker Swarm installed  
- Docker Compose (optional, for local testing)  
- Basic understanding of Docker and command line interface  

---

## Installation and Setup

### 1. Clone the Repository

bash
git clone https://github.com/yourusername/real-time-todo-app.git
cd real-time-todo-app
2. Build Docker Images (if applicable)
bash
Copy
docker build -t yourusername/flask-backend ./backend
docker build -t yourusername/nginx ./nginx
docker pull redis:latest
3. Initialize Docker Swarm (if not already done)
bash
Copy
docker swarm init
4. Deploy the Stack
bash
Copy
docker stack deploy -c docker-compose.yml realtime-todo
This command starts all services defined in the docker-compose.yml file, including NGINX, Flask backend replicas, and Redis.

5. Access the Application
Open a web browser and navigate to:

cpp
Copy
http://<YOUR_VM_IP_ADDRESS>/
Multiple users on the same network can access and collaborate on the to-do list in real-time.

Usage
Add tasks via the web interface.

Tasks will appear immediately for all connected users.

Edit or delete tasks, and changes will synchronize in real-time.

The system supports multiple backend replicas ensuring continuous availability.

How It Works
Client Interaction: Users interact through a browser UI connected via Socket.IO for real-time communication.

NGINX Reverse Proxy: NGINX receives incoming requests and forwards them to one of several Flask backend containers, distributing load evenly.

Backend Processing: Flask backend containers handle task operations and broadcast updates to all other containers via Redis Pub/Sub.

Redis Pub/Sub: Acts as a central event bus synchronizing all backend replicas.

Real-Time Updates: Backend sends real-time updates back to clients over Socket.IO connections.

Fault Tolerance and Scalability
Docker Swarm automatically restarts containers if they fail, maintaining service continuity.

Multiple replicas of backend and proxy services prevent single points of failure.

Load balancing optimizes resource utilization and responsiveness.

Eventlet allows Flask to handle thousands of concurrent WebSocket connections efficiently.

Future Work
Implement secure user authentication and access control

Develop mobile applications for broader accessibility

Add real-time analytics and monitoring dashboards

Enable multi-node Docker Swarm clusters for true high availability

Enhance security with end-to-end encryption
