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
            }
        }

        stage('Deploy to Prod'){
            steps{
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key',
                keyFileVariable:'MY-SSH-KEY', usernameVariable: 'username')]){
                    sh '''
                    scp -i $MY-SSH-KEY -o StrictHostKeyChecking=no myapp.zip 
                    ${username}@${SERVER_IP}:/home/ubuntu/

                    ssh -i $MY-SSH-KEY -o StrictHostKeyChecking=no ${username}@${SERVER_IP} <<
                    EOF
                        unzip -o /home/ubuntu/myapp.zip -d /home/ubuntu/app/
                        source app/venv/bin/activate
                        cd /home/ubuntu/app
                        pip install -r requirements.txt
                        sudo systemctl restart flaskapp.service

                    '''
                }
            }
        }


       
    }
}