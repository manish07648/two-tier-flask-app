pipeline {
    agent any

    stages {

        stage("Code") {
            steps {
                echo "code in progress......"
                git url: "https://github.com/manish07648/two-tier-flask-app.git", branch: "master"
                echo "cloning done"
            }
        }

        stage("Build") {
            steps {
                echo "Docker build bhi ho gaya"
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test") {
            steps {
                echo "testing bhi hogi"
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "docker hub pa push in progress....."

                withCredentials([usernamePassword(
                    credentialsId: "dockerhubcreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {

                    sh '''
                    echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin
                    docker tag two-tier-flask-app $dockerHubUser/two-tier-flask-app
                    docker push $dockerHubUser/two-tier-flask-app:latest
                    '''
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Docker Deploy ho raha ha"
                sh "docker compose up -d --build"
                echo "Deployment Done"
            }
        }
    }
}
