# Docker Interview Questions and Answers

## Q1: What is Docker and how do you use it in your project?

Docker is a platform that allows us to package applications and their dependencies into lightweight, portable containers. It ensures consistency across different environments — whether it's development, testing, or production — by running the application the same way everywhere."

We use Docker to containerize all our services and manage them consistently across environments. It's deeply integrated into our Jenkins-based CI/CD pipeline — from build to image scanning to GitOps-based deployment with ArgoCD on AWS EKS. It makes our delivery process faster, more reliable, and easier to scale."

## Q2: What is the difference between Docker image and container?

A Docker image is a lightweight, standalone, executable package that includes everything needed to run a piece of software, including the code, runtime, libraries, and dependencies. A container is a running instance of an image, isolated from the host system and other containers.

## Q3: What is Docker architecture?

Docker architecture consists of three main components: the Docker client, the Docker daemon, and the Docker registry. The client communicates with the daemon, which manages Docker objects like images, containers, networks, and volumes. The registry stores Docker images.

## Q4: How is Docker operable on a Linux machine?

Docker runs natively on Linux using the host's kernel. It leverages Linux features like cgroups and namespaces to provide isolation and resource management for containers.

## Q5: Did you build the Dockerfile using CodeBuild?

Yes, I used AWS CodeBuild to automate the building of Docker images from Dockerfiles, ensuring consistent and reproducible builds in my CI/CD pipeline.

## Q6: Explain how to write a Dockerfile.

A Dockerfile is a script that contains step-by-step instructions on how to build a Docker image. It defines the base image, environment, dependencies, and how the container should run."

## Q7: What will happen if the Docker image has port 8080 and container/application has some other port?

If the container exposes a different port than the image, you need to map the container's port to the host's port using the -p flag when running the container. For example, docker run -p 8080:3000 maps the container's port 3000 to the host's port 8080.

## Q8: How is the image pulled from a private repository?

To pull an image from a private registry, you first authenticate using docker login or a registry-specific CLI like AWS CLI. This creates a session token locally. Then the image is pulled as normal. In Kubernetes or Jenkins pipelines, you manage this with imagePullSecrets or credentials securely passed into the build environment."

## Q9: From where is the image pulled when you use docker pull?

When you use docker pull <image>, Docker pulls the image from a container registry — which is a storage and distribution system for container images. If no registry is specified, Docker pulls from Docker Hub by default.

## Q10: Explain Docker networking and types of network. What is the default network?

Docker networking enables communication between containers, the host, and external networks (like the internet). Docker automatically sets up networking for containers using different network drivers.
Types of Network:
- Bridge (default)
- Host
- None
- Overlay (Swarm only)
- Macvlan
The default network is Bridge

## Q11: What is the difference between Docker bind mount and volume?

Docker supports two main types of persistent storage: volumes and bind mounts.

Volumes are fully managed by Docker and stored under /var/lib/docker/volumes. They're ideal for production environments because they're portable, secure, and easier to back up.

On the other hand, bind mounts directly map a host directory into the container, which is useful for local development when I want to reflect changes immediately. However, bind mounts are more prone to permission issues and tightly couple the container to the host filesystem, so I avoid using them in production.

## Q12: Can we install docker inside a container?

Yes, we can install docker inside a container

## Q13: What is inside docker file?

A Dockerfile contains a set of instructions to automate the creation of a Docker image. It includes steps like setting the `base image (FROM)`, `installing packages (RUN)`, `copying code (COPY)`, `defining environment variables (ENV)`, `exposing ports (EXPOSE)`, and specifying the default command using `CMD or ENTRYPOINT`.

When we build the image with docker build, these instructions are executed sequentially to create a layered, reusable image."

## Q14: What is the issue with using large file image in dockerfile?

Using large files or images in a Dockerfile can slow down the build, push/pull, and deployment process. It also increases storage usage on Docker hosts and CI agents, and makes caching less efficient.

Large base images may introduce unnecessary vulnerabilities, so I prefer using slim or Alpine-based images.

## Q15: If Docker containers are consuming too much disk space, how do you fix it?

If Docker containers are consuming too much disk space, I usually troubleshoot and fix the issue by cleaning up unused resources like dangling images, stopped containers, unused volumes, and build cache. 

## Q16: Are you using Dockerfile? Are you building the dockerfile by codebuild?

Yes I do use Dockerfile in most of our applications to containerize our applications. But I use Docker platform to build the images. If something comes up with codeBuild, I can do that.

## Q17: How many containers can run in a pod?

A kubernetes pod runs one or more containers in a pod but in most cases we run a single container per pod to follow the single-responsibility principle.

## Q18: In your projects how many containers you ran? Can you give me the use case where can run 4-5 containers in a pod?

Typically, we run one container a pod, we run two containers in a pod, which is side car container. I implemented service mesh like envoy as a side car container,

