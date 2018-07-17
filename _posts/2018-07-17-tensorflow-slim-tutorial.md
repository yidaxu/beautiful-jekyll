---
layout: post
title: Tensorflow Slim Tutorial II: Repeatedly perform the same operation.
bigimg: /img/path.jpg
tags: [python, tensorflow, slim, repeat, stack]
---


对于重复的操作，比如连续添加相同的卷积层，tf.slim 提供了更加便捷简单的方法。

#### 完全相同的方法(方法相同，参数相同) tf.slim.repeat

比如如下代码，在max_pool2d之前，神经网络加入了三个一模一样的conv2d操作。

```python
net = ...
net = slim.conv2d(net, 256, [3, 3], scope='conv3_1')
net = slim.conv2d(net, 256, [3, 3], scope='conv3_2')
net = slim.conv2d(net, 256, [3, 3], scope='conv3_3')
net = slim.max_pool2d(net, [2, 2], scope='pool2')
```

利用 repeat operation就特别简单了那。不仅可以一部调用多次相同操作，如slim.conv2d。而且还可以自己只能化得命名scope。
如下代码会自动对每一个slim.conv2d进行命名。具体为 ‘conv3/conv3_1’, 'conv3/conv3_2' 和 'conv3/conv3_3'

```python 
net = slim.repeat(net, 3, slim.conv2d, 256, [3, 3], scope='conv3')
net = slim.max_pool2d(net, [2, 2], scope='pool2')
```

####方法相同，参数不同 tf.slim.stack

```python
# Verbose way:
x = slim.fully_connected(x, 32, scope='fc/fc_1')
x = slim.fully_connected(x, 64, scope='fc/fc_2')
x = slim.fully_connected(x, 128, scope='fc/fc_3')

# Equivalent, TF-Slim way using slim.stack:
slim.stack(x, slim.fully_connected, [32, 64, 128], scope='fc')
```


```python
# Verbose way:
x = slim.conv2d(x, 32, [3, 3], scope='core/core_1')
x = slim.conv2d(x, 32, [1, 1], scope='core/core_2')
x = slim.conv2d(x, 64, [3, 3], scope='core/core_3')
x = slim.conv2d(x, 64, [1, 1], scope='core/core_4')

# Using stack:
slim.stack(x, slim.conv2d, [(32, [3, 3]), (32, [1, 1]), (64, [3, 3]), (64, [1, 1])], scope='core')
```
