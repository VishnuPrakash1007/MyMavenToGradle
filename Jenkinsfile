pipeline {
    agent any

    tools {
        jdk 'JDK'          // ✅ Make sure this matches Jenkins config
        // No need to define Gradle if using wrapper (recommended)
    }

    environment {
        CHROME_BIN = '/usr/bin/google-chrome'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/VishnuPrakash1007/MyMavenToGradle.git'
            }
        }

        stage('Grant Permission') {
            steps {
                sh 'chmod +x gradlew'
            }
        }

        stage('Install Chrome Dependencies') {
            steps {
                sh '''
                echo "Checking Chrome..."
                google-chrome --version || true

                echo "Installing dependencies..."
                sudo apt-get update
                sudo apt-get install -y libxss1 libappindicator1 libindicator7 fonts-liberation libnss3 lsb-release xdg-utils
                '''
            }
        }

        stage('Build') {
            steps {
                sh './gradlew clean build -x test'
            }
        }

        stage('Run Automation') {
            steps {
                sh './gradlew run'
            }
        }
    }

    post {
        success {
            echo '✅ Build SUCCESS'
        }
        failure {
            echo '❌ Build FAILED'
        }
    }
}
