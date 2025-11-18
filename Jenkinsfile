pipeline {
    agent any
    environment {
        # Remplacez par votre registre (ex: Docker Hub ou un registre privé)
        DOCKER_REGISTRY = 'registry.hub.docker.com/<aladin78>'
        IMAGE_NAME = 'devops_pro'
        TAG = "latest"
    }
    stages {
        stage('Checkout Code') {
            steps {
                # Récupère le code de GitHub (fait automatiquement par Jenkins)
                echo 'Code checked out from SCM'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    # Construction de l'image
                    docker.build("${DOCKER_REGISTRY}/${IMAGE_NAME}:${TAG}", ".")
                }
            }
        }
        stage('Push Image to Registry') {
            steps {
                script {
                    # Poussez l'image vers le registre Docker (Nécessite une configuration d'informations d'identification dans Jenkins)
                    docker.withRegistry("https://${DOCKER_REGISTRY}", 'docker-hub-credentials-id') {
                        docker.image("${DOCKER_REGISTRY}/${IMAGE_NAME}:${TAG}").push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                # Cette étape sera couverte dans la Section 4
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}
