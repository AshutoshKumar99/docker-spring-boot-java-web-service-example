## Jenkins Pipeline for Spring Boot Application

This Jenkins pipeline automates the process of building, creating a Docker image, and deploying a Spring Boot application. It ensures code quality and seamless deployment through well-defined stages.

### Prerequisites

- Docker
- Maven 3
- OpenJDK version 11

### Pipeline Stages

1. **Checkout**
   - Clones the code from the GitHub repository.

2. **Build**
   - Cleans and builds the project using Maven.

3. **Docker Build and Push**
   - Builds the Docker image for the Spring Boot application.
   - Pushes the Docker image to Docker Hub.

4. **Deploy Spring Boot App to Container**
   - Pulls the Docker image from Docker Hub.
   - Stops and removes any existing containers running on port 80.
   - Deploys the new Docker container with the updated application.

### Access the Application

- URL: `http://IPV4:80/docker-java-app/test`

### Key Points
- **Credentials Management**: Ensure Docker Hub (`dockerhub-credentials`) and SSH (`sshcred`) credentials are configured in Jenkins.
- **Server Preparation**: Ensure the target server has Docker installed, running, and accessible via SSH.
- **Pipeline Clarity**: The stages are clearly named and commented for easy understanding and maintenance.

This pipeline provides an efficient CI/CD process for your Spring Boot application, leveraging Jenkins and Docker for automated builds, image creation, and deployment.
