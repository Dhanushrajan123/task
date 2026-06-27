pipeline {
    agent any

    environment {
        SOURCE_SERVER = 'ubuntu@3.108.191.59'
        TARGET_SERVER = 'ubuntu@3.108.191.59'
        DEPLOY_PATH   = '/var/www/html'
        
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/your-repo/your-project.git'
            }
        }

        stage('Fetch from Source Server') {
            steps {
                echo 'Pulling latest files from source server...'
                sh """
                    scp -i ${SSH_KEY} -r ${SOURCE_SERVER}:${DEPLOY_PATH}/* ./site/
                """
            }
        }

        stage('Validate Files') {
            steps {
                echo 'Validating required files...'
                sh """
                    test -f ./site/index.html || { echo 'index.html not found!'; exit 1; }
                    test -d ./site/assets     || { echo 'assets folder not found!'; exit 1; }
                    test -d ./site/images     || { echo 'images folder not found!'; exit 1; }
                    echo 'Validation passed.'
                """
            }
        }

        stage('Deploy to Target Server') {
            steps {
                echo 'Deploying to target server...'
                sh """
                    # Backup existing deployment
                    ssh -i ${SSH_KEY} ${TARGET_SERVER} '
                        if [ -d ${DEPLOY_PATH} ]; then
                            sudo cp -r ${DEPLOY_PATH} ${DEPLOY_PATH}_backup_\$(date +%Y%m%d%H%M%S)
                        fi
                    '

                    # Copy files to target
                    scp -i ${SSH_KEY} -r ./site/* ${TARGET_SERVER}:${DEPLOY_PATH}/
                """
            }
        }

        stage('Set Permissions') {
            steps {
                echo 'Setting file permissions...'
                sh """
                    ssh -i ${SSH_KEY} ${TARGET_SERVER} '
                        sudo chown -R www-data:www-data ${DEPLOY_PATH}
                        sudo chmod -R 755 ${DEPLOY_PATH}
                    '
                """
            }
        }

        stage('Restart Web Server') {
            steps {
                echo 'Restarting nginx/apache...'
                sh """
                    ssh -i ${SSH_KEY} ${TARGET_SERVER} '
                        sudo systemctl reload nginx || sudo systemctl reload apache2
                    '
                """
            }
        }

        stage('Health Check') {
            steps {
                echo 'Running health check...'
                sh """
                    sleep 5
                    curl -f http://target-server-ip/index.html && echo 'Health check passed!' || {
                        echo 'Health check failed!'; exit 1;
                    }
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
            // mail to: 'team@example.com', subject: 'Deploy Success', body: 'Deployed successfully.'
        }
        failure {
            echo 'Deployment failed! Rolling back...'
            sh """
                ssh -i ${SSH_KEY} ${TARGET_SERVER} '
                    LATEST_BACKUP=\$(ls -td ${DEPLOY_PATH}_backup_* | head -1)
                    if [ -n "\$LATEST_BACKUP" ]; then
                        sudo rm -rf ${DEPLOY_PATH}
                        sudo cp -r \$LATEST_BACKUP ${DEPLOY_PATH}
                        echo "Rolled back to \$LATEST_BACKUP"
                    fi
                '
            """
        }
        always {
            cleanWs()
        }
    }
}
