# 🐳 Docker Overview

## 📘 Part 1: Docker Basics

**Concept:**
Docker is a platform that helps you build, ship, and run applications inside containers. Containers are lightweight, portable, and run consistently across environments.

**Core Components:**

* **Containers:** Run applications in isolated environments.
* **Images:** Blueprints used to create containers.
* **Registries:** Repositories (like Docker Hub) for storing and sharing images.

**Basic Commands:**

```bash
docker run <image_name>      # Run a container
docker ps                    # List running containers
docker stop <id>             # Stop a container
docker rm <id>               # Remove a container
docker build -t <name> .     # Build image from Dockerfile
docker pull <image_name>     # Pull image from registry
```

**🧪 Practical Work:**

* Install Docker
* Run an Nginx container:

  ```bash
  docker run -d -p 8080:80 nginx
  ```

**🎯 Assignment:**
Pull and run an Nginx container from Docker Hub, then verify at `http://localhost:8080`.

---

## 📘 Part 2: Docker Advanced

**Concepts:**

* **Dockerfile:** Defines steps to build custom images.
* **Volumes:** Persist data beyond container lifecycle.
* **Networking:** Enables communication between containers.

**Dockerfile Example:**

```dockerfile
FROM python:3.8-slim
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
EXPOSE 80
CMD ["python", "app.py"]
```

**🧪 Practical Work:**
Build and run a Python app:

```bash
docker build -t python-app .
docker run -d -p 5000:80 python-app
```

**🎯 Assignment:**
Containerize a web app (Flask/Node.js) using a custom Dockerfile and run it locally.

---

## 📘 Part 3: Docker Compose

**Concept:**
Docker Compose manages multi-container applications using a single `docker-compose.yml` file.

**Example:**

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
      MYSQL_ROOT_PASSWORD: example
```

**🧪 Practical Work:**
Run multi-container setup (WordPress + MySQL):

```bash
docker-compose up -d
```

**🎯 Assignment:**
Create a WordPress site with MySQL using Docker Compose and verify at `http://localhost:8080`.

---

## ✅ Summary

* **Docker Basics:** Containers, Images, Registries
* **Docker Advanced:** Dockerfile, Volumes, Networking
* **Docker Compose:** Manage multi-container apps easily
  Docker simplifies app deployment, improves consistency, and makes environment setup automated and reproducible.

