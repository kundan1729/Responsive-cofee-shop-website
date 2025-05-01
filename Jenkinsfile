pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/kundan1729/Responsive-cofee-shop-website.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Health Check') {
            steps {
                script {
                    sleep 5
                    def response = sh(script: "curl -s -o /dev/null -w \"%{http_code}\" http://localhost:3000", returnStdout: true).trim()
                    if (response != "200") {
                        error "App is not healthy. Status code: ${response}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Tearing down container...'
            sh 'docker-compose down'
        }
    }
}
