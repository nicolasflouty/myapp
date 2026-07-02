pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn -B clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging application...'
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Deploy to Test') {
            steps {
                echo 'Deploying to test environment...'
                sh '''
                    mkdir -p /tmp/deploy-test
                    cp target/myapp.jar /tmp/deploy-test/
                    echo "Deployed to test environment at /tmp/deploy-test"
                '''
            }
        }

        stage('Approval') {
            steps {
                echo 'Waiting for manual approval to deploy to production...'
                input message: 'Deploy to PRODUCTION?', ok: 'Deploy'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                sh '''
                    mkdir -p /tmp/deploy-prod
                    cp target/myapp.jar /tmp/deploy-prod/
                    echo "Deployed to production at /tmp/deploy-prod"
                '''
            }
        }
    }
}
