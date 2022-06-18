pipeline {
    agent any

    stages {
        stage('Clone git repo') {
            steps {
                sh 'echo "STAGE 0: Cloning app code from SCM ..."'
                git 'https://github.com/rakeshdevopsguru/cicd-with-jenkins-docker-eks.git'
            }    
        }        
        stage('Lint all app code') {
            steps {
                sh 'echo "STAGE 1: Checking app code for syntax error ..."'
               
            }
        }   
        stage( 'Build docker image for app' ) {
            steps {
                sh 'echo "STAGE 2: Building and tagging docker image ..."'
                sh 'docker build -t web-app:v1.0 .'
                sh 'docker image ls'                  
            }
        } 
        stage( 'Push image to dockerhub repo' ) {
            steps {
                withDockerRegistry([url: "", credentialsId: "dockerhub"]) {
                    sh 'echo "STAGE 3: Uploading image to dockerhub repository ..."'
                    sh 'docker login'
                    sh 'docker tag web-app:v1.0 rakesh1533/webapp:v1.0'
                    sh 'docker push rakesh1533/webapp:v1.0'          
                }
            }
        }                                   
        stage( 'Deploy image to AWS EKS' ) {
             kubernetesDeploy(
               configs: 'templates/*',
               kubeconfigId: 'K8S',
               enableConfigSubstitution: true
               )  
            }
        }               
    }
