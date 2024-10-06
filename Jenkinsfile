pipeline {
    agent any

    environment {
        SERVER_IP = credentials('app-server-ip')
        IMAGE_NAME = 'samdocker33/kk-flask-app'
        IMAGE_TAG = "${IMAGE_NAME}:${env.GIT_COMMIT}"
    }
    options { skipDefaultCheckout() }

    stages {
        stage('Checkout') {
            steps {

                    git url: 'https://github.com/devops051033/jenkins-project.git', branch: 'appCodeDocarize'
                    sh "ls -ltr"
                    echo "The current commit hash is: ${env.GIT_COMMIT}"

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

        stage('Login to docker hub') {
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred',
                usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin'
                }
                echo 'Login successfully'
            }
        }

        stage('Build Docker Image'){
            steps{
                
                sh 'docker build -t ${IMAGE_TAG} .'
                echo "Docker image build successfully"
                sh 'docker image ls'
            }
        }
    }
}