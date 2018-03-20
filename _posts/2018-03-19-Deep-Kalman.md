---
layout: post
title: Deep Kalman Programming
bigimg: /img/path.jpg
tags: [deep kalman, deep markov model,deep learning]
---
<table width="100%">
<tr>
<td>

* 程序入口输入参数：

```python
-vm LR
-infm mean-field
-dset  piano-sorted
```
</td>
</tr>
<tr>
<td>
* 参数汇总

```python
shuffle : False 
use_nade : False 
cov_explicit : False 
dataset : piano-sorted 
epochs : 2000 
seed : 1 
init_weight : 0.1 
reg_spec : _ 
expt_name : uid 
use_prev_input : False 
reg_value : 0.05 
dim_actions : 0 
reloadFile : ./NOSUCHFILE 
dim_stochastic : 100 
rnn_layers : 1 
transition_layers : 2 
lr : 0.0008 
reg_type : l2 
init_scheme : uniform 
replicate_K : None 
optimizer : adam 
data_type : binary 
use_generative_prior : False 
q_mlp_layers : 1 
maxout_stride : 4 
batch_size : 20 
savedir : ./chkpt-piano-sorted 
forget_bias : -5.0 
inference_model : mean_field 
emission_layers : 2 
savefreq : 25 
rnn_size : 600 
paramFile : ./NOSUCHFILE 
emission_type : mlp 
nonlinearity : relu 
dim_observations : 88 
rnn_dropout : 0.1 
ntrain : 5000 
dim_hidden : 200 
var_model : LR 
anneal_rate : 10.0 
debug : False 
validate_only : False 
transition_type : simple_gated 
unique_id : DKF_lr-8_0000e-04-vm-LR-inf-mean_field-dh-200-ds-100-nl-relu-bs-20-ep-2000-rs-600-ttype-simple_gated-etype-mlp-previnp-False-ar-1_0000e+01-rv-5_0000e-02-nade-False-nt-5000-uid 
leaky_param : 0.0 
```
</td>
</tr>
<tr>
<td>
* 有了数据库名称，进入loadDataset函数进行提取数据库

```python
dataset = loadDataset(params['dataset'])
```
调用之后返回

```python
_polyphonic(dataset)

```
</td>
</tr>
<tr>
<td>
  
在_polyphonic(dset)之中
首先调用 process._processPolyphonic(dset)
如果有数据库，那么什么都不做。如果没有数据库，那么进行产生数据库的准备

最后产生的数据：
数据是这样的
* 首先是由一个个样本构成的，每一个样本是一个sequence。
* 一个样本是由不同个时间点数据构成的。
* 一个时间点的数据是一组88维的向量， 每个位置取0或1代表是否有该键

* Mask：Number_Sequence, MAX_LENGTH （当某个样本（Sequence）,的某个时间点（T) 有数据时为1）
* Compileddata : Number_Sequence, MAX_LENGTH, DIM (当某个样本，的某个时间点的，某一个键有值时为1)
* 如果数据是sorted version的话，值的是根据时间点有值的多少，也就是Sequence的长度来搞事情的。

最终dataset的结果如下：
```python
datasets = {}
datasets['train']            = ff['train'].value
datasets['test']             = ff['test'].value
datasets['valid']            = ff['valid'].value
datasets['mask_train']       = ff['mask_train'].value
datasets['mask_test']        = ff['mask_test'].value
datasets['mask_valid']       = ff['mask_valid'].value
datasets['dim_observations'] = datasets['train'].shape[2]
datasets['dim_actions']      = 0
datasets['hasMasks']         = True
datasets['data_type']        = 'binary'
```
</td>
</tr>






