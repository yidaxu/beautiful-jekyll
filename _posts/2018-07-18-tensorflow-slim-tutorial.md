---
layout: post
title: Tensorflow Slim Tutorial
bigimg: /img/path.jpg
tags: [python, tensorflow, slim]
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
