pipeline {
    agent any

    // Variables globales
    environment {
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_USERNAME = 'ayoubjebahi'        // ton username Docker Hub
        REPO_NAME       = 'monapp'             // pour avoir monapp-serveur / monapp-client
        IMAGE_SERVER    = "${DOCKER_USERNAME}/${REPO_NAME}-serveur"
        IMAGE_CLIENT    = "${DOCKER_USERNAME}/${REPO_NAME}-client"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    echo "Compilation gérée dans les Dockerfiles (multi-stage build)."
                }
            }
        }

        stage('Docker Build Images') {
            steps {
                script {
                    def imageTag = "build-${env.BUILD_NUMBER}"

                    sh """
                      echo '--- Build image serveur ---'
                      docker build -t ${IMAGE_SERVER}:${imageTag} ./serveur
                      docker tag ${IMAGE_SERVER}:${imageTag} ${IMAGE_SERVER}:latest

                      echo '--- Build image client ---'
                      docker build -t ${IMAGE_CLIENT}:${imageTag} ./client
                      docker tag ${IMAGE_CLIENT}:${imageTag} ${IMAGE_CLIENT}:latest
                    """
                }
            }
        }

        stage('Docker Push to Registry') {
            steps {
                script {
                    def imageTag = "build-${env.BUILD_NUMBER}"

                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )]) {
                        sh """
                          echo "${DOCKER_PASSWORD}" | docker login -u "${DOCKER_USER}" --password-stdin ${DOCKER_REGISTRY}

                          echo '--- Push serveur ---'
                          docker push ${IMAGE_SERVER}:${imageTag}
                          docker push ${IMAGE_SERVER}:latest

                          echo '--- Push client ---'
                          docker push ${IMAGE_CLIENT}:${imageTag}
                          docker push ${IMAGE_CLIENT}:latest
                        """
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    def imageTag = "build-${env.BUILD_NUMBER}"

                    sh """
                      echo '--- Mise à jour des manifests ---'
                      sed -i 's|image: .*monapp-serveur:.*|image: ${IMAGE_SERVER}:${imageTag}|g' ci-cd-config/k8s-serveur-deployment.yaml
                      sed -i 's|image: .*monapp-client:.*|image: ${IMAGE_CLIENT}:${imageTag}|g' ci-cd-config/k8s-client-deployment.yaml

                      echo '--- Déploiement sur Kubernetes ---'
                      kubectl apply -f ci-cd-config/k8s-serveur-deployment.yaml
                      kubectl apply -f ci-cd-config/k8s-client-deployment.yaml
                    """
                }
            }
        }
    }
}

