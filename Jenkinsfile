pipeline {
    agent any

    stages {
        stage('Info') {
            steps {
                echo "Build: ${env.BUILD_NUMBER}"
                echo "Galaz: ${env.GIT_BRANCH}"
            }
        }
        stage('Build') {
            steps {
                sh "docker build -t narzedzia:${env.BUILD_NUMBER} ."
            }
        }
        stage('Testy') {
            steps {
                sh "docker run --rm narzedzia:${env.BUILD_NUMBER} python3 test_app.py"
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
