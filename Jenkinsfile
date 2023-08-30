pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Building the code...'
                echo "sh 'mvn clean package'"
            }
        }
        
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit tests...'
                echo "sh 'mvn test'"
                echo 'Running integration tests...'
            }

            post {
                failure {
                    emailext attachLog: true,
                            body: 'Unit and Integration Tests failed! Check the attachment for logs.',
                             subject: 'Unit and Integration Tests Failed',
                             to: 'mackenzie0175@gmail.com',
                             attachmentsPattern: '**/*.log'
                }
                success {
                    emailext attachLog: true,
                            body: 'Unit and Integration Tests succeeded! Check the attachment for logs.',
                             subject: 'Unit and Integration Tests Succeeded',
                             to: 'mackenzie0175@gmail.com',
                             attachmentsPattern: '**/*.log'
                }
            }
        }
        
        stage('Code Analysis') {
            steps {
                echo 'Analyzing the code...'
                echo "sh 'sonar-scanner'"

            }
        }
        
        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                echo "sh 'owasp-zap-command'"
                
            }
            post {
                failure {
                    emailext attachLog: true,
                            body: 'Security Scan failed! Check the attachment for logs.',
                            subject: 'Security Scan Failed',
                            to: 'mackenzie0175@gmail.com',
                            attachmentsPattern: '**/*.log'
                }
                success {
                    emailext attachLog: true,
                            body: 'Security Scan succeeded! Check the attachment for logs.',
                            subject: 'Security Scan Succeeded',
                            to: 'mackenzie0175@gmail.com',
                            attachmentsPattern: '**/*.log'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging server...'
                echo "sh 'aws s3 cp my-app.zip s3:staging-bucket/'"
                echo "sh 'aws ec2 create-instance '"
                }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                echo "sh 'run-integration-tests-command'"
                }
        }
        
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production server...'
                echo "sh 'aws s3 cp my-app.zip s3:production-bucket/'"
                echo "sh 'aws ec2 create-instance ...'"
            }
        }
    }
    
}
