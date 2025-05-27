### Explain Docker networking and types of network. What is the default network.
 Docker networking enables communication between containers, the host, and external networks (like the internet). Docker automatically sets up networking for containers using different network drivers.
    Types of Network:
    - Bridge (default)
    - Host
    - None
    - Overlay (Swarm only)
    - Macvlan
The default network is Bridge

### Docker bind mount vs volume 
Docker supports two main types of persistent storage: volumes and bind mounts.

Volumes are fully managed by Docker and stored under /var/lib/docker/volumes. They’re ideal for production environments because they’re portable, secure, and easier to back up.

On the other hand, bind mounts directly map a host directory into the container, which is useful for local development when I want to reflect changes immediately. However, bind mounts are more prone to permission issues and tightly couple the container to the host filesystem, so I avoid using them in production.

### Can we install docker inside a container.
Yes, we can install docker inside a container

### What is inside docker file
A Dockerfile contains a set of instructions to automate the creation of a Docker image. It includes steps like setting the `base image (FROM)`, `installing packages (RUN)`, `copying code (COPY)`, `defining environment variables (ENV)`, `exposing ports (EXPOSE)`, and specifying the default command using `CMD or ENTRYPOINT`.

When we build the image with docker build, these instructions are executed sequentially to create a layered, reusable image.”

### What is the issue with using large file image in dockerfile
Using large files or images in a Dockerfile can slow down the build, push/pull, and deployment process. It also increases storage usage on Docker hosts and CI agents, and makes caching less efficient.

Large base images may introduce unnecessary vulnerabilities, so I prefer using slim or Alpine-based images.

### If Docker containers are consuming too much disk space, how do you fix it?
If Docker containers are consuming too much disk space, I usually troubleshoot and fix the issue by cleaning up unused resources like dangling images, stopped containers, unused volumes, and build cache. 

###R u using Dockerfile? u r build the dockerfile by codebuild?
Yes I do use Dockerfile in most of our applications to containerize our applications. But I use Docker platform to build the images. If something comes up with codeBuild, I can do that.

### How many containers can run in a pod?
A kubernetes pod runs one or more containers in a pod but in most cases we run a single container per pod to follow the single-responsibility principle.

### in ur projects how many containers u ran? can u give me the use case where can run 4-5 containers in a pod?
Typiclaly, we run one container a pod, we run two containers in a pod, which is sdie car container. I implemented service mesh like envoy as a side car container,

### write sample docker file
``` bash
    FROM node:14
    WORKDIR /app
    COPY package.json
    RUN npm install
    COPY . .
    EXPOSE 3000
    CMD ["npm", "start"]
```

### difference between cmd and entry point 
Both CMD and ENTRYPOINT define the default behavior of a Docker container at runtime, but they differ in purpose and flexibility.

CMD provides default commands for the container, which can be overridden at runtime. ENTRYPOINT defines the executable that will always run when the container starts, and CMD arguments are passed to it.

 Use **ENTRYPOINT** when your container is meant to run only one main process, like:
   nginx, python app.py, java -jar app.jar, bash script.sh

🔹 Use **CMD** to provide default options for that main command, like:
    Default flags, file paths, or environment settings
### How do you reduce the size of Dockerfile
I reduce Docker image size by:

    Using slim/minimal base images
    Leveraging multi-stage builds
    Cleaning up cache/temp files
    Minimizing layers and build context
    This results in faster builds, smaller attack surface, and efficient deployments.
### What is the difference between COPY and ADD command in Docker File.
Both COPY and ADD are used in Dockerfiles to copy files from your build context into the image.
The key difference is that ADD offers more features, while COPY is more predictable and preferred for simplicity.

    Use COPY whenever possible because it’s explicit and easier to maintain.
    Use ADD only when you need features like:

        Extracting tar archives
        Downloading from remote URLs (though not recommended — better to use curl or wget)
### Explain me all the steps for multi stage docker image
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
### What are the layer's you will get in Docker while building 
When building a Docker image, each instruction in the Dockerfile creates a new image layer. These layers form a stacked filesystem where each layer represents a filesystem delta — and together, they build the final image.

### what is run and exec command
1. RUN (used in Dockerfile)
The RUN command is used inside a Dockerfile to execute commands during the image build process.
2. docker exec (used on a running container)
The exec command is used in the CLI to run a command inside an already running container.
Used for debugging, inspecting, or running maintenance tasks


### Diff b/w using VM vs Docker-notAnswered

### Types of storage drivers in Docker

### Does Docker have kernel in place

C name and namespace in docker
### Which network will be used to isolate a communication b/w two containers
### How Docker is operable on a Linux machine? Explain the docker architecture components