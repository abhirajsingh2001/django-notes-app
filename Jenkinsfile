pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                echo "Cloning the code"
                git url: "https://github.com/abhirajsingh2001/django-notes-app", branch: "main"
                echo "Code cloned"
            }
        }
        stage("Build"){
            steps{
                echo "Building the image"
                sh "docker build . -t django-notes-cicd:latest"
                echo "Build the image"
            }
        }
        stage("Push to Docker hub"){
            steps{
                echo "Pushing to Docker Hub"
                withCredentials([
                    usernamePassword(
                      credentialsId: "dockerHub",
                      usernameVariable: "dockerHubUsername",
                      passwordVariable: "dockerHubPassword"
                    )
                ])
                {
                    sh "docker tag  django-notes-cicd:latest ${dockerHubUsername}/django-notes-cicd:latest"
                    sh "docker login -u ${dockerHubUsername} -p ${dockerHubPassword}"
                    sh "docker push ${dockerHubUsername}/django-notes-cicd:latest"
                    
                }
                echo "Pushed to docker hub"
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the code"
                sh 'docker-compose down && docker-compose up -d'
                echo "Code Deployed"
            }
        }
    }
}
