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
	stage('Docker Login Check') {
    steps {
        bat 'docker logout'
        bat 'docker login -u masthandocker01'
    }
}
	stage('Push Docker Image') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'docker-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {

            bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
            bat 'docker tag demo-app %DOCKER_USER%/demo-app'
            bat 'docker push %DOCKER_USER%/demo-app'
        }
    }
}
	stage('Build Docker Image') {
            steps {
                bat 'docker build -t demo-app .'
            }
        }
	stage('Push Docker Image') {
    steps {
        bat 'docker tag demo-app masthandocker01/demo-app'
        bat 'docker push masthandocker01/demo-app'
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
