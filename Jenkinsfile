pipeline {
    agent any
    
    stages {

        stage('Checkout') {
            steps {
                git url: 'https://git@github.com:devops051033/jenkins-project.git', branch: 'basicJenkinsPiplineFromSCM'
                sh "ls -ltr"
            }
        }
        stage('Setup') {
            steps {
                sh "pip install -r requirements.txt"
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