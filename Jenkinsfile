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

                    sh '''
scp -o StrictHostKeyChecking=no index.html ubuntu@10.1.0.249:/tmp/

ssh -o StrictHostKeyChecking=no ubuntu@10.1.0.249 << 'EOF'
sudo apt update -y
sudo apt install nginx -y
sudo rm -f /var/www/html/index.nginx-debian.html
sudo mv /tmp/index.html /var/www/html/index.html
sudo chown www-data:www-data /var/www/html/index.html
sudo chmod 644 /var/www/html/index.html
sudo systemctl restart nginx
EOF
                    '''
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
