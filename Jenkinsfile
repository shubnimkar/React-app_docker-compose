pipeline {
    agent any
    stages {
        
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Git Checkout') {
            steps {
                git changelog: false, poll: false, url: 'https://github.com/shubnimkar/React-app_docker-compose.git'
            }
        }
        
        stage('Docker Compose Down') {
            steps {
                script {
                    
                    sh 'docker-compose down -d'
                }
            }
        }
        
        stage('Remove Images & Containers') {
            steps {
                script {
                    
                    sh 'docker rm -f $(docker ps -aq)'
                    sh 'docker rmi -f $(docker images -aq)'
                }
            }
        }
        
        
        stage('Docker Compose Up') {
            steps {
                script {
                    
                    sh 'docker-compose up -d'
                }
            }
        } 
        
        stage("Docker Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'b14ae725-ef7a-4db9-93e9-3c5c5d6d431d'){   
                       
                       sh "docker tag reactpipeline_nginx shubnimkar/reactpipeline_nginx:latest "
                       sh "docker tag reactpipeline_frontend shubnimkar/reactpipeline_frontend:latest "
                       sh "docker tag reactpipeline_backend shubnimkar/reactpipeline_backend:latest "
                       sh "docker push shubnimkar/reactpipeline_nginx:latest "
                       sh "docker push shubnimkar/reactpipeline_frontend:latest "
                       sh "docker push shubnimkar/reactpipeline_backend:latest "
                    }
                }
            }
        }
        
    }
}
