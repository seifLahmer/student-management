pipeline {
    agent any
    environment {
        IMAGE_NAME = "myapp"
        CONTAINER_NAME = "myapp_container"
        PORT_MAPPING = "8080:8080"
        SONAR_HOST_URL = "http://192.168.1.14:9000"
        SONAR_AUTH_TOKEN = credentials('sonarqube') // Jenkins credential ID
    }
	  tools {
        jdk 'JAVA_HOME' // name of JDK configured in Jenkins
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo "Code already checked out by Jenkins"
            }
        }

        stage('Maven Build') {
            steps {
                echo "Building project with Maven..."
                sh 'mvn clean compile'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube analysis..."
                sh """
                    mvn sonar:sonar \
                    -Dsonar.projectKey=devops_gil \
                    -Dsonar.host.url=$SONAR_HOST_URL \
                    -Dsonar.login=$SONAR_AUTH_TOKEN
                """
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
}
