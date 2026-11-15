
pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        IMAGE_SERVER = 'malekbejaoui1/mern-server'
        IMAGE_CLIENT = 'malekbejaoui1/mern-client'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:malek123814/mern-app-docker.git',
                    credentialsId: 'github_ssh'
            }
        }

        stage('SERVER : build, scan, push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DH_USER',
                    passwordVariable: 'DH_PASS'
                )]) {
                    sh '''
                        set -eu

                        echo "===== BUILD SERVER ====="

                        docker build \
                          -t "$IMAGE_SERVER:$BUILD_NUMBER" \
                          server

                        echo "===== SCAN TRIVY SERVER ====="

                        docker run --rm \
                          -v /var/run/docker.sock:/var/run/docker.sock \
                          aquasec/trivy:0.76.2 image \
                          "$IMAGE_SERVER:$BUILD_NUMBER" \
                          > trivy_server.txt

                        cat trivy_server.txt

                        echo "===== LOGIN DOCKER HUB ====="

                        echo "$DH_PASS" | docker login \
                          -u "$DH_USER" \
                          --password-stdin

                        echo "===== PUSH SERVER ====="

                        docker push "$IMAGE_SERVER:$BUILD_NUMBER"
                    '''
                }
            }
        }

        stage('CLIENT : build, scan, push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DH_USER',
                    passwordVariable: 'DH_PASS'
                )]) {
                    sh '''
                        set -eu

                        echo "===== BUILD CLIENT ====="

                        docker build \
                          -t "$IMAGE_CLIENT:$BUILD_NUMBER" \
                          client

                        echo "===== SCAN TRIVY CLIENT ====="

                        docker run --rm \
                          -v /var/run/docker.sock:/var/run/docker.sock \
                          aquasec/trivy:0.76.2 image \
                          "$IMAGE_CLIENT:$BUILD_NUMBER" \
                          > trivy_client.txt

                        cat trivy_client.txt

                        echo "===== LOGIN DOCKER HUB ====="

                        echo "$DH_PASS" | docker login \
                          -u "$DH_USER" \
                          --password-stdin

                        echo "===== PUSH CLIENT ====="

                        docker push "$IMAGE_CLIENT:$BUILD_NUMBER"
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "===== ARCHIVAGE RAPPORTS TRIVY ====="

            archiveArtifacts(
                artifacts: 'trivy_*.txt',
                allowEmptyArchive: true
            )

            echo "===== NETTOYAGE DOCKER ====="

            sh '''
                docker logout || true

                docker image rm \
                  "$IMAGE_SERVER:$BUILD_NUMBER" \
                  "$IMAGE_CLIENT:$BUILD_NUMBER" \
                  2>/dev/null || true
            '''
        }
    }
}
