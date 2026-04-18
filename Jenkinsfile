pipeline {
    agent any

    tools {
        jdk 'Java21'
        maven 'Maven3'
    }

    stages {

        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t demo-app .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                    bat 'docker tag demo-app %DOCKER_USER%/demo-app'
                    bat 'docker push %DOCKER_USER%/demo-app'
                }
            }
        }
	stage('Deploy to AWS') {
    steps {
        bat '''
        echo Starting deployment...

        ssh -i "C:\\Users\\masth\\OneDrive\\Documents\\demo-key.pem" ^
        -o StrictHostKeyChecking=no ^
        ubuntu@18.190.152.242 "bash deploy.sh"
        '''
    }
}
        stage('System Check') {
            steps {
                bat 'whoami'
                bat 'java -version'
                bat 'mvn -v'
                bat 'docker version'
                bat 'where docker'
            }
        }
    }
}
