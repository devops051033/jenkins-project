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


       
    }
}