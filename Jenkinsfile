pipeline {
    agent any

    environment {
        TARGET_SERVER = 'ubuntu@13.232.35.201'
        DEPLOY_PATH   = '/var/www/html'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Dhanushrajan123/task.git'
            }
        }

        stage('Deploy to Apache') {
            steps {
                sh """
                    scp -r * ${13.232.35.201}:${var/www/html}/
                """
            }
        }

    }
    post {
        success {
            echo 'Deployed successfully!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
