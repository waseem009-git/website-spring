pipeline {
    agent any

    environment {
        GOOGLE_CREDENTIALS = credentials('gcp-service-account') // Reference the ID of your credential
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone your repository from GitHub and specify the branch if needed
                git branch: 'main', url: 'https://github.com/waseem009-git/website-spring.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the project using Maven
                    sh 'mvn clean package'
                }
            }
        }

        stage('Deploy to App Engine') {
            steps {
                script {
                    // Authenticate with Google Cloud using the service account credentials
                    withCredentials([file(credentialsId: 'gcp-service-account', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                        // Use the credentials to authenticate and deploy to Google App Engine
                        sh '''
                            gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                            gcloud app deploy --quiet
                        '''
                    }
                }
            }
        }
    }
}
