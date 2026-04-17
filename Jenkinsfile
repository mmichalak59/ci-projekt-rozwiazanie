pipeline {
    agent any

    stages {
        stage('Info') {
            steps {
                echo "Build: ${env.BUILD_NUMBER}"
                echo "Galaz: ${env.GIT_BRANCH}"
            }
        }
        stage('Testy') {
            steps {
                sh 'python3 test_app.py'
            }
        }
        stage('Gotowe') {
            steps {
                echo 'Pipeline zakonczona.'
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
