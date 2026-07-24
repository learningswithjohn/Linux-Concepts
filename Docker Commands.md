Below are some of the important docker commands:

1. docker images
   Lists docker images on the host machine.

2. docker ps
   Lists running containers on the host machine.

3. docker ps -a
   List running as well as stopped containers on the host machine.

4. docker build -t john123/my-docker-image:v1.1
   Builds an image from a dockerfile with image tag versioned as v1.1

5. docker history john123/my-docker-image:v1.1
   Shows every instruction line executed during the build phase and how much storage space each layer consumes.

6. docker run -d --name my-container -p 8080:80 john123/my-docker-image:v1.1
   Used to create and run container with name 'my-container'
   -d : Detached mode. Run the container in the background. (Run the container in background and gives you the shell prompt back)
   If no -d flag used, then you need to wait for your terminal until you stop the container.
   maps port 8080 on your host machine to port 80 inside the container.

7. docker exec -it my-container /bin/bash
   -i : interactive
   -t : terminal
   Basically it Opens a live terminal prompt inside your running 'my-container' container for troubleshooting.
   It is used to run commands inside your container.

8. docker logs --tail 50 -f my-container
   Fetch the last 50 lines of logs from a container and follow live logs.

9. docker rm -f my-container
   This removes a stopped container
   Directly you can not remove a container. you need to stop first and then run the above command to remove the container.

10. docker rmi john123/my-docker-image:v1.1
    This will remove the specified image from the host machine.

11. docker system prune -a --volumes -f
    By default without any flags/options (docker system prune), docker removes : Stopped containers, Dangling images, Unused networks, Build cache. It does not removes Running containers, Images currently used by containers, Named volumes.
    -a : Remove ALL unused images, not just dangling images.
    By default, docker does not remove volumes. Because volumes usually contain important data.
    But since the command specifies --volumes , Docker also deletes unused volumes.
    -f : Basically while running system prune, docker asks/warns/prompts before removing, with usage of -f docker forcefully removes without any prompt.

12. docker pull john123/my-docker-image:v1.1
    docker push john123/my-docker-image:v1.1
    The above commands will pull and push the images.

13. docker save -o my-app-backup.tar john123/my-docker-image:v1.1
    This command takes an image that currently sits on your machine and bundles all of its configuration data, history metadata, and filesystem layers into a single .tar archive file.
    -o (or --output): Specifies the name of the file you want to create (e.g., my-app-backup.tar)

14. docker load -i my-app-backup.tar
    Once you transfer that .tar file to a completely different computer (even one without an internet connection), you use docker load to unpack it straight into that new machine's local Docker engine.
    -i (or --input): Specifies the path to the backup .tar file you want to read from.

The docker save and docker load commands are used to share Docker images completely offline without using a network-based cloud registry (like Docker Hub).

====================================================================================================================================================================

Docker troubleshooting commands:

1. docker inspect my-container
   The above command Returns detailed JSON information about a container or image.
   Container's IP Address, Mounts, Volumes, Port mappings, Network configuration etc

   How do you find the IP Address of a container?
   docker inspect my-container | grep IPAddress

2. docker stop my-container
   docker start my-container
   docker restart my-container
   The above commands are used to stop/start/restart the container

3. docker cp test.txt my-container:/tmp
   It copies a file named test.txt from your host machine's current working directory directly into the /tmp folder inside a running or stopped container named my-container.

4. docker stats
   This command shows LIVE CPU, Memory, Disk I/O, Network. Very useful in production.

5. docker network ls
   Lists Docker networks.

6. docker network inspect bridge
   This command shows Connected containers, Subnet, Gateway etc.

7. docker volume ls
   Lists docker volumes









   
