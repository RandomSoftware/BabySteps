pipeline {
    agent any

    tools {
        maven 'Maven 3.8.5'
        jdk 'JDK 11'
    }
    triggers {
        pollSCM('H/1 * * * *') // проверка каждые 5 минут
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RandomSoftware/BabySteps.git'
            }
        }
        stage('Publish Test Reports') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }
        stage('Build & Test') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Сборка, тесты и архивирование прошли успешно.'
        }
        failure {
            echo 'Ошибка во время сборки или тестов.'
        }
    }
}
