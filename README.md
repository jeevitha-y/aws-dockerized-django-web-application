📖 Overview

This project demonstrates the end-to-end deployment of a containerized Django web application using Docker on an AWS EC2 instance. It covers infrastructure setup, containerization, deployment, and network configuration to expose the application securely over the internet.

🧰 Technology Stack
Cloud Platform: AWS EC2 (Ubuntu 24.04)
Containerization: Docker
Backend: Python 3.11, Django
Version Control: Git, GitHub
Container Registry: Docker Hub
🏗️ Architecture Workflow
Launch EC2 instance (Ubuntu)
Install and configure Docker
Clone application repository
Build Docker image using Dockerfile
Run container and expose application
Configure Security Group for external access
Push image to Docker Hub
⚙️ Implementation Steps
1. Connect to EC2 Instance
ssh -i sample-cli.pem ubuntu@<public-ip>
2. Install Docker
sudo apt update
sudo apt install docker.io -y
3. Verify and Enable Docker
sudo systemctl status docker
sudo usermod -aG docker ubuntu

Re-login is required after adding the user to the Docker group.

4. Validate Installation
docker run hello-world
5. Clone Repository
git clone https://github.com/jeevitha-y/docker-container-creation
cd docker-container-creation/example-project/python-web-app
6. Authenticate with Docker Hub
docker login
7. Build Docker Image
docker build -t jeevithay/my-first-docker-image:v2 .
📦 Build Output (Excerpt)
Sending build context to Docker daemon  44.54kB
Step 1/7 : FROM python:3.11-slim
Step 2/7 : WORKDIR /app
Step 3/7 : COPY requirements.txt .
Step 4/7 : RUN pip install -r requirements.txt
Step 5/7 : COPY django/ /app/
Step 6/7 : EXPOSE 8000
Step 7/7 : CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
Successfully built 0f35be9da960
Successfully tagged jeevithay/my-first-docker-image:v2
8. Run Docker Container
docker run -p 8000:8000 jeevithay/my-first-docker-image:v2
🌐 Network Configuration (AWS Security Group)

To enable external access to the application:

Rule Type	Protocol	Port	Source
Custom TCP	TCP	8000	0.0.0.0/0

Without this configuration, the application will not be accessible via browser.

🌍 Application Access
http://<EC2-Public-IP>:8000
🔍 Operational Commands
docker ps
docker ps -a
docker images
docker inspect <image-id>
📤 Push Image to Docker Hub
docker push jeevithay/my-first-docker-image:v2
⚠️ Troubleshooting
Permission Denied (Docker Socket)
sudo usermod -aG docker ubuntu
Docker Service Not Running
sudo systemctl start docker
Application Not Accessible
Ensure port 8000 is open in Security Group
Verify container is running using docker ps
