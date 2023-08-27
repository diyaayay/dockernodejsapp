pipeline {
    agent any
    environment {
        PROJECT_NAME = "dockernodejsapp"
        DOCKER_IMAGE_TAG = "${PROJECT_NAME}:${BUILD_NUMBER}" // Use a unique tag
        CONTAINER_NAME = "${PROJECT_NAME}-${BUILD_NUMBER}" // Use a unique container name
    }
    stages {
        stage("Docker Build") {
            steps {
                script {
                    // Build the Docker image with a unique tag
                    sh "docker build -t ${DOCKER_IMAGE_TAG} ."
                }
            }
        }
        stage("Docker Run") {
            steps {
                script {
                    // Stop and remove the previous container with a unique name
                    sh "(docker ps -q -f name=${CONTAINER_NAME}) && (docker stop ${CONTAINER_NAME} && docker rm ${CONTAINER_NAME}) || true"

                    // Run the Docker container
                    sh "docker run -d --name ${CONTAINER_NAME} ${DOCKER_IMAGE_TAG}"
                }
            }
        }
        stage("Docker Test") {
            steps {
                script {
                    def responseCode = sh(script: 'curl -o /dev/null -s -w "%{http_code}" http://localhost:8000/', returnStatus: true)
                    if (responseCode == 200) {
                        echo "Test passed! HTTP Status Code: ${responseCode}"
                    } else {
                        error "Test failed! HTTP Status Code: ${responseCode}"
                    }
                }
            }
        }
    }
}