## Q19: Write sample docker file
```bash
FROM node:14
WORKDIR /app
COPY package.json
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

## Q20: What is the difference between CMD and ENTRYPOINT?

Both CMD and ENTRYPOINT define the default behavior of a Docker container at runtime, but they differ in purpose and flexibility.

CMD provides default commands for the container, which can be overridden at runtime. ENTRYPOINT defines the executable that will always run when the container starts, and CMD arguments are passed to it.

Use **ENTRYPOINT** when your container is meant to run only one main process, like:
   nginx, python app.py, java -jar app.jar, bash script.sh

🔹 Use **CMD** to provide default options for that main command, like:
    Default flags, file paths, or environment settings

## Q21: How do you reduce the size of Dockerfile?

I reduce Docker image size by:
- Using slim/minimal base images
- Leveraging multi-stage builds
- Cleaning up cache/temp files
- Minimizing layers and build context
This results in faster builds, smaller attack surface, and efficient deployments.

## Q22: What is the difference between COPY and ADD command in Docker File?

Both COPY and ADD are used in Dockerfiles to copy files from your build context into the image.
The key difference is that ADD offers more features, while COPY is more predictable and preferred for simplicity.

Use COPY whenever possible because it's explicit and easier to maintain.
Use ADD only when you need features like:
- Extracting tar archives
- Downloading from remote URLs (though not recommended — better to use curl or wget)

## Q23: Explain me all the steps for multi stage docker image

A multi-stage Docker build allows you to use one stage for building your application and a second, much lighter stage for running it — minimizing the final image size and removing build-time dependencies."

1. First Stage: Build Environment
Use a base image with build tools, like compilers or SDKs.
```bash
# Stage 1: Build stage
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
```

2. Second Stage: Runtime Environment
Use a lightweight base image (e.g., alpine, nginx, distroless) to copy only the final output from the builder.
```bash
# Stage 2: Production image
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## Q24: What are the layers you will get in Docker while building?

When building a Docker image, each instruction in the Dockerfile creates a new image layer. These layers form a stacked filesystem where each layer represents a filesystem delta — and together, they build the final image.

## Q25: What is RUN and EXEC command?

1. RUN (used in Dockerfile)
The RUN command is used inside a Dockerfile to execute commands during the image build process.

2. docker exec (used on a running container)
The exec command is used in the CLI to run a command inside an already running container.
Used for debugging, inspecting, or running maintenance tasks

## Q26: How do you fix security issues in Docker images?

I scan Docker images using tools like Trivy or Snyk, then fix issues by updating the base image, patching OS and app dependencies, and reducing image size with multi-stage builds. Once fixed, I rebuild, rescan, and promote the secure image through the pipeline."

## Q27: What happens if the Docker image is large? What issues can occur?

If a Docker image is large, it can cause multiple performance, security, and operational issues throughout the CI/CD pipeline and runtime environment."

🐢 1. Slow Build and Deployment Times
📦 2. Increased Storage Consumption
🐘 3. Higher Bandwidth and Network Load
    Large images = more data transferred
🔓 4. Larger Attack Surface
    More packages → more outdated libraries → more vulnerabilities

## Q28: What are the types of storage drivers in Docker?

Docker uses storage drivers to manage the layered filesystem of containers. The default and most efficient driver is overlay2, which uses union mounts to combine layers. Other drivers like aufs, devicemapper, btrfs, and zfs exist for specialized environments. For Windows containers, Docker uses windowsfilter

## Q29: Does Docker have kernel in place?

No, Docker does not have its own kernel. Docker containers share the host system's Linux kernel.

## Q30: In your projects, how many containers you ran? Give a use case where you ran 4-5 containers in a pod.

In most of my projects, we follow the one-container-per-pod model, but there are valid cases for multi-container pods. I've worked on setups where we ran 4–5 containers within a single pod using the sidecar pattern, especially in observability, logging, and tightly coupled service coordination.

In one of our EKS workloads, we ran 5 containers in a pod to implement a secure, observable, and resilient real-time service. We followed sidecar and ambassador patterns — Fluent Bit for logging, Prometheus for metrics, Envoy for proxying, and a helper container for token management. All these supported the main application while maintaining isolation and shared resources."

## Q31: What are CNAME and namespace in docker?

CNAME in Docker is not a native concept, but we simulate it using Docker network aliases to point one container name to another — like DNS aliasing. Namespaces, on the other hand, are foundational to Docker's isolation model. Docker uses Linux namespaces to isolate containers' processes, networking, filesystems, and more — allowing them to run securely on the same host."

## Q32: Which network will be used to isolate a communication between two containers?

The none network driver is used to isolate containers from networking, preventing them from communicating with other containers or the host.

## Q33: How Docker is operable on a Linux machine? Explain the docker architecture components

Docker runs natively on Linux using the host's kernel. It leverages Linux features like cgroups and namespaces to provide isolation and resource management for containers.

## Q34: What is the difference between VM and Docker?

The core difference is that VMs virtualize hardware and run a full OS, while Docker containers virtualize at the OS level and share the host kernel. Containers are lighter, faster, and more portable than VMs

## Q35: If Docker containers are consuming too much disk space, how do you fix it?

Use docker system prune to remove unused images, containers, and volumes. Optimize Dockerfiles to reduce image size, and use multistage builds.

## Q36: How do you deploy a Docker-based app to Kubernetes?

Define a Kubernetes deployment YAML file, specifying the Docker image, replicas, and resources. Use kubectl apply -f deployment.yaml to deploy the application.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 80
```