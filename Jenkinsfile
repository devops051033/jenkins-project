pipeline {
    agent any

    environment {
        SERVER_IP = credentials('app-server-ip')
    }
    options { skipDefaultCheckout() }

    stages {
        stage('Checkout') {
            steps {
                 dir("/var/jenkins_home/workspace/${JOB_NAME}"){
                    git url: 'https://github.com/devops051033/jenkins-project.git', branch: 'singleServerAppDeployment'
                    sh "ls -ltr"
                }
            }
        }
        stage('Setup') {
            steps {
                dir("/var/jenkins_home/workspace/${JOB_NAME}") {
                    sh "python3 -m venv venv" // Create virtual environment
                    sh "bash -c 'source venv/bin/activate && pip install -r requirements.txt'" // Activate and install
                }
            }
        }
        stage('Test') {
            steps {
                dir("/var/jenkins_home/workspace/${JOB_NAME}") {
                    sh "bash -c 'source venv/bin/activate && pytest'" // Activate and run tests
                    sh "whoami"
                }
            }
        }

        stage('Package code'){
            steps{
                echo "zipping application code"
                sh "zip -r myapp.zip ./* -x '*.git'"
                sh "ls -lart"
                echo "===present working directory ===="
                sh "pwd"
            }
        }

        stage('Deploy to Prod'){
            steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key',
                keyFileVariable: 'MY_SSH_KEY', usernameVariable: 'username')]) {
            sh '''
            pwd
            # Transfer the application zip file to the remote server
            scp -i $MY_SSH_KEY -o StrictHostKeyChecking=no myapp.zip ${username}@${SERVER_IP}:/home/ubuntu/

            # SSH into the remote server to perform the necessary steps
            ssh -i $MY_SSH_KEY -o StrictHostKeyChecking=no ${username}@${SERVER_IP} << EOF
                # Unzip the application
                unzip -o /home/ubuntu/myapp.zip -d /home/ubuntu/app/
                
                # Activate the virtual environment
                source new-venv/bin/activate
                
                # Change directory to the application folder
                cd /home/ubuntu/app
                
                # Install requirements
                pip install -r requirements.txt
                
                sudo systectl reload flaskapp.service
                # Restart the Flask service
                sudo systemctl restart flaskapp.service
EOF
            '''
        }
    }
        }


       
    }
}