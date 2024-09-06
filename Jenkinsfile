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
                    bat 'mvn clean package'  // Use bat for Windows
                }
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    bat 'mvn test'  // Use bat for Windows
                }
            }
        }
        stage('Code Analysis') {
            steps {
                script {
                    echo 'Performing code analysis...'
                    // Tool: SonarQube
                    bat 'sonar-scanner'  // Use bat for Windows
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    // Tool: OWASP Dependency-Check
                    bat 'dependency-check --project MyProject --out dependency-check-report'  // Use bat for Windows
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging environment...'
                    // Tool: AWS CLI or similar
                    bat 'aws s3 cp target/myapp.jar s3://my-staging-bucket/'  // Use bat for Windows
                }
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    // Tool: Maven or similar
                    bat 'mvn verify'  // Use bat for Windows
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production environment...'
                    // Tool: AWS CLI or similar
                    bat 'aws s3 cp target/myapp.jar s3://my-production-bucket/'  // Use bat for Windows
                }
            }
        }
    }
    post {
        always {
            emailext(
                to: "${EMAIL_RECIPIENT}",
                subject: "Build ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
                body: """
                    Build Details:
                    - Job: ${env.JOB_NAME}
                    - Build Number: ${env.BUILD_NUMBER}
                    - Status: ${currentBuild.currentResult}
                    
                    Please find the attached log.
                """,
                attachmentsPattern: '**/logs/*.log'
            )
        }
    }
}
