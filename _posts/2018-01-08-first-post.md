---
layout: post
title: SSH Setup from Scratch
bigimg: /img/path.jpg
tags: [linux, ssh]
---
* Build aliases for first Access

```python
vim ~/.bash_aliases
alias yida = 'ssh yida@ip'
source ~/.bash_aliases
```
* Set SSH without password

```python
ssh-keygen
ssh-copy-id -i ~/.ssh/id_rsa.pub yida@ip
```
* SSH

```python
yida
```
