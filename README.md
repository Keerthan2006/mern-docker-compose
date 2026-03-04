# Containerizing MERN Stack using Docker and Docker Compose

## Project Overview

This project demonstrates how to **containerize a MERN (MongoDB, Express, React, Node.js) stack application** using Docker and Docker Compose.

The implementation was done in **two stages**.

First, the application was containerized and tested **locally using three separate containers**:

* Frontend container
* Backend container
* MongoDB container

Each container was built and started manually using **Docker build and Docker run commands**.

After verifying that the application worked correctly, **Docker Compose** was introduced to simplify the management of the multi-container setup.

Docker Compose allows the entire application stack to be started with a **single command** and handles networking, volumes, and service orchestration automatically.

---

# Architecture

```
User Browser
      |
      v
Frontend (React + Nginx)
      |
      v
Backend (Node.js + Express)
      |
      v
MongoDB Database
```

Each component runs in its **own Docker container** and communicates through a **Docker network**.

---

# Initial Implementation (Without Docker Compose)

The application was first executed using **three independent containers**.

Steps performed:

* Built a Docker image for the **React frontend** using a **multi-stage Docker build**.
* Used **Nginx** in the runtime stage to serve the React production build.
* Built a Docker image for the **Node.js backend**.
* Pulled the **MongoDB official image**.
* Created a **custom Docker bridge network** so all containers could communicate with each other.

Example workflow:

```
docker build -t mern-frontend .
docker build -t mern-backend .

docker network create compose
docker volume create mongo-data

docker run --network=compose -p 5173:80 mern-frontend
docker run --network=compose -p 5050:5050 mern-backend
docker run --network=compose -v mongo-data:/data/db -p 27017:27017 mongo
```

o

This setup allowed the containers to communicate with each other within the same network.

---

# Docker Compose Implementation

After confirming that the application works correctly with manually started containers, **Docker Compose** was introduced.

Docker Compose simplifies container management by defining all services inside a **single configuration file**.

Using Docker Compose, the following components are defined:

* Frontend service
* Backend service
* MongoDB service
* Custom network
* Persistent volume for MongoDB

This allows the entire application stack to be started with a single command.

---

# Networking

A **custom bridge network** is used so all services can communicate internally.

```
frontend  <---->  backend  <---->  mongodb

---

# Volumes

MongoDB uses a **named Docker volume** to store database data.

```
mongo-data:/data/db
```

Benefits of using volumes:

* Data persists even if containers stop
* Database data is not lost when containers are recreated
* Provides persistent storage for the database

---

# Running the Application

Start all containers:

```
docker compose up --build
```

This command will:

* Build frontend and backend images
* Pull the MongoDB image
* Create the Docker network
* Create the MongoDB volume
* Start all containers

---

# Access the Application

Frontend

```
http://localhost:5173
```

Backend API

```
http://localhost:5050
```

MongoDB

```
mongodb://localhost:27017
```

---

# Stopping the Application

Stop and remove containers:

```
docker compose down
```

Stop containers and remove volumes:

```
docker compose down -v
```

---

# Key Docker Concepts Demonstrated

* Dockerfile creation
* Multi-stage Docker builds
* Container networking
* Docker volumes for persistent storage
* Running containers manually with `docker run`
* Multi-container orchestration using Docker Compose

---

# Author

Keerthan

