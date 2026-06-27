pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/Dhanushrajan123/task.git'
        BRANCH = 'main'
        TARGET_IP = '13.235.135.40'
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
                    rsync -avz --delete \
                    ./
            ubuntu@13.235.135.40: /var/www/html/
                
              """
            }
        }
        stage('Restart Apache') {
            steps {
                sh """
                ssh ubuntu@13.235.135.40 "sudo systemctl restart apache2"
                """
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
