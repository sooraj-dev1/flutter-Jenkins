pipeline {
    agent any

    environment {
        PATH = "/opt/flutter/bin:${env.PATH}"
    }

    stages {
        stage('Flutter Doctor') {
            steps {
                sh 'flutter doctor -v'
            }
        }

        stage('Get Dependencies') {
            steps {
                sh 'flutter clean'
                sh 'flutter pub get'
            }
        }

        stage('Analyze Code') {
            steps {
                sh 'flutter analyze'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'flutter test'
            }
        }

        stage('Build Android APK') {
            steps {
                sh 'flutter build apk --debug'
            }
        }
    }

    post {
        success {
            echo "Flutter build and tests completed successfully!"
            archiveArtifacts artifacts: 'build/app/outputs/flutter-apk/*.apk', fingerprint: true, allowEmptyArchive: true
        }
        failure {
            echo "Pipeline failed! Please check logs for test or build errors."
        }
        always {
            sh 'flutter clean'
        }
    }
}