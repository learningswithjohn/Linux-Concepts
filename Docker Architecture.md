Docker is a containerization platform that provides easy way to containerize your applications, which means, using Docker you can build container images, run the images to create containers and also push these containers to container regestries such as DockerHub, Quay.io and so on.

Here is the architecture of docker:

<img width="1009" height="527" alt="image" src="https://github.com/user-attachments/assets/f0967fd5-77ed-4b7d-84f4-3f8bdcdfd8d2" />

Docker LifeCycle
We can use the above Image as reference to understand the lifecycle of Docker.

There are three important things,

1. docker build -> builds docker images from Dockerfile
2. docker run -> runs container from docker images
3. docker push -> push the container image to public/private regestries to share the docker images.

Understanding the docker terminology:

Docker daemon:

The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

Docker client:

The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.

Docker Desktop:

Docker Desktop is an easy-to-install application for your Mac, Windows or Linux environment that enables you to build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (dockerd), the Docker client (docker), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper.

Docker registries:

A Docker registry stores Docker images. When you use the docker pull or docker run commands, the required images are pulled from your configured registry. When you use the docker push command, your image is pushed to your configured registry.

Dockerfile:

Dockerfile is a file where you provide the steps to build your Docker Image.

=================================================================================================================================================================

docker build -t john123/my-first-docker-image:latest .

Breakdown of the above command:

 -t is for tag, i mean tagging the image and fo versioning purpose.
 john123 :  It indicates that the image belongs to the user account or usernme or repository called john123
 my-first-docker-image : This is the actual image name
 latest : This is the tag name. we can replace/update it as v1.0, v1.1, v1.2 based on our versioning
 . (The Dot) : This is the build context. 

 Consider this is your project folder structure:

 <img width="821" height="230" alt="image" src="https://github.com/user-attachments/assets/7ec1c9e9-c245-494a-b302-8508cd4ac7ab" />

 Suppose your terminal is inside my-app and just run 'pwd' and suppose if it gives : /home/john/my-app

 Now if you run : docker build -t john123/my-first-docker-image:latest .
 This . (dot) tell docker Use the current directory /home/john/my-app as the build context.
 Docker sends everything in that directory (except files excluded by .dockerignore) to the Docker daemon.

 How does Docker use this context?

 In dockerfile, you will have an instruction called: COPY . .
 What it means is:
 First . → Copy from the build context (your current directory)
 Second . → Copy into the current working directory(what you define in WORKDIR) inside the image


 Note :

 You can also you another path instead of . (Dot) , like below:
 docker build -f /path/to/Dockerfile -t john123/my-first-docker-image:latest /path/to/project/source
 ====================================================================================================================================================================

