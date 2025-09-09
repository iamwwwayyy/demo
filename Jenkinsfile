pipeline {
    agent any

    environment {
        DOCKER_CMD = '/opt/homebrew/bin/docker'
        DOCKER_HOST = 'unix:///Users/w/Library/Containers/com.docker.docker/Data/docker.sock'
    }

    stages {
        stage('Test Docker Connection') {
            steps {
                sh '''
                    echo "=== Testing Docker connection ==="
                    $DOCKER_CMD info || true
                    echo "=== Docker contexts ==="
                    $DOCKER_CMD context ls
                '''
            }
        }

        stage('Clean Docker Auth') {
            steps {
                sh '''
                    echo "=== Current Docker login status ==="
                    $DOCKER_CMD info | grep -i username || true
                    echo "=== Logging out from Docker ==="
                    $DOCKER_CMD logout || true
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
                    $DOCKER_CMD build --no-cache -t wwwayyy/jenkins-demo .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh '''
                        echo "=== Logging into Docker Hub ==="
                        echo $DOCKER_PASSWORD | $DOCKER_CMD login -u $DOCKER_USERNAME --password-stdin
                        echo "=== Pushing image ==="
                        $DOCKER_CMD push wwwayyy/jenkins-demo
                    '''
                }
            }
        }
    }

    post {
        always {
            sh '$DOCKER_CMD logout || true'
        }
    }
}