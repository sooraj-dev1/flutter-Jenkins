pipeline {
    agent any

    environment {
        // If Flutter or Android SDK are in custom directories on your Jenkins server, specify them here:
        // FLUTTER_HOME = '/opt/flutter'
        // PATH         = "${FLUTTER_HOME}/bin:${PATH}"
        // ANDROID_HOME = '/opt/android-sdk'
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
                // Analyzes project using your analysis_options.yaml
                sh 'flutter analyze'
            }
        }

        stage('Run Tests') {
            steps {
                // Runs all tests located in the test/ folder
                sh 'flutter test'
            }
        }

        stage('Build Android APK') {
            steps {
                // Builds debug or release APK (change to --release if signing is configured)
                sh 'flutter build apk --debug'
            }
        }
    }

    post {
        success {
            echo "Flutter build and tests completed successfully!"
            // Archive the generated APK artifact
            archiveArtifacts artifacts: 'build/app/outputs/flutter-apk/*.apk', fingerprint: true, allowEmptyArchive: true
        }
        failure {
            echo "Pipeline failed! Please check logs for test or build errors."
        }
        always {
            // Optional: Clean build artifacts to conserve agent disk space
            sh 'flutter clean'
        }
    }
}