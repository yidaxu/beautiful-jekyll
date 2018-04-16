---
layout: post
title: Docker Basic Command and Docker + Jupyter + Remotely.
bigimg: /img/path.jpg
tags: [linux, docker, tensorflow, jupyter]
---

### How to install docker
```python
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
apt-cache policy docker-ce
sudo apt-get install -y docker-ce
sudo systemctl status docker

```

### Executing the docker command without sudo 
```python
sudo systemctl status docker
su - ${USER}
id -nG
```
reference:https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04
### docker 常用命令

```Python 
# pull a docker image from a remote registry to a local machine
docker pull _image_name_
# List all images
docker images
# List All Containers
docker ps -a
# Stand Up a Docker Container (Running in the Background)
docker run -it -d _image_name_
# Log Into a Running Docker Container
docker exec -it _container_id_ bash
# Remove A Single Docker Image
docker rmi _image_name_
# Remove All Docker Images
docker rmi $(docker images -q)
# Stop All Docker Containers
docker stop $(docker ps -a -q)
# Delete All Docker Containers
docker rm $(docker ps -a -q)
# Commit Docker Container to an Image
docker commit _container_id_ _image_name_
# Push Image to Remote Registry
docker push _image_name_
# Copy a File From Host Into a Container
docker cp _file_ _container_id_:/
# Login
docker login
# Logout
docker logout
# Get ip address of container
docker inspect _container_id_ | egrep '"IPAddress":[^,]+'
```
Reference:https://askmacgyver.com/blog/tutorials/useful-docker-commands





### 在远程机器上运行的代码

```
sudo docker run it -p 8888:8888 gcr.io/tensorflow/tensorflow
# p 8888:8888  远程机的　port 8888　和　docker的port相联通．
```
### 在本地机器上的操作

```
ssh -N -f -L localhost:8888:localhost:8888 yida@remoteip
把本地机的port 8888　和　远程机的　port相联通
```


