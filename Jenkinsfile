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
                    // Create a virtual environment
                    sh "python3 -m venv venv"
                    // Activate the virtual environment and install requirements
                    sh """
                    source venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    """
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