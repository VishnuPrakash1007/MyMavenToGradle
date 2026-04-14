pipeline {
    agent any

    tools {
        jdk 'JDK'   // Must match Jenkins Global Tool Configuration
    }

    environment {
        CHROME_BIN = '/usr/bin/google-chrome'
        GRADLE_OPTS = '-Dorg.gradle.daemon=false'
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
                sudo apt-get update -y
                sudo apt-get install -y \
                    libxss1 \
                    libappindicator1 \
                    libindicator7 \
                    fonts-liberation \
                    libnss3 \
                    lsb-release \
                    xdg-utils
                '''
            }
        }

        stage('Upgrade Gradle Wrapper') {
            steps {
                sh '''
                echo "Upgrading Gradle Wrapper..."
                sed -i 's/gradle-4.4.1-bin.zip/gradle-7.6-bin.zip/g' gradle/wrapper/gradle-wrapper.properties
                ./gradlew --version
                '''
            }
        }

        stage('Build') {
            steps {
                sh './gradlew clean build -x test --no-daemon'
            }
        }

        stage('Run Automation') {
            steps {
                sh './gradlew run --no-daemon'
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
        always {
            echo '📌 Pipeline execution completed'
        }
    }
}
