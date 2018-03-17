---
layout: post
title: 3D Model Generation
bigimg: /img/path.jpg
tags: [3D,deep learning,CV]
---
Gan 说白了就是有两个网络，一个是Generator G 一个是discriminator D. G 想创造出完美假货，让D看不出来。 D想做完美打假人，无论G如何努力，D都能知道
做出来的是假的。
<table width="100%">
<tr>
<td><img src="/img/gan11.png" alt="None" ></td>
</tr>


### Tensorflow 基础知识
如何定义变量
<table width="100%">
<tr>
<td colspan="2">两种定义变量的方法</td>
</tr>
<tr>
<td>tf.Variable()</td>
<td>tf.get_variable()</td>
</tr>
<tr>
<td>不可以指向以前已经定义的变量，重名了系统自动加后缀区分</td>
<td>可以指向以前已经定义的变量</td>
</tr>
<tr>
<td>tf.name_scope()对其影响</td>
<td>tf.name_scope()对其没有影响</td>
</tr>
  
### 重复利用变量
如达到想要重复利用变量的效果（也就是说一个变量出现在两个网络，这两个网络对这个变量都有影响），需要配合使用tf.get_variable()和tf.variable_scope()来提取变量。
* 不想tf.Variable()每次都产生新变量，tf.get_variable()如果需要了同样名字的变量时，它会单纯的提取这个同样名字的变量
* 但是这种情况必须要强调scope.reuse_variables()
  
 ### gan的套路
 * 定义generator
 ```Python
 def generator(z, out_dim, n_units=128, reuse=False, alpha=0.01):
    #确保 所有的变量以generator开头
    with tf.variable_scope('generator', reuse=reuse):
        # Hidden layer
        h1 = tf.layers.dense(z, n_units, activation=None)
        # Leaky ReLU
        h1 = tf.maximum(alpha * h1, h1)
        
        # Logits and tanh output
        logits = tf.layers.dense(h1, out_dim, activation=None)
        out = tf.tanh(logits) 
        return out
 ```
* 定义discriminator 
```Python

def discriminator(x, n_units=128, reuse=False, alpha=0.01):
    with tf.variable_scope('discriminator', reuse=reuse):
        # Hidden layer
        h1 = tf.layers.dense(x, n_units, activation=None)
        # Leaky ReLU
        h1 = tf.maximum(alpha * h1, h1)
        
        logits = tf.layers.dense(h1, 1, activation=None)
        out = tf.sigmoid(logits)
        
        return out, logits
```
* 建立模型
```python
#清楚以前的图，避免图的堆积
tf.reset_default_graph()
# Create our input placeholders
input_real, input_z = model_inputs(input_size, z_size)

# Build the model
# 建立generator, 输出是图片
g_model = generator(input_z, input_size, n_units=g_hidden_size, alpha=alpha)
# g_model is the generator output
# 建立第一个discriminator
d_model_real, d_logits_real = discriminator(input_real, n_units=d_hidden_size, alpha=alpha)
# 建立第二个discriminator 但是要共用参数
d_model_fake, d_logits_fake = discriminator(g_model, reuse=True, n_units=d_hidden_size, alpha=alpha)
```
* 计算损失和建立优化器。
```python

# Calculate losses
d_loss_real = tf.reduce_mean(
                  tf.nn.sigmoid_cross_entropy_with_logits(logits=d_logits_real, 
                                                          labels=tf.ones_like(d_logits_real) * (1 - smooth)))
d_loss_fake = tf.reduce_mean(
                  tf.nn.sigmoid_cross_entropy_with_logits(logits=d_logits_fake, 
                                                          labels=tf.zeros_like(d_logits_real)))
d_loss = d_loss_real + d_loss_fake

g_loss = tf.reduce_mean(
             tf.nn.sigmoid_cross_entropy_with_logits(logits=d_logits_fake,
                                                     labels=tf.ones_like(d_logits_fake)))
learning_rate = 0.002

# Get the trainable_variables, split into G and D parts
t_vars = tf.trainable_variables()
g_vars = [var for var in t_vars if var.name.startswith('generator')]
d_vars = [var for var in t_vars if var.name.startswith('discriminator')]

d_train_opt = tf.train.AdamOptimizer(learning_rate).minimize(d_loss, var_list=d_vars)
g_train_opt = tf.train.AdamOptimizer(learning_rate).minimize(g_loss, var_list=g_vars)
```
