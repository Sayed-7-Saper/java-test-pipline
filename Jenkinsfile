pipeline {
    agent any
    environment {
        OPENSHIFT_PROJECT = 'nti-java'
        OPENSHIFT_SERVER = 'https://api.ocpuat.devopsconsulting.org:6443'
        APP_PORT = '8080'
        APP_SERVICE_NAME = 'java-boot-app'
        APP_HOST_NAME = 'java-boot-app.apps.ocpuat.devopsconsulting.org'
    }
    stages {

        stage('Checkout') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/Sayed-7-Saper/java-test-pipline.git'
                }
            }
        }
        stage('Build') {
            steps {
                // Build your Docker image here
                sh 'docker build -t java-test:latest .'
            }
        }
       
    
        stage('Docker Login & Push image ') {
            steps {
                // Push the Docker image to Docker Hub
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "echo \${DOCKER_PASSWORD} | docker login -u \${DOCKER_USERNAME} --password-stdin"
                    sh 'docker tag java-test:latest 10103040/java-test:latest'
                    sh 'docker push 10103040/java-test:latest'
                    sh 'docker logout'
                  
                }
            }
        }
         stage('Deploy to OpenShift') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'openshift', variable: 'OPENSHIFT_SECRET')]) {
                        sh "oc login --token=\${OPENSHIFT_SECRET} \${OPENSHIFT_SERVER} --insecure-skip-tls-verify"
                    }
                    sh "oc project \${OPENSHIFT_PROJECT}"    
                    sh "oc new-app 10103040/java-test:latest"
                    sh "oc create route edge --service \${APP_SERVICE_NAME} --port \${APP_PORT} --hostname \${APP_HOST_NAME} --insecure-policy Redirect"



                    //sh "oc project elsayed"
                    //sh "oc apply -f  deployment.ymln --kubeconfig=${OPENSHIFT_SECRET} -n elsayed" 
                   
                }
            }
        }
    }
    
}
