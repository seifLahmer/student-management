pipeline {
    agent any

    environment {
        IMAGE_NAME = "studentapp"
        CONTAINER_NAME = "studentcontainer"
        PORT_MAPPING = "8081:8080"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Code already checked out by Jenkins"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image..."
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Remove Old Container') {
            steps {
                echo "Removing old container if exists..."
                sh "docker rm -f ${CONTAINER_NAME} || true"
            }
        }

        stage('Run New Container') {
            steps {
                echo "Running new container..."
                sh "docker run -d -p ${PORT_MAPPING} --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
            }
        }

        stage('Verification') {
            steps {
                echo "Listing running containers..."
                sh "docker ps"
                echo "Listing images..."
                sh "docker images"
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully 🚀"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}
