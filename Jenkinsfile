pipeline {
    agent any
    image: docker:19.03.12
     environment {
		DOCKERHUB_CREDENTIALS=credentials('docker-hub')
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
                sh 'docker build -t java-test .'
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
    }
}
  