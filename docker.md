# Docker

1. **Installing Docker**
   
   1.1. Download and install Docker:

   ```bash
   https://docs.docker.com/get-docker/
   ```

   1.2. Verify the installation:

   ```bash
   docker --version
   ```

2. **Managing Docker Images**
   
   2.1. List local images:

   ```bash
   docker images
   ```

   2.2. Pull an image from a registry:

   ```bash
   docker pull <IMAGE_NAME>:<TAG>
   ```

   2.3. Build an image from a Dockerfile:

   ```bash
   docker build -t <IMAGE_NAME>:<TAG> <PATH>
   ```

   2.4. Remove an image:

   ```bash
   docker rmi <IMAGE_NAME>:<TAG>
   ```

3. **Working with Containers**
   
   3.1. Run a container in detached mode:

   ```bash
   docker run --name <CONTAINER_NAME> -d <IMAGE_NAME>:<TAG>
   ```

   3.2. List running containers:

   ```bash
   docker ps
   ```

   3.3. List all containers (including stopped):

   ```bash
   docker ps -a
   ```

   3.4. View container logs:

   ```bash
   docker logs <CONTAINER_NAME>
   ```

   3.5. Stop a running container:

   ```bash
   docker stop <CONTAINER_NAME>
   ```

   3.6. Remove a stopped container:

   ```bash
   docker rm <CONTAINER_NAME>
   ```

   3.7. Execute a shell inside a container:

   ```bash
   docker exec -it <CONTAINER_NAME> sh
   ```

4. **Docker Compose (v1)**
   
   4.1. Start services and build images if needed:

   ```bash
   docker-compose up --build
   ```

   4.2. Start in detached mode:

   ```bash
   docker-compose up -d
   ```

   4.3. Specify a compose file:

   ```bash
   docker-compose -f docker-compose.dev.yml up
   ```

   4.4. Stop and remove containers, networks:

   ```bash
   docker-compose down
   ```

   4.5. List Compose-managed containers:

   ```bash
   docker-compose ps
   ```

   4.6. View logs for a specific service:

   ```bash
   docker-compose logs <SERVICE_NAME>
   ```

   4.7. Rebuild a specific service:

   ```bash
   docker-compose build <SERVICE_NAME>
   ```

5. **System Maintenance**
   
   5.1. Show disk and resource usage:

   ```bash
   docker system df
   ```

   5.2. Remove unused data (containers, networks, images):

   ```bash
   docker system prune
   ```