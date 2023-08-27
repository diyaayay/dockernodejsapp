pipeline {
    agent any
    environment {
        PROJECT_NAME = "awsdevops"
    }
    stages {
        stage("Docker Build") {
            steps {
                script {
                    sh "docker build -t ${PROJECT_NAME} ."
                }
            }
        }
        stage("Docker Run") {
            steps {
                script {
                    sh "(docker ps -s | grep ${PROJECT_NAME}) && (docker stop ${PROJECT_NAME} && docker rm ${PROJECT_NAME})"
                    sh "docker run -d --name ${PROJECT_NAME} ${PROJECT_NAME}:latest"
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
