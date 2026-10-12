pipeline {
    agent any

    stages {
        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                // Clean and install requirements
                sh 'python3 -m pip install -r requirements.txt'

                // FIXED: Injects environment variables to bind Flask properly and detaches the runner process context
                sh 'export FLASK_RUN_HOST=0.0.0.0 && nohup python3 app.py > flask.log 2>&1 &'

                // Give the web server plenty of headroom to initialize (5 seconds)
                sh 'sleep 5'

                // Run your non-headless pytest verification suite
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
