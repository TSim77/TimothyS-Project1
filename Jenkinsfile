pipeline {
    
    agent any
    
    environment {
        dockerImage = ''
        registry = 'tsim77/project_1_docker_repository'
        registryCredential = 'dockerhub_id'
    }
    
    stages {
        
        stage('Clone Git') { 
            steps { 
                git 'https://github.com/TSim77/flask-demo.git' 
            }
        }
        
        stage('Build image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        
        stage('Deploy image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        }
        
        stage('Clean up') { 
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        }
        
        stage('Stop Docker container') {
         steps {
            sh 'docker ps -f name=flaskappContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=flaskappContainer -q | xargs -r docker container rm'
         }
        }
        
        stage('Docker Run') {
        steps{
            script {
                dockerImage.run("-p 8096:5000 --rm --name flaskappContainer")
             }
            }
        }
    }
}
