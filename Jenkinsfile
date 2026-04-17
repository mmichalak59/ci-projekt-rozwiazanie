pipeline {
    agent any

    stages {
        stage('Info') {
            steps {
                echo "Build: ${env.BUILD_NUMBER}"
                sh 'whoami'
                sh 'cat /etc/os-release | grep -E "^NAME|^VERSION"'
            }
        }
        stage('Instalacja Python') {
            steps {
                sh 'echo "" | sudo -S apt-get install -y python3 2>/dev/null || echo "" | sudo -S yum install -y python3 2>/dev/null || echo "" | sudo -S dnf install -y python3'
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
        success { echo "Build ${env.BUILD_NUMBER} zakonczony sukcesem." }
        failure { echo "Build ${env.BUILD_NUMBER} zakonczony bledem — sprawdz logi." }
    }
}
