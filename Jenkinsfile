pipeline {
    agent any

    stages {

        stage('Create Network') {
            steps {
                sh '''
                docker network create labnet || true
                '''
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app backend'
            }
        }

        stage('Deploy Backends') {
            steps {
                sh '''
                docker rm -f backend1 backend2 || true
                docker run -d --network labnet --name backend1 backend-app
                docker run -d --network labnet --name backend2 backend-app
                sleep 5
                '''
            }
        }

        stage('Start NGINX') {
            steps {
                sh '''
                docker rm -f nginx-lb || true
                docker run -d --network labnet --name nginx-lb -p 80:80 nginx
                sleep 5
                docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf
                docker exec nginx-lb nginx -s reload
                '''
            }
        }

    }
}
