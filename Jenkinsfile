pipeline {
    agent any

    environment {
        // Set the environment variables if needed
        REPO_NAME = 'house-price-predictor'
        SSH_CREDENTIAL_ID = 'git@github.com:BharathiJotthshanaB/lab.git'
        SERVER_ADDRESS = 'jotthshana@192.168.1.10' // Replace with your server user and address
        DEPLOY_PATH = '/home/deployuser/AI' // Path on the remote server
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository from GitHub
                git 'https://github.com/BharathiJotthshanaB/lab.git' // Replace with your actual repo URL
            }
        }

        stage('Build') {
            steps {
                script {
                    // Example: build Docker image (if applicable)
                    sh '''
                        docker build -t ${lab} -f bharathi.Dockerfile .
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests (if you have any)
                    sh '''
                        python3 -m unittest discover
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sshagent (credentials: [git@github.com:BharathiJotthshanaB/lab.git]) {
                    sh '''
                        # Create the AI directory on the remote server if it doesn't exist
                        ssh ${SERVER_ADDRESS} 'mkdir -p ${DEPLOY_PATH}'

                        # Copy files from the Jenkins workspace to the remote server's AI directory
                        scp -r * ${SERVER_ADDRESS}:${DEPLOY_PATH}

                        # Execute the Python program on the remote server
                        ssh ${SERVER_ADDRESS} 'cd ${DEPLOY_PATH} && python3 House_Price_Prediction.py'
                    '''
                }
            }
        }
    }

    post {
        always {
            // Clean up actions, like archiving artifacts
            echo 'Cleaning up...'
            cleanWs()
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
