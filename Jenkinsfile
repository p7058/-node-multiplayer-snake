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
                /* Let's make sure we have the repository cloned to our workspace */
                checkout scm
            }
        }

        stage('Build-and-Tag') {
            steps {
                script {
                    /* This builds the actual image; synonymous to docker build on the command line */
                    app = docker.build(env.DOCKER_IMAGE)
                }
            }
        }

        stage('Post-to-dockerhub') {
            steps {
                script {
                    docker.withRegistry(env.DOCKER_REGISTRY, 'dockerhub_cred') {
                        app.push("latest")
                    }
                }
            }
        }

        stage('Pull-image-server') {
            steps {
                sh 'echo pulling image ...'
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
