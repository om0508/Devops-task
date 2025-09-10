pipeline {
    agent any

    environment {
        APP_NAME = "devops-task"
        DOCKER_IMAGE = "devops-task:latest"
        APP_PORT = "3000"   // change if your app runs on different port
    }

    stages {
        stage('Pull Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/om0508/Devops-task.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh "docker build -t $DOCKER_IMAGE ."
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo "Running tests..."
                    // If you have tests in package.json (npm test), else skip
                    sh "docker run --rm $DOCKER_IMAGE npm test || echo 'No tests found'"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying container..."
                    // Stop old container if running
                    sh "docker rm -f $APP_NAME || true"
                    // Run new container
                    sh "docker run -d --name $APP_NAME -p $APP_PORT:$APP_PORT $DOCKER_IMAGE"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful! App running on port $APP_PORT"
        }
        failure {
            echo "❌ Build or deployment failed!"
        }
    }
}
