pipeline{
    
    agent any
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/devopsbytanishka/node-app.git", branch: "main"
                echo 'My code has been cloned'
            }
        }
        
        stage("build and test"){
            steps{
                sh "docker build -t node-app ."
                echo 'code is build and tested'
            }
        }
        
        stage("push to dockerhub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser}  -p ${env.dockerHubPass}"
                sh "docker tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                sh "docker push ${env.dockerHubUser}/node-app:latest"
                echo 'image is pushed'
                }
            }
        }
        
        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo 'deployment is done'
            }
        }
    }
    
}
