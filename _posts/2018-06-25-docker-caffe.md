---
layout: post
title: Horrible Caffe and Horrible Nvidia Driver Things
bigimg: /img/path.jpg
tags: [linux, caffe, nvidia, docker]
---
* To install cuda-9.0 
```python
vim ~/.bash_aliases
alias yida = 'ssh yida@ip'
source ~/.bash_aliases
```
* Set the horrible nvidia driver things.

Go to the following link [cuda-9.0](https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=deblocal)
```python
`sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb`
`sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub`
`sudo apt-get update`
`sudo apt-get install cuda-9-0`
```

* Install nvidia docker
Follow the link [nvidia docker](https://github.com/NVIDIA/nvidia-docker) to install nvidia docker

* Install caffer docker

```python
nvidia-docker run -ti bvlc/caffe:gpu caffe --version
```

* Most importance thing

To start docker, you need to use nvidia-docker, not only docker, baby.
```python
 nvidia-docker run -ti bvlc/caffe:gpu
```
