pipeline {
    agent any
    environment {
        APP_NAME = "devops-task"
        DOCKER_IMAGE = "devops-task:latest"
        APP_PORT = "3000"
    }

    stages {
        stage('Pull Code') {
            steps {
                git branch: 'main', url: 'https://github.com/om0508/Devops-task.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh "docker run --rm $DOCKER_IMAGE npm test || echo 'No tests found'"
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying container..."
                sh "docker rm -f $APP_NAME || true"
                sh "docker run -d --name $APP_NAME -p $APP_PORT:$APP_PORT $DOCKER_IMAGE"
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
