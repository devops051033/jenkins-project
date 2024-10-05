pipeline {
    agent any
    options { skipDefaultCheckout() }
    stages {
        stage('Checkout') {
            steps {
                dir("/var/jenkins_home/workspace/${JOB_NAME}") {
                    git url: 'https://github.com/devops051033/jenkins-project.git', branch: 'basicJenkinsPiplineFromSCM'
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
        stage('Deploy'){
            steps{  
                dir("/var/jenkins_home/workspace/${JOB_NAME}") {
                    sh "python3 -m venv venv" // Create virtual environment
                    sh "bash -c 'source venv/bin/activate && pip install -r requirements.txt'" // Activate and install
                    sh "python3 app.py"
                }
            }
    
        }
    }
}