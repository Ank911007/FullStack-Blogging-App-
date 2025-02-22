pipeline { 
    agent { label 'slave-1' }


    
     
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    stages {
        
        stage('Compile') {
            steps {
            sh  "mvn compile"
            }
        }
        
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        
        stage('Package') {
            steps {
                sh "mvn package"
            }
        }

         stage('Build Image') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-cred') {
                        sh 'docker build -t ank906/blogging:latest -f docker/Dockerfile .'
                    }
                }
            }
        }
        
        stage('trivy image scan') {
            steps {
                echo '''trivy image --format table \
        -o image.html ank906/blogging:latest'''
            }
        }
        
        stage('Push Image') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-cred') {
                        sh 'docker push ank906/blogging:latest'
                    }
                }
            }
        }
    }
}
