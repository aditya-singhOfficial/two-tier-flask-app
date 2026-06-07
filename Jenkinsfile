pipeline{
    agent { 
        label 'dev'
    };
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/aditya-singhOfficial/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Trivy file system scan"){
            steps{
                sh "trivy fs . -o results.json"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Developer / Tester Testing......."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                credentialsId:"dockerHubCreds",
                passwordVariable: "dockerHubPass",
                usernameVariable: "dockerHubUser"
                )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    
    post{
        success{
            script{
                emailext from: 'adityasingh3902@gmail.com',
                to: 'adityasingh3902@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'adityasingh3902@gmail.com',
                to: 'adityasingh3902@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
