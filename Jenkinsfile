pipeline {
    agent any
    tools {
        jdk "openjdk11" // Specify the JDK tool to use
        maven "Maven3" // Specify the Maven tool to use
    }

    stages {
        stage('Checkout') { // Stage to checkout the code from the repository
            steps {
                git changelog: false, poll: false, url: 'https://github.com/AshutoshKumar99/docker-spring-boot-java-web-service-example.git'
            }
        }
        stage('Build') { // Stage to build the project using Maven
            steps {
                sh 'mvn clean install' // Run Maven clean install
            }
        }
        stage('Docker Build and Push') { // Stage to build and push the Docker image
            steps {
                script {
                    def dockerImage = "ashutoshkumar999/docker-spring-boot-java-web-service:${BUILD_NUMBER}" // Define Docker image name with build number
                    withDockerRegistry(credentialsId: 'dockerhub-credentials') { // Use Docker registry credentials
                        sh "docker build -t ${dockerImage} ." // Build Docker image
                        sh "docker push ${dockerImage}" // Push Docker image to registry
                        sh 'java -version'
                    }
                }
            }
        }
        stage('Deploy Spring Boot App to Container') { // Stage to deploy the Spring Boot application to a container
            steps {
                script {
                    def dockerImage = "ashutoshkumar999/docker-spring-boot-java-web-service:${BUILD_NUMBER}" // Define Docker image name with build number
                    sshagent(credentials: ['sshcred']) { // Use SSH credentials
                        sh """
                            ssh -o StrictHostKeyChecking=no ubuntu@65.1.136.93 <<EOF
                            docker pull ${dockerImage} // Pull the Docker image

                            # Stop and remove any container using port 80
                            PORT_CONTAINERS=\$(docker ps -q --filter "publish=80")
                            if [ ! -z "\$PORT_CONTAINERS" ]; then
                                docker stop \$PORT_CONTAINERS
                                docker rm \$PORT_CONTAINERS
                            fi

                            # Stop and remove any container with the name docker-spring-boot-java-web-service
                            if [ \$(docker ps -aq -f name=docker-spring-boot-java-web-service) ]; then
                                docker stop docker-spring-boot-java-web-service
                                docker rm docker-spring-boot-java-web-service
                            fi

                            # Run the new Docker container
                            docker run -d -p 80:8080 --name docker-spring-boot-java-web-service ${dockerImage}
EOF
                        """
                    }
                }
            }
        }
    }
}
