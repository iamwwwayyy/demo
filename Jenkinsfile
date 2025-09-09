pipeline {
    agent any

    stages {
        stage('Clean Docker Auth') {
            steps {
                sh '''
                    echo "=== Current Docker login status ==="
                    docker info | grep -i username || true
                    echo "=== Logging out from Docker ==="
                    docker logout || true
                    echo "=== Checking Docker config ==="
                    ls -la ~/.docker/ || true
                    echo "=== Clean complete ==="
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    echo "=== Building Docker image ==="
                    docker build --no-cache -t dewixaltius/jenkins-demo .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh '''
                        echo "=== Logging into Docker Hub ==="
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        echo "=== Pushing image ==="
                        docker push dewixaltius/jenkins-demo
                    '''
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