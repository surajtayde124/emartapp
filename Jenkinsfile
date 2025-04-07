pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from the current branch
                    git: "https://github.com/surajtayde124/emartapp.git"
                }
            }
        }
        
    post {
        always {
            // Clean up Docker images after the pipeline run
            sh "docker-compose -f ${DOCKER_COMPOSE_FILE} down"
        }

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
        
}
