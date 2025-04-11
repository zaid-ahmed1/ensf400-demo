pipeline {
    agent any

    environment {
        COMMIT_HASH = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image with tag: $COMMIT_HASH"
                    sh "docker build -t ensf400-demo:${COMMIT_HASH} ."
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests using Gradle...'
                sh './gradlew check'
            }
        }

        stage('Static Analysis with SonarQube') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv('SonarQubeServer') {
                    sh './gradlew sonarqube'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}