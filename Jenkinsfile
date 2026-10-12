pipeline {
    agent any

    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                // Install Python dependencies
                sh 'python3 -m pip install -r requirements.txt'

                // FIXED: Removed the double 'sh sh' syntax error
                sh 'python3 app.py &'

                // Wait for Flask application to start
                sh 'sleep 5'

                // FIXED: Standardized to python3 to prevent ModuleNotFoundError
                sh 'python3 -m pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"

                sh "docker build -t mypythonflaskapp:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                  sh 'docker login -u saisathwika1126 -p Sathwika@1126'
                }
            }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Push Docker Image to Docker Hub"

                sh "docker tag mypythonflaskapp:v1 saisathwika1126/docker_week8/mypythonflaskapp:v1"

                sh "docker push saisathwika1126/docker_week8/mypythonflaskapp:v1"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying application to Kubernetes"

                sh 'kubectl apply -f deployment.yaml --validate=false'

                sh 'kubectl apply -f service.yaml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
