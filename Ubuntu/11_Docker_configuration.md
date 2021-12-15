   - [Task](#task)
   - [Useful links](#useful)
   - [Close port 5000](#close-port)
   - [Manage Docker](#manage)
   - [Manage Docker as a non-root user](#non-root)
   - [Start containers automatically](#start-container)
   - [Stop all docker containers](#stop-all)
   - [Manage Docker Compose tool](#docker-compose)

---
### <a name="task" />Task
   - Docker configuration and administration.

---
### <a name="useful" />Useful Docker links

   - [How To Install and Use Docker on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)
   - [Post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall)
   - [Get Started, Part 1: Orientation and setup](https://docs.docker.com/get-started)
   - [Start containers automatically](https://docs.docker.com/config/containers/start-containers-automatically)

Docker Compose links:
   - [Docker compose tutorial for beginners by example [all you need to know]](https://youtu.be/4EqysCR3mjo)
   - [What is Docker Compose | How to create docker compose file](https://youtu.be/HUpIoF_conA)

---
### <a name="close-port" />Close port 5000

Close port 5000 for security reasons.
You should login via SSH graphical interface
`ssh -X user@website -p port` or via X2Go Client
and then use `http://localhost:5000` to connect to DIGITS.

```shell script
# Open port 5000 for NVIDIA DIGITS (not secure)
##sudo ufw allow 5000
# Close port 5000 for NVIDIA DIGITS
sudo ufw delete allow 5000
# Check the status
sudo ufw status
```

---
### <a name="manage" />Manage Docker
[Manage Docker containers](https://docs.docker.com/engine/reference/commandline/container/)

```shell script
# To show only running containers
docker ps
# or
docker container ls

docker container stop 6827ed3a784f  # stop container

# To show all containers
docker ps -a
# or
docker container ls -a

# Check status of the docker service. Press "Q" key to exit.
sudo systemctl status docker
# or
systemctl is-active docker 
```

---
### <a name="non-root" />Manage Docker as a non-root user

If you don’t want to preface the `docker` command with `sudo`,
create a Unix group called `docker` and add users to it.

:exclamation: **Only trusted users should be allowed
to control your Docker daemon.** :exclamation:

```shell script
# Show docker group
cat /etc/group | grep "docker"
# Create the docker group if it doesn't exist
sudo groupadd docker
# Add trusted users to the docker group.
# I don't know, is it possible to add existing group to docker group?
sudo usermod -aG docker $USER
# Log out and log back.

# Verify that you can run docker commands without sudo.
docker run hello-world

# To delete user from a docker group
sudo deluser username docker
```

---
### <a name="start-container" />Start containers automatically

It starts container, but not [Nvidia DIGITS](old/12_Nvidia_DIGITS.md) itself.

```shell script
# Autostart
docker run -dit --restart unless-stopped nvidia/digits:latest

# Stop autostart
docker run -dit --restart no nvidia/digits:latest
```

---
### <a name="stop-all" />Stop all docker containers

```shell script
sudo docker kill $(sudo docker ps -q)
```

---
### <a name="docker-compose" />Manage Docker Compose tool

[Docker Compose](https://docs.docker.com/compose) is a tool
for defining and running multi-container Docker applications.

```shell script
# Install Docker Compose tool. WARNING: current version could be different.
sudo curl -L https://github.com/docker/compose/releases/download/1.24.0-rc1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Test the installation
docker-compose --version
```
