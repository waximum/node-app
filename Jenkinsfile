pipeline{
    
    agent any
    
    stages{
        stage("code"){
            steps{
                git url: "https://github.com/devopsbytanishka/node-app.git", branch:"main"
                echo 'Code is cloned'
            }
        }
        stage("build & test"){
            steps{
                sh "docker build -t node-app ."
                echo 'Build and tested successfully'
            }
        }
        stage("Push Img to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                    sh "docker push ${env.dockerHubUser}/node-app:latest"
                    echo 'Image pushed to DockerHub'
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo 'Deployment on Server'
            }
        }
        
    }
    
}
