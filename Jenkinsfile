pipeline {
    agent any

    stages {

        stage("Code Clone") {
            steps {
                git branch: 'master', url: 'https://github.com/LondheShubham153/two-tier-flask-app.git'
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test") {
            steps {
                echo "Tests running..."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhubcreds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker tag two-tier-flask-app $USER/two-tier-flask-app
                    docker push $USER/two-tier-flask-app
                    '''
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d --build"
            }
        }
    }
}
