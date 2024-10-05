pipeline {
    agent any
    
    stages {

        stage('Checkout') {
            steps {
                dir("/var/jenkins_home/workspace/${JOB_NAME}") { // Path inside the container
                    git url: 'https://github.com/devops051033/jenkins-project.git', branch: 'basicJenkinsPiplineFromSCM'
                    sh "ls -ltr"
                }
            }
        }
        stage('Setup') {
            steps {
                dir("/var/jenkins_home/workspace/${JOB_NAME}") { // Use JOB_NAME variable
                    sh "pip install -r requirements.txt"
                }
            }
        }
        stage('Test') {
            steps {
                dir("/var/jenkins_home/workspace/${JOB_NAME}") { // Use JOB_NAME variable
                    sh "pytest"
                    sh "whoami"
                }
            }
        }
   
    }
}