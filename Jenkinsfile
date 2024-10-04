pipeline {
    agent any
    
    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/devops051033/jenkins-project.git', branch: 'basicJenkinsPiplineFromSCM'
                sh "ls -ltr"
            }
        }
        stage('Setup') {
            steps {
                sh "pwd"
                sh "apt install python3-venv"
                sh "python3 -m venv venv"
                sh "source venv/bin/activate"
                sh "sudo pip install -r requirements.txt"
            }
        }
        stage('Test') {
            steps {
                sh "pytest"
                sh "whoami"
            }
        }
   
    }
}