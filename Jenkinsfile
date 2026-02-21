pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'YOUR_GITHUB_REPO_LINK'
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-image ./backend'
            }
        }

        stage('Run Backend Container') {
            steps {
                sh 'docker run -d --name backend-container backend-image'
            }
        }

        stage('Run NGINX') {
            steps {
                sh 'docker run -d -p 80:80 -v $(pwd)/nginx/default.conf:/etc/nginx/conf.d/default.conf nginx'
            }
        }
    }
}
