pipeline {
    agent any
    environment {
        EMAIL_RECIPIENT = 'dominicdiona@gmail.com'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the code...'
                    // Tool: Maven 
                    sh 'mvn clean package' 
                }
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    sh 'mvn test' 
                }
            }
        }
        stage('Code Analysis') {
            steps {
                script {
                    echo 'Analyzing code...'
                    // Tool: SonarQube
                    sh 'sonar-scanner' 
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    // Tool: OWASP Dependency-Check 
                    sh 'dependency-check.sh' 
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging server...'
                    // Tool: AWS CLI 
                    sh 'aws deploy --stage staging'
                }
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    sh 'mvn verify' 
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production server...'
                    sh 'aws deploy --stage production' 
            }
        }
    }
    post {
        success {
            emailext(
                to: "${EMAIL_RECIPIENT}",
                subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Good news! Build ${env.JOB_NAME} #${env.BUILD_NUMBER} was successful. Check the logs for details.",
                attachLog: true
            )
        }
        failure {
            emailext(
                to: "${EMAIL_RECIPIENT}",
                subject: "Build Failure: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Unfortunately, Build ${env.JOB_NAME} #${env.BUILD_NUMBER} failed. Please check the logs for details.",
                attachLog: true
            )
        }
    }
}


