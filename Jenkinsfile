pipeline {
    agent any

    environment {
        TARGET_SERVER = 'ubutnu@13.232.35.201'
        DEPLOY_PATH   = '/var/www/html'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Dhanushrajan123/task.git'
        }

        stage('Deploy to apache') {
            steps {
                 
                    sh """
                                    ubuntu@13.232.35.201: /var/www/html/

                    """
                }
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

