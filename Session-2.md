# Docker Fundamentals - Training Session
### Beginner Friendly Session Notes

---

# Session Agenda

1. What is Docker?
2. Why Docker?
3. Problems Before Docker
4. Virtual Machines vs Docker
5. Docker Architecture
6. Docker Components
7. Docker Installation
8. Docker Images
9. Docker Containers
10. Docker Commands
11. Dockerfile
12. Docker Volumes
13. Docker Networking
14. Docker Compose
15. Best Practices
16. Live Demo
17. Q&A

---

# 1. What is Docker?

## Definition

**Docker** is an open-source containerization platform that allows developers to package an application along with all its dependencies into a lightweight, portable unit called a **Container**.

A Docker container can run consistently on any machine, whether it's a developer's laptop, a testing server, or a production environment.

---

# Why Docker?

Imagine you developed an application on your laptop.

It works perfectly on your machine.

But when you deploy it to another server...

❌ Application crashes

❌ Missing libraries

❌ Different OS configuration

❌ Different software versions

This problem is commonly known as:

> **"It works on my machine!"**

Docker solves this problem by packaging everything required to run the application.

---

# Problems Before Docker

Traditional Deployment

```
Application

↓

Operating System

↓

Required Libraries

↓

Java / Python / NodeJS

↓

Database Drivers

↓

Configurations
```

Every server had different configurations.

Result:

- Dependency Issues
- Version Conflicts
- Deployment Failures
- Environment Mismatch

---

# Docker Solution

Docker packages

- Application
- Runtime
- Libraries
- Dependencies
- Configuration

into one single package called

## Container

Now the application behaves the same everywhere.

---

# Real Life Analogy

Imagine shipping goods.

Without Containers

- Different sized boxes
- Difficult transportation
- Damage risk

With Shipping Containers

- Standard Size
- Easy Transport
- Portable Anywhere

Docker Containers work the same way.

Applications become portable.

---

# Virtual Machine vs Docker

## Virtual Machine

```
Hardware

↓

Host OS

↓

Hypervisor

↓

Guest OS

↓

Application
```

Every VM has its own Operating System.

Heavy

Slow

Consumes More RAM

---

## Docker

```
Hardware

↓

Host OS

↓

Docker Engine

↓

Containers

↓

Application
```

Containers share the Host Operating System.

Lightweight

Fast

Less Memory

---

# Virtual Machine vs Docker

| Virtual Machine | Docker |
|----------------|---------|
| Heavy | Lightweight |
| Own OS | Shared OS |
| Slow Startup | Starts in Seconds |
| High Memory Usage | Low Memory Usage |
| Large Size | Small Size |
| Less Portable | Highly Portable |

---

# Docker Architecture

```
Docker Client

       ↓

Docker Daemon

       ↓

Docker Engine

       ↓

Images

       ↓

Containers
```

---

# Docker Components

## Docker Engine

Runs Docker Containers.

---

## Docker Client

Used to execute Docker commands.

Example

```
docker run nginx
```

---

## Docker Image

Blueprint of an application.

Read Only.

Example

Ubuntu Image

Python Image

Nginx Image

MySQL Image

---

## Docker Container

Running instance of an Image.

Example

```
Image

↓

Container Running
```

---

## Docker Hub

Public repository of Docker Images.

Popular Images

- Ubuntu
- Nginx
- Redis
- MySQL
- MongoDB
- Python
- Node

---

# Docker Workflow

```
Dockerfile

      ↓

Docker Build

      ↓

Docker Image

      ↓

Docker Run

      ↓

Container
```

---

# Installing Docker

Windows

- Install Docker Desktop

Linux

```bash
sudo apt update

sudo apt install docker.io
```

Verify

```bash
docker --version
```

---

# Docker Images

Download an image

```bash
docker pull nginx
```

List images

```bash
docker images
```

Delete image

```bash
docker rmi nginx
```

---

# Docker Containers

Run container

```bash
docker run nginx
```

Run in background

```bash
docker run -d nginx
```

Run with custom name

```bash
docker run --name web nginx
```

Run on specific port

```bash
docker run -d -p 8080:80 nginx
```

---

# Useful Docker Commands

Check running containers

```bash
docker ps
```

Check all containers

```bash
docker ps -a
```

Stop container

```bash
docker stop container_name
```

Start container

```bash
docker start container_name
```

Restart container

```bash
docker restart container_name
```

Delete container

```bash
docker rm container_name
```

---

# Container Logs

View logs

```bash
docker logs container_name
```

Follow logs

```bash
docker logs -f container_name
```

---

# Execute Commands Inside Container

Open shell

```bash
docker exec -it container_name bash
```

or

```bash
docker exec -it container_name sh
```

---

# Dockerfile

