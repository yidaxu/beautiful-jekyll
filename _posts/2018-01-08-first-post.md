---
layout: post
title: Flake it till you make it
subtitle: Excerpt from Soulshaping by Jeff Brown
bigimg: /img/path.jpg
tags: [books, test]
---
### 建立alias快捷符号

```
vim ~/.bash_aliases

alias yida = 'ssh yida@ip'

source ~/.bash_aliases
```
### ssh 无密码登录

```
ssh-keygen
ssh-copy-id -i ~/.ssh/id_rsa.pub yida@ip
yida
```
