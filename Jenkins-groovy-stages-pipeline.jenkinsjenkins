pipeline {
    agent { label "Jenkins-agent-1" }

    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/LondheShubham153/django-notes-app.git", branch: "main"
                echo "Code cloned successfully"
            }
        }

        stage("Build") {
            steps {
                echo "Building the code"
                sh "whoami"
                sh "docker build -t notes-app:latest ."
                sh "docker images"
            }
        }

        stage("Test") {
            steps {
                echo "Testing the code"
            }
        }
         stage("Push to DockerHub") {
            steps {
                echo "Pushing the image to DockerHub"
                withCredentials([usernamePassword(
                    'credentialsId': "dockerHubCred",
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_PASS'
                )]) {
                    sh """
    set -ex
    echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USER}" --password-stdin
    docker image tag notes-app:latest ${DOCKERHUB_USER}/notes-app:latest || true
    docker push ${DOCKERHUB_USER}/notes-app:latest || true
"""
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the code"
                sh "docker compose up -d"
            }
        }
    }
}
