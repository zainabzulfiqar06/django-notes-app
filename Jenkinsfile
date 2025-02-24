pipeline {
    agent any

    stages {
        stage('Cloning repo') {
            steps {
                echo 'Cloning the repo'
                git url :"https://github.com/zainabzulfiqar06/django-notes-app.git", branch: "main"
            }
        }
        stage('Building app') {
            steps {
                echo 'Building the app'
                sh ' docker build -t django-notes-app . '
            }
        }
        stage('Push to docker hub') {
            steps {
                echo 'Pushing the image to docker hub'
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker tag django-notes-app ${env.dockerhubuser}/django-notes-app:latest"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"  
                sh "docker push ${env.dockerhubuser}/django-notes-app:latest "
                }
            }
        }
        stage('Deploying the app') {
            steps {
                echo 'Deploying the django-notes-app'
                sh "docker-compose down && docker-compose up -d"
               
            }
        }
    }
}