A Dockerfile is a text file that contains instructions to build a Docker Image.

---

# Sample Dockerfile

```dockerfile
FROM nginx:latest

COPY . /usr/share/nginx/html

EXPOSE 80
```

---

# Dockerfile Instructions

## FROM

Base Image

```dockerfile
FROM ubuntu
```

---

## WORKDIR

Working directory

```dockerfile
WORKDIR /app
```

---

## COPY

Copy files

```dockerfile
COPY . .
```

---

## RUN

Execute command during image build

```dockerfile
RUN apt update
```

---

## CMD

Default command

```dockerfile
CMD ["python","app.py"]
```

---

## EXPOSE

Expose container port

```dockerfile
EXPOSE 80
```

---

# Build Docker Image

```bash
docker build -t myapp .
```

---

# Run Docker Image

```bash
docker run -d -p 8080:80 myapp
```

---

# Docker Volumes

Problem

Containers are temporary.

If container is deleted,

Data is lost.

Solution

Docker Volumes

Volumes store data outside the container.

---

Create volume

```bash
docker volume create myvolume
```

List volumes

```bash
docker volume ls
```

Use volume

```bash
docker run -v myvolume:/data nginx
```

---

# Docker Networking

Containers communicate using Docker Networks.

Types

- Bridge (Default)
- Host
- None
- Overlay

Create Network

```bash
docker network create mynetwork
```

List Networks

```bash
docker network ls
```

---

# Docker Compose

Docker Compose is used to run multiple containers together using a YAML file.

Example

Application

↓

Web Server

↓

Database

↓

Redis

Instead of multiple commands,

Run one command.

---

# Sample docker-compose.yml

```yaml
version: '3'

services:

  web:
    image: nginx
    ports:
      - "8080:80"

  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: password
```

Run

```bash
docker compose up -d
```

Stop

```bash
docker compose down
```

---

# Docker Image Lifecycle

```
Dockerfile

↓

Build Image

↓

Push to Docker Hub

↓

Pull Image

↓

Run Container
```

---

# Best Practices

- Keep images small.
- Use official base images.
- Avoid running containers as root.
- Use .dockerignore.
- Store secrets securely.
- Use volumes for persistent data.
- Tag images properly.
- Remove unused images regularly.

---

# Real World Example

A Java Application

Requires

- Java 17
- Maven
- Spring Boot
- MySQL

Without Docker

Install everything manually.

With Docker

Run one command.

Application works instantly.

---

# Advantages of Docker

- Fast Deployment
- Lightweight
- Portable
- Consistent Environment
- Easy Scaling
- Efficient Resource Usage
- Supports CI/CD
- Simplifies Microservices

---

# Docker vs Traditional Deployment

| Traditional | Docker |
|--------------|---------|
| Manual Setup | Automated |
| Environment Issues | Consistent Environment |
| Heavy Deployment | Lightweight |
| Slow | Fast |
| Hard to Scale | Easy Scaling |
| Dependency Conflicts | Bundled Dependencies |

---

# Common Docker Commands Cheat Sheet

```bash
docker --version

docker pull nginx

docker images

docker run nginx

docker run -d nginx

docker ps

docker ps -a

docker stop container

docker start container

docker restart container

docker rm container

docker rmi image

docker logs container

docker exec -it container bash

docker build -t myapp .

docker compose up -d

docker compose down
```

---

# Live Demo Flow

## Demo 1

Install Docker

```bash
docker --version
```

---

## Demo 2

Pull Nginx Image

```bash
docker pull nginx
```

---

## Demo 3

Run Nginx Container

```bash
docker run -d -p 8080:80 --name nginx-demo nginx
```

Open Browser

```
http://localhost:8080
```

Nginx Welcome Page appears.

---

## Demo 4

View Running Containers

```bash
docker ps
```

---

## Demo 5

View Logs

```bash
docker logs nginx-demo
```

---

## Demo 6

Access Container

```bash
docker exec -it nginx-demo bash
```

---

## Demo 7

Build Custom Docker Image

Create Dockerfile

```dockerfile
FROM nginx

COPY . /usr/share/nginx/html
```

Build

```bash
docker build -t mywebsite .
```

Run

```bash
docker run -d -p 9090:80 mywebsite
```

Open

```
http://localhost:9090
```

---

# Key Takeaways

- Docker is a containerization platform.
- Containers package applications with all dependencies.
- Docker solves the "Works on My Machine" problem.
- Images are templates; Containers are running instances.
- Dockerfile defines how to build images.
- Docker Compose helps manage multi-container applications.
- Volumes provide persistent storage.
- Docker is a key tool in modern DevOps and CI/CD pipelines.

---

# Thank You!

## Questions?

**Happy Containerizing! 🐳**
