pipeline{
    agent any
    
    stages{
        stage("Code Clone"){
            steps{
                echo "Code Clone Stage"
                git url: "https://github.com/akshaypx9/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Code Build & Test"){
            steps{
                echo "Code Build Stage"
                sh "docker build -t node-app ."
            }
        }
        stage("Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhub",
                    usernameVariable:"dockerhubUser", 
                    passwordVariable:"dockerhubPass")]){
                sh 'echo $dockerhubPass | docker login -u $dockerhubUser --password-stdin'
                sh "docker image tag node-app:latest ${env.dockerhubUser}/node-app:latest"
                sh "docker push ${env.dockerhubUser}/node-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d --build"
            }
        }
    }
}
