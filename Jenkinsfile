pipeline {
    agent any

    stages {
        stage('Info') {
            steps {
                echo "Build: ${env.BUILD_NUMBER}"
                echo "Galaz: ${env.GIT_BRANCH}"
            }
        }
        stage('Instalacja Python') {
            steps {
                sh 'sudo apt-get install -y python3 || sudo yum install -y python3 || sudo dnf install -y python3'
                sh 'python3 --version'
            }
        }
        stage('Testy') {
            steps {
                sh 'python3 test_app.py'
            }
        }
        stage('Build') {
            steps {
                sh "docker build -t narzedzia:${env.BUILD_NUMBER} ."
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker stop app-demo || true'
                sh 'docker rm app-demo || true'
                sh "docker run -d --name app-demo -p 5000:5000 narzedzia:${env.BUILD_NUMBER}"
            }
        }
    }

    post {
        success {
            echo "Build ${env.BUILD_NUMBER} zakonczony sukcesem."
        }
        failure {
            echo "Build ${env.BUILD_NUMBER} zakonczony bledem — sprawdz logi."
        }
    }
}
