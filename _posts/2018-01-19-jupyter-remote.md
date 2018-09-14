---
layout: post
title: Jupyter Notebook Remotely Connected
bigimg: /img/path.jpg
tags: [linux, jupyter]
---

### 第一步 如何创建 Jupyter Notebook 密码
```python
jupyter notebook --generate-config
jupyter notebook password
```
### 第二步 登录远程主机
```bash
ssh user_name@ip_address 
```
### 第三步 在远程主机上运行 Jupyter Notebook
```bash
（remote_user@remote_host）
ipython notebook --no-browser --port=8889
```
### 第四步 在本地主机上建立远程链接
```bash
（local_user@local_host）
ssh -N -f -L localhost:8888:localhost:8889 remote_user@remote_host
```
### 补充 如何 kill port
```bash
lsof -n -i4TCP:8080 
```

[Reference 1](https://coderwall.com/p/ohk6cg/remote-access-to-ipython-notebooks-via-ssh)

[Reference 2](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-to-connect-to-a-remote-server-in-ubuntu)

[Reference 3](https://stackoverflow.com/questions/24387451/how-can-i-kill-whatever-process-is-using-port-8080-so-that-i-can-vagrant-up)
