pipeline {
    agent any

    environment {
        TARGET_SERVER = "13.204.69.126"
        DEPLOY_DIR = "/var/www/html"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Dhanushrajan123/task.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building application"

                sh """
                    mkdir -p ${DEPLOY_DIR}
                """
            }
        }

        stage('Deploy') {
    steps {
        sh '''
        scp -r build ubuntu@13.204.69.126:/var/www/html/
        '''
    }
}
    }

    post {
        success {
            echo "Deployment Successful"
        }

        failure {
            echo "Deployment Failed"
        }
    }
}
