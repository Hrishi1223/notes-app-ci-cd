pipeline {
    agent {label'H'}

    stages {
        stage('cloning code ') {
            steps {
                git url: 'https://github.com/Hrishi1223/django-notes-app.git',branch:'main'
                echo 'code clone succesfull'
            }
        }
        stage('build docker image') {
            steps {
                bat 'docker build -t notes .'
                echo 'image build '
            }
        }
        stage('pushing to Docker Hub') {
            steps {
                withCredentials([usernamePassword
                (credentialsId:'dockerHubcredentials',
                passwordVariable:'dockerhubpass',
                usernameVariable:'dockerhubuser')]){
                script{
                try{    
                bat"docker login -u %dockerhubuser% -p %dockerhubpass%"
                bat "docker image tag notes:latest %dockerhubuser%/notes:latest"
                bat "docker push %dockerhubuser%/notes:latest"
                echo 'image is pushed to docker hub '
                }catch (Exception e){
                    error "Failed to push to Docker Hub: ${e.getMessage()}"
                    
                }
                }
                }
            }
        }
         stage('Deploying ') {
            steps {
                
               script {
                    bat 'docker run -d --name notes_app -p 8000:8000 hrishikesh23/notes:latest'
                } 
               

                echo 'dploy succesfully'
                
            }
        }
    }
}
