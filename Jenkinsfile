pipeline {
    agent any

    environment {
        TARGET_SERVER = "13.127.119.157"
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
                sh """
                    echo "Deploying application..."

                    cp -r * ${DEPLOY_DIR}/

                    echo "Deployment completed."
                """
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
