pipeline{

    agent {label "Dev1"};

    stages{
        stage("code"){
            steps{
                git url : "https://github.com/AkshatJoshi-18/two-tier-flask-app", branch : "master"
                echo "clone done ...."
            }
        }
        
        stage("build"){
            steps{
                sh "docker build -t akshat8630/two-tier-flask-app:latest ."
                echo "build done ...."
            }
        }

        stage("docker hub"){
            steps{ withCredentials([usernamePassword(
                credentialsId : "dockerHub",
                passwordVariable : "dockerHubPass",
                usernameVariable : "dockerHubUser"
                )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }

        stage("test"){
            steps{
                echo "testing done ...."
            }
        }

        stage("deploy"){
            steps{
                sh "docker-compose down "
                sh "docker-compose up -d --build flask-app"
                echo "deploy done ...."
            }
        }
        
    }
}