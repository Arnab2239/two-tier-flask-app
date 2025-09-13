pipeline {
    agent { label "dev" }

    stages {

        stage('Code Clone') {
            steps {
                git url: 'https://github.com/Arnab2239/two-tier-flask-app.git', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-app .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerUserPass1',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}"
                    sh "docker image tag my-app ${DOCKER_USER}/flask-app:latest"
                    sh "docker push ${DOCKER_USER}/flask-app:latest"
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    post {
        success {
            script {
                emailext from: "devopsarnab08@gmail.com" ,
                    subject: "two-tier-flask-app deploy successfully" ,
                    body: "build successfuly" ,
                to: "devopsarnab08@gmail.com" ,
                mimeType: 'text/html'
            }
        }
        failure {
            script {
                emailext from: "devopsarnab08@gmail.com" ,
                    subject: "two-tier-flask-app deploy faild" ,
                    body: "build faild" ,
                to: "devopsarnab08@gmail.com" ,
                mimeType: 'text/html'
            }
        }
    }
}
