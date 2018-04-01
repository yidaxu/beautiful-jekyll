---
layout: post
title: Tensorflow Usage
bigimg: /img/path.jpg
tags: [Tensorflow]
---

### 1 Variable Scope
本身变量名前加前缀
```Python
with tf.variable_scope("foo"):
    with tf.variable_scope("bar"):
        v = tf.get_variable("v", [1])
        assert v.name == "foo/bar/v:0"
```

#### 1.1 三种设置Reuse的方法

１．　利用　Auto Resuse

```Python
def foo():
  with tf.variable_scope("foo", reuse=tf.AUTO_REUSE):
    v = tf.get_variable("v", [1])
  return v

v1 = foo()  # Creates v.
v2 = foo()  # Gets the same, existing v.
assert v1 == v2
```

2. 利用　reuse = True

```Python
with tf.variable_scope("foo"):
    v = tf.get_variable("v", [1])
with tf.variable_scope("foo", reuse=True):
    v1 = tf.get_variable("v", [1])
assert v1 == v
```

3. 利用Scope Setting 

```Python
with tf.variable_scope("foo") as scope:
    v = tf.get_variable("v", [1])
    scope.reuse_variables()
    v1 = tf.get_variable("v", [1])
assert v1 == v
```
### 2 Tf.slim
#### 2.1 Argscope
除了　name_scope　和　variable_scope　以外，新加入的一种scope的mechanism.　能够让程序员方便的指定多层之间共享的参数．


格式如下
```python
with slim.arg_scope([层名字１，层名字２]，　共享参数一，共享参数二)：
    代码
```


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

上面的代码，conv2d 和 fully_connected　共享　activation, weights_initializer, weights_regularizer. conv2d独享 stride, padding.　另外可以通过local赋值，来单独设置某个操作的参数．

