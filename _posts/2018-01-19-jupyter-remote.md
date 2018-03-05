---
layout: post
title: Jupyter Notebook Remotely Connected
bigimg: /img/path.jpg
tags: [linux, jupyter]
---

### Jupyter Password
```python
jupyter notebook --generate-config
jupyter notebook password
```
### Jupyter remote

```
remote_user@remote_host$ ipython notebook --no-browser --port=8889

local_user@local_host$ ssh -N -f -L localhost:8888:localhost:8889 remote_user@remote_host

localhost:8888
```

https://coderwall.com/p/ohk6cg/remote-access-to-ipython-notebooks-via-ssh

https://www.digitalocean.com/community/tutorials/how-to-use-ssh-to-connect-to-a-remote-server-in-ubuntu