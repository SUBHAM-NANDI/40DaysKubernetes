## Day 2/40
## How to Dockerize a Project


### Install Docker Desktop

To get started with Docker on your local machine, download the Docker Desktop client:

- [Docker Desktop Download](https://www.docker.com/products/docker-desktop/)

### Getting Started with Docker

1. **Clone a Sample Repository**:  
   You can either use your own project or clone a sample repository using the following command:

   ```bash
   git clone https://github.com/docker/getting-started-app.git
   ```

2. **Navigate to the Project Directory**:  
   Change your working directory to the project folder:

   ```bash
   cd getting-started-app/
   ```

3. **Create a Dockerfile**:  
   Inside the project directory, create a new file named `Dockerfile`:

   ```bash
   touch Dockerfile
   ```

4. **Add Dockerfile Content**:  
   Open the `Dockerfile` in your preferred text editor and paste the following content:

   ```dockerfile
   FROM node:18-alpine
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   CMD ["node", "src/index.js"]
   EXPOSE 3000
   ```

   This Dockerfile sets up a Node.js environment, installs dependencies, and specifies how to run the application.

5. **Build the Docker Image**:  
   Build the Docker image from the Dockerfile using the following command:

   ```bash
   docker build -t day02-todo .
   ```

6. **Verify the Docker Image**:  
   After building, check that the image was created and stored locally:

   ```bash
   docker images
   ```

### Pushing the Docker Image to Docker Hub

1. **Log In to Docker Hub**:  
   Authenticate with Docker Hub to push your image:

   ```bash
   docker login
   ```

2. **Tag the Image**:  
   Tag your image to prepare it for pushing to a remote repository:

   ```bash
   docker tag day02-todo:latest username/new-reponame:tagname
   ```

3. **Push the Image**:  
   Push the tagged image to Docker Hub:

   ```bash
   docker push username/new-reponame:tagname
   ```

### Pulling and Running the Docker Image

1. **Pull the Image in Another Environment**:  
   If you need to pull the image to a different environment, use:

   ```bash
   docker pull username/new-reponame:tagname
   ```

2. **Run the Docker Container**:  
   Start the container and map the application to port 3000:

   ```bash
   docker run -dp 3000:3000 --name docker-container-name username/new-reponame:tagname
   ```

   After this, your app should be accessible at `http://localhost:3000`.

### Additional Docker Commands

- **Access the Container Shell**:  
  To enter the running container, use:

  ```bash
  docker exec -it containername sh
  ```
  or
  ```bash
  docker exec -it containerid sh
  ```

- **View Container Logs**:  
  To check the logs from your container:

  ```bash
  docker logs containername
  ```
  or
  ```bash
  docker logs containerid
  ```

