---
layout: post
title: Tensorflow Slim Tutorial I: Arg_Scope
bigimg: /img/path.jpg
tags: [python, tensorflow, slim, arg scope]
---

### Scope

#### Arg Scope 可以设置默认参数

当很多个操作，都共享默认参数的时候, 用 Arg Scope 可以更加快捷的写代码。

```Python

net = slim.conv2d(inputs, 64, [11, 11], 4, padding='SAME',
                  weights_initializer=tf.truncated_normal_initializer(stddev=0.01),
                  weights_regularizer=slim.l2_regularizer(0.0005), scope='conv1')
net = slim.conv2d(net, 128, [11, 11], padding='VALID',
                  weights_initializer=tf.truncated_normal_initializer(stddev=0.01),
                  weights_regularizer=slim.l2_regularizer(0.0005), scope='conv2')
net = slim.conv2d(net, 256, [11, 11], padding='SAME',
                  weights_initializer=tf.truncated_normal_initializer(stddev=0.01),
                  weights_regularizer=slim.l2_regularizer(0.0005), scope='conv3')
```

上一段代码有很多共同的参数，比如 weights_initializer, weights_regularizer 都是一样的， 我们利用arg_scope就可以快速的设置默认参数了。

```Python

with slim.arg_scope([slim.conv2d], padding='SAME',
                      weights_initializer=tf.truncated_normal_initializer(stddev=0.01)
                      weights_regularizer=slim.l2_regularizer(0.0005)):
    net = slim.conv2d(inputs, 64, [11, 11], scope='conv1')
    net = slim.conv2d(net, 128, [11, 11], padding='VALID', scope='conv2')
    net = slim.conv2d(net, 256, [11, 11], scope='conv3')
```

* 这样我们就设置了slim.conv2d的默认参数，padding, weights_initializer, weights_regularizer
* 另外要注意的是，对于特殊的参数，我们可以强制复制哦。真是棒棒哒。

更棒的是，我们可以对于多个操作进行嵌套的使用。

```Python 
with slim.arg_scope([slim.conv2d, slim.fully_connected],
                      activation_fn=tf.nn.relu,
                      weights_initializer=tf.truncated_normal_initializer(stddev=0.01),
                      weights_regularizer=slim.l2_regularizer(0.0005)):
  with slim.arg_scope([slim.conv2d], stride=1, padding='SAME'):
    net = slim.conv2d(inputs, 64, [11, 11], 4, padding='VALID', scope='conv1')
    net = slim.conv2d(net, 256, [5, 5],
                      weights_initializer=tf.truncated_normal_initializer(stddev=0.03),
                      scope='conv2')
    net = slim.fully_connected(net, 1000, activation_fn=None, scope='fc')
```
上面的代码定义了两个操作的默认参数:slim.conv2d, slim.fully_connecte. 首先，我们通过第一个arg_scope定义了二者共同的默认参数，又通过第二层的嵌套定义了slim.conv2d自己的默认参数。
