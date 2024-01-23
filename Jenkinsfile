pipeline {
    agent any
     environment {
		DOCKERHUB_CREDENTIALS=credentials('docker-hub')
	}
    
    stages {
        stage('Build') {
            steps {
                // Build your Docker image here
                sh 'docker build -t java-test .'
            }
        }
        stage('Docker Login') {
            steps {
              sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push') {
            steps {
                // Push the Docker image to Docker Hub
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker tag java-test:latest 10103040/java-test:latest'
                    sh 'docker push 10103040/java-test:latest'
                    sh 'docker logout'
                }
            }
        }
    }
}
