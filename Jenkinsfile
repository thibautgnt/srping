pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'thibautgnt/spring-app'
        DOCKER_CREDENTIALS = 'docker-hub'
    }

    // Suppression de la section tools spécifique
    // Si vous configurez un JDK dans Jenkins, vous pouvez utiliser:
    // tools {
    //     jdk 'NOM-DE-VOTRE-JDK-DANS-JENKINS'
    // }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/thibautgnt/srping.git', credentialsId: 'github-token'
            }
        }

        stage('Build Application') {
            steps {
                sh './gradlew clean build -x test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
                }
            }
        }
    }
}
