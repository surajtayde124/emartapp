pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'docker-compose'
        KUBECONFIG = 'C:\Users\Suraj\.kube\\config'
        MINIKUBE_HOME = 'C:\Users\Suraj\.minikube'
    }

    stages {
        
        stage('Debug') {
            steps {
                echo "KUBECONFIG path: ${KUBECONFIG}"
                echo "MINIKUBE_HOME path: ${MINIKUBE_HOME}"
            }
        }
        stage('Checkout') {
            steps {
                // Checkout your project from the source control
               checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/surajtayde124/emartapp.git']]])
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build your Docker image
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push the Docker image to your registry
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                // Set the Docker environment to use Minikube's Docker daemon
                script {
                    withEnv(["DOCKER_HOST=tcp://192.168.99.100:2376", "DOCKER_CERT_PATH=${MINIKUBE_HOME}/.minikube/certs", "DOCKER_TLS_VERIFY=1"]) {
                        // Run your container in Minikube
                        bat "kubectl --kubeconfig=${KUBECONFIG} apply -f kubeconfig.yaml"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Reaching at the end of pipeline...'
            // Add any post-build clean-up steps here
        }
        success {
            echo 'Build, test, and deployment completed successfully.'
        }
        failure {
            echo 'The build, test, or deployment failed.'
        }
    }
}
