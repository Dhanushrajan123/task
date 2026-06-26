pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/username/repository.git'
        BRANCH = 'main'
        DEPLOY_DIR = '/var/www/html'
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Deploy to Apache') {
            steps {
                sh """
                    sudo rm -rf ${DEPLOY_DIR}/*
                    sudo cp -r * ${DEPLOY_DIR}/
                    sudo chown -R www-data:www-data ${DEPLOY_DIR}
                    sudo chmod -R 755 ${DEPLOY_DIR}
                """
            }
        }

        stage('Restart Apache') {
            steps {
                sh 'sudo systemctl restart apache2'
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful'
        }
        failure {
            echo 'Deployment Failed'
        }
    }
}
