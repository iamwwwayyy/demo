pipeline {
    agent any

    stages {
        stage('Verify') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Use Docker Pipeline plugin
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        def customImage = docker.build("${env.DOCKER_USERNAME}/jenkins-demo", "--no-cache .")
                        customImage.push()
                    }
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}