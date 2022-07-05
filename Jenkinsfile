pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/2206-devops-batch/flask-demo.git'
                
                // Run pip install
                sh "pip3 install -r requirements-dev.txt"
                
                // Run pytest
                sh "python3 -m pytest app-test.py"
            }
        }
    }
}
