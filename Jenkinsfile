pipeline {
    agent {
        label 'ubu-app-agent'
    }
    environment {
        DOCKER_IMAGE = 'dockme70/snake'
        DOCKER_REGISTRY = 'https://registry.hub.docker.com'
    }
    stages {
        stage('Cloning Git') {
            steps {
                // Cloning the repository into the workspace
                checkout scm
            }
        }

        stage('Build-and-Tag') {
            steps {
                script {
                    // Building the Docker image
                    app = docker.build(env.DOCKER_IMAGE)
                }
            }
        }

        stage('Post-to-dockerhub') {
            steps {
                script {
                    // Pushing the Docker image to Docker Hub
                    docker.withRegistry(env.DOCKER_REGISTRY, 'dockerhub_cred') {
                        app.push("latest")
                    }
                }
            }
        }

        stage('Pull-image-server') {
            steps {
                // Pulling and running the Docker image on the server
                sh 'echo pulling image ...'
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }
    post {
        always {
            // Cleaning up the workspace
            cleanWs()
        }
    }
}
