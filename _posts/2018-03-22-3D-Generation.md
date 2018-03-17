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

