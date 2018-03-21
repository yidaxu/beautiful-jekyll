---
layout: post
title: 3D Model Generation
bigimg: /img/path.jpg
tags: [3D,deep learning,CV]
---


Gan 说白了就是有两个网络，一个是Generator G 一个是discriminator D. G 想创造出完美假货，让D看不出来。 D想做完美打假人，无论G如何努力，D都能知道
做出来的是假的。
<div>
<table width="100%">
<tr>
<td><img src="/img/gan11.png" alt="None" ></td>
</tr>
</div>  

  
### 重复利用变量
parameter table
<table width="100%">
<tr>
<td >缩写</td>
<td >全称</td>
<td >默认值</td>
<td >意思</td>
</tr>

<tr>
<td >n</td>
<td >name</td>
<td >test</td>
<td >当前实验的名称，用于创建存放models的路径</td>
</tr>

<tr>
<td >d</td>
<td >data</td>
<td >data/train/chair</td>
<td >数据存放的位置（voxel models）</td>
</tr>

<tr>
<td >e</td>
<td >epochs</td>
<td >1500</td>
<td >需要run多少个epochs</td>
</tr>
<tr>
<td >b</td>
<td >batchsize</td>
<td >256</td>
<td >Batch Size</td>
</tr>
### Models

<table width="100%">
<tr>
<td > input size [batchsize, output_size, output_size, outputsize] name = real_models</td>
</tr>
<tr>
<td >  z [batchsize, 200]</td>
</tr>

<tr>
<td >
在model.py 之中的generator作如下问题 


</td>
</tr>
