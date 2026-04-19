**Overview**

This project demonstrates the end-to-end deployment of a containerized Django web application using Docker on an AWS EC2 instance. It covers infrastructure setup, containerization, deployment, and network configuration to expose the application securely over the internet.

**Technology Stack**

Cloud Platform: AWS EC2 (Ubuntu 24.04)
Containerization: Docker
Backend: Python 3.11, Django
Version Control: Git, GitHub
Container Registry: Docker Hub  

**Architecture Workflow**
Launch EC2 instance (Ubuntu)
Install and configure Docker
Clone application repository
Build Docker image using Dockerfile
Run container and expose application
Configure Security Group for external access
Push image to Docker Hub

**Implementation Steps**


**1. Connect to EC2 Instance**
```bash
ssh -i sample-cli.pem ubuntu@<public-ip>
```
**2. Install Docker**
```bash
sudo apt update
sudo apt install docker.io -y
```
**3. Verify and Enable Docker**
```bash
sudo systemctl status docker
sudo usermod -aG docker ubuntu
```
Re-login is required after adding the user to the Docker group.

**4. Validate Installation**
```bash
docker run hello-world
```
**5. Clone Repository**
```bash
git clone https://github.com/jeevitha-y/docker-container-creation
cd docker-container-creation/example-project/python-web-app
```
**6. Authenticate with Docker Hub [Create an account with https://hub.docker.com/]**
```bash
docker login
```
```bash
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: jeevitha
Password:
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```
**7. Build Docker Image**

You need to change the username accoringly in the below command
```bash
docker build -t jeevithay/my-first-docker-image:v2 .
```
Output of the above command
``` bash
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
```
**8.Verify Docker Image is created**
```bash
docker images
```
Output
```bash
REPOSITORY                            TAG       IMAGE ID       CREATED          SIZE
jeevithay/my-first-docker-image:v2   latest    960d37536dcd   26 seconds ago   467MB
ubuntu                               latest    58db3edaf2be   13 days ago      77.8MB
hello-world                          latest    feb5d9fea6a5    1 months ago    13.3kB
```
**9. Run Docker Container**
```bash
docker run -it jeevithay/my-first-docker-image:v2
```
**10. Push the Image to DockerHub and share it with the world**
```bash
docker push jeevithay/my-first-docker-image:v2
```
Output
```bash
Using default tag: latest
The push refers to repository [docker.io/jeevithay/my-first-docker-image]
896818320e80: Pushed
b8088c305a52: Pushed
69dd4ccec1a0: Pushed
c5ff2d88f679: Mounted from library/ubuntu
latest: digest: sha256:6e49841ad9e720a7baedcd41f9b666fcd7b583151d0763fe78101bb8221b1d88 size: 1157
```
**Network Configuration (AWS Security Group)**

To enable external access to the application:
```bash
Rule Type	Protocol	Port	Source
Custom TCP	TCP	8000	0.0.0.0/0
```
Without this configuration, the application will not be accessible via browser.

**Application Access**
```bash
http://<EC2-Public-IP>:8000
```
