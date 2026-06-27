pipeline {
    agent any

    environment {
        TARGET_SERVER = 'user@target-server-ip'
        DEPLOY_PATH   = '/var/www/html'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/your-project.git'
            }
        }

        stage('Deploy to apache') {
            steps {
                 
                    sh """
                                    ubuntu@13.235.135.40: /var/www/html/

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
