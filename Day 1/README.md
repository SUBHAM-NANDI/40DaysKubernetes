## Day 1/40
## Docker Tutorial For Beginners - Docker Fundamentals
## Docker Architecture


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/88d17yqjbir9ncz5tpun.jpg)

### Components:
1. **Client**: The interface through which a user interacts with Docker. Commands like `docker build`, `docker push`, `docker pull`, and `docker run` are issued from here.

2. **Docker Host**: The machine where Docker is installed. It hosts the Docker Daemon, images, and containers.

3. **Docker Daemon (dockerd)**: The background service that manages Docker objects (such as images, containers, networks, and volumes) and listens for Docker API requests.

4. **Dockerfile**: A text file that contains instructions to build a Docker image. It includes steps like installing software, copying files, and setting up the environment.

5. **Docker Registry**: A storage and distribution system for Docker images. Docker Hub is the default registry, but private registries can also be used.

6. **Docker Image**: A lightweight, standalone, and executable package that includes everything needed to run a piece of software, including the code, a runtime, libraries, and settings.

7. **Docker Container**: A runnable instance of a Docker image. Containers are isolated environments that share the OS kernel but are otherwise independent.

### Workflow Steps:
1. **`docker build` (Step 1)**: The client sends a `docker build` command to the Docker Daemon. The Docker Daemon reads the Dockerfile (Step 2) to create a Docker Image (Step 3).

2. **Docker Daemon processes Dockerfile (Step 2)**: The Dockerfile is read by the Docker Daemon, which executes the instructions to build an image.

3. **Docker Image Creation (Step 3)**: Once the Dockerfile is processed, a Docker Image is created.

4. **`docker push` (Step 4)**: The client sends a `docker push` command to the Docker Daemon, instructing it to push the Docker Image to a Docker Registry (Step 5).

5. **Docker Registry (Step 5)**: The Docker Image is stored in the Docker Registry, making it available for others to pull and use.

6. **`docker pull` (Step 6)**: The client sends a `docker pull` command to the Docker Daemon, which pulls the Docker Image from the Docker Registry to the Docker Host.

7. **`docker run` (Step 7)**: The client issues a `docker run` command, and the Docker Daemon creates a Docker Container (Step 9) from the Docker Image.

8. **Docker Container Creation (Step 9)**: The Docker Daemon uses the Docker Image to create a Docker Container, which is then executed.

