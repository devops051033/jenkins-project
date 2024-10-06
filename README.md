RUN jenkins container so that it has python and virtual env

Modify Your Docker Image
You can create a custom Docker image based on the official Jenkins image and install the necessary packages. Here’s how to do it:

Create a Dockerfile: Create a Dockerfile in your project directory with the following contents:
Dockerfile
# =============
FROM jenkins/jenkins:lts
USER root
# Install Python and venv package
RUN apt-get update && \
    apt-get install -y python3 python3-pip python3-venv && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
USER jenkins
# =============

Build the Custom Image: Run the following command in the same directory where your Dockerfile is located:

docker build -t my-jenkins-image .
Run Jenkins Using the Custom Image: Use the custom image 

docker run -p 8080:8080 -p 50000:50000 -d \
  --user $(id -u):$(id -g) \
  -v jenkins_home:/var/jenkins_home \
  -v /home/user/jenkins_jobs:/var/jenkins_home/workspace \
  my-jenkins-image


# Use this configuration "flaskapp-systemd.service" in the host where you are doploying the app

[Unit]

Description=flask app
After=network.target

[Service]

User=ubuntu
Group=ubuntu
WorkingDirectory=/home/ubuntu/app/
Environment="PATH=/home/ubuntu/app/venv/bin"
ExecStart=/home/ubuntu/app/venv/bin/python3 /home/ubuntu/app/app.py

[Install]

WantedBy=multi-user.target