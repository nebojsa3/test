pipeline {
    agent any  // Runs on any available agent/worker in Jenkins

    environment {
        // Define environment variables, like credentials for SSH and repository
        SERVER_IP = "198.23.134.29"  // Server IP
        REMOTE_DIR = "/var/www/html/test"  // Destination directory on the server
        SSH_CREDENTIALS = "git"  // The Jenkins credentials ID for SSH key access
        GIT_REPO = "https://github.com/nebojsa3/test.git"  // Your GitHub repository URL
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the GitHub repository
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Test') {
            steps {
                script {
                    // Example test: Check if the main PHP file exists
                    if (!fileExists('index.php')) {
                        error "index.php file is missing!"
                    }
                    
                    // Add any other tests or validations here
                    echo "Running tests (syntax checks, etc.)"
                    
                    // Example: Run PHP syntax check
                    sh 'php -l index.php'
                    
                    // You can add more complex tests here (like PHPUnit for PHP unit tests)
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the files to the remote server via SSH
                    echo "Deploying files to the server"

                    // Use SSH to copy files to the server
                    sh """
                        ssh -o StrictHostKeyChecking=no -i ${SSH_CREDENTIALS} user@${SERVER_IP} "
                            rm -rf ${REMOTE_DIR}/* && 
                            cp -r * ${REMOTE_DIR}/
                        "
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}
