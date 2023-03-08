---
title: "Docker and Containerization"
author: "Vladimir Lelicanin - SAE Institute"
format:
  revealjs:
    theme: default
    title-slide-attributes:
        data-background-image: https://keystoneacademic-res.cloudinary.com/image/upload/f_auto/q_auto/g_auto/w_256/element/15/156456_sae.jpg
        data-background-position: top left
        data-background-repeat: no-repeat
        data-background-size: 100px
    transition: fade
    footer: "Vladimir Lelicanin - SAE Institute Belgrade"
    margin: 0.2
    auto-animate: true
    preview-links: auto
    link-external-newwindow: true
    scrollable: true
    embed-resources: false
    slide-level: 1
    chalkboard: true
    multiplex: true
    slide-number: true
    incremental: false
    logo: https://upload.wikimedia.org/wikipedia/commons/e/e2/SAE_Institute_Black_Logo.jpg
---


## Introduction



Docker is an open-source containerization platform that can package and run applications in a portable way. This presentation will provide an overview of Docker and containerization.

---

## Why Containerization?

Containerization offers a way to ensure that an application can run consistently across different environments. This consistency is achieved by packaging the application and its dependencies into a container that can be run on any machine.

---

## Docker Architecture

Docker uses a client/server architecture where the client communicates with the Docker daemon via a REST API. The Docker daemon manages the containers, images, networks, and volumes.

```bash
# Example docker command
docker run -it --rm ubuntu bash
```

Documentation link: <https://docs.docker.com/engine/reference/commandline/docker/>

---

## Docker Images

A Docker image is a read-only template that includes the application and its dependencies. An image can be used to run a container.

```Dockerfile
# Example Dockerfile for a Node.js application
FROM node:14
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```

Documentation link: <https://docs.docker.com/engine/reference/builder/>

---

## Docker Containers

A Docker container is a running instance of an image. A container is isolated from the host machine and other containers. A container can expose ports, volumes, and environment variables.

```bash
# Example docker command to create and run a container
docker run -d --name mycontainer -p 80:80 nginx
```

Documentation link: https://docs.docker.com/engine/reference/commandline/container/

---

## Docker Registry

Docker images can be stored in a registry for later use. Docker Hub is a public registry that contains many pre-built images. Private registries can also be used to store images.

```bash
# Example docker command to push an image to Docker Hub
docker login
docker tag myimage:latest username/myimage:latest
docker push username/myimage:latest
```

Documentation link: <https://docs.docker.com/engine/reference/commandline/push/>


---

## Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications. A Compose file defines the services, networks, and volumes that make up an application.

```yaml
# Example Docker Compose file for a web application with a database
services:
  web:
    build: .
    ports:
      - "5000:5000"
  db:
    image: postgres
```

Documentation link: <https://docs.docker.com/compose/compose-file/>

---

## Container Orchestration


Container orchestration tools like Kubernetes and Docker Swarm are used to manage containers at scale. These tools automate the deployment, scaling, and management of containerized applications.

```bash
# Example kubectl command to scale a deployment
kubectl scale deployment myapp --replicas=3
```

Documentation link: <https://kubernetes.io/docs/tutorials/kubernetes-basics/scale/scale-replication-controller/>

---

## Docker Security

Docker provides several security features including namespaces, cgroups, and AppArmor. Docker also supports image and container signing to ensure integrity.

```bash
# Example docker command to sign an image
docker trust sign myimage:latest
```

Documentation link: <https://docs.docker.com/engine/security/>

---

## Comparison with Virtual Machines

Docker containers are lightweight compared to virtual machines because they share the host operating system kernel. Containers are also more portable because they do not require a hypervisor.

---

## Use Cases

Docker is used in many industries and applications including web development, data science, and DevOps. Docker can also be used for testing and building applications.


---

## Contributions

Docker is an open-source platform that welcomes contributions from the community. Contributions can be made to Docker itself, as well as to the many Docker images and tools on Docker Hub.

Documentation link: <https://docs.docker.com/community/>


---

## Conclusion

Docker is a powerful tool for containerizing applications. Containerization offers consistency, portability, and scalability. Docker is used in many industries and applications, and is contributing to the continued growth of technology.

---

## References

1. Docker documentation: <https://docs.docker.com/>
2. Docker Hub: <https://hub.docker.com/>
3. Kubernetes documentation: <https://kubernetes.io/docs/home/>
4. Docker Security: <https://docs.docker.com/engine/security/>
5. Open-source contributions: <https://docs.docker.com/community/>
