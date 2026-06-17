pipeline {

    agent any

    environment {

        SERVER_IP = "10.1.0.249"

        DEPLOY_PATH = "/var/www/html"

    }
    stages {

        stage('Checkout') {

            steps {

                checkout scm

            }

        }

        stage('Verify Files') {

            steps {

                sh '''

                    pwd

                    ls -ltr

                '''

            }

        }

        stage('Deploy Website') {

            steps {

                sshagent(['Jenkins-ssh-key']) {

                    sh """

                    scp -o StrictHostKeyChecking=no \

                    index.html ubuntu@${SERVER_IP}:/tmp/

                    ssh -o StrictHostKeyChecking=no \

                    ubuntu@${SERVER_IP} '

                    sudo apt update

                    sudo apt install nginx -y

                    sudo rm -rf /var/www/html/index.nginx-debian.html

                    sudo mv /tmp/index.html ${DEPLOY_PATH}/index.html

                    sudo chown www-data:www-data ${DEPLOY_PATH}/index.html

                    sudo chmod 644 ${DEPLOY_PATH}/index.html

                    sudo systemctl restart nginx

                    '

                    """

                }

            }

        }

    }

    post {

        success {

            echo "Website deployed successfully"

        }

        failure {

            echo "Deployment failed"

        }

    }

}