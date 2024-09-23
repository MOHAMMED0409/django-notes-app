pipeline {
    
    agent any
    
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/MOHAMMED0409/django-notes-app.git", branch: "main"
                echo "Repo is clonned"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t notes-app:latest"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId:"dockerhub",
                        passwordVariable:"dockerhubpass", 
                        usernameVariable:"dockerhubuser"
                        )
                    ]
                ){
                sh "docker image tag notes-app:latest ${env.dockerhubuser}/notes-app:latest"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push ${env.dockerhubuser}/notes-app:latest"
                }
            }
        }
        
        stage("Deploy"){
            steps{
                sh "docker-compose up -d"
            }
        }
    }
}
