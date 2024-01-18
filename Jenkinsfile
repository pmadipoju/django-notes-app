pipeline{
    agent any
    
    stages{
        stage("Code"){
            steps{
                echo "cloning the code "
                git url:"https://github.com/pmadipoju/django-notes-app.git", branch:"main"
            }
        }
        stage ("build"){
            steps{
                echo "build docker image "
                sh "docker build -t notes-app ."
            }
        }
        stage ("push to dockerhub"){
            steps{
                echo "pushing image to dockethub "
                withCredentials([usernamePassword(credentialsId:"DockerHub",passwordVariable:"DockerHubPass",usernameVariable:"DockerHubUser")]){
                   sh "docker tag notes-app ${env.DockerHubUser}/notes-app:latest"
                   sh "docker login -u $env.DockerHubUser -p $env.DockerHubPass" 
                   sh "docker push ${env.DockerHubUser}/notes-app:latest"
                }
                
                
            }
        }
        stage ("deploy"){
            steps{
                echo "Deploying the container "
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
