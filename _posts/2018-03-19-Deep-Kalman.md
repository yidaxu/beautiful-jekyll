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
<tr>
<td>
DKF model
初始化

* 第一步要做的就是建立Parameters
 
 ```python
  npWeights: OrderedDict()  # A dictionary that remember the order it added
  self._createInferenceParams(npWeights)
  self._createGenerativeParams(npWeights)
  return npWeights
  ```
* createInferenceParams 详细求解

  这是 Transaction的程序解析，DIM_STOCHASTIC 就是z的维度

```python
  
  DIM_HIDDEN:200
  DIM_STOCHASTIC: 100
  
  如果 Transaction_type 是 mlp的话：
  DIM_HIDDEN_TRANS =  2 * 200
  Transition_layers:2
  0 层 input: dim_stachastic output: DIM_HIDDEN_TRANS
  1 层 input: DiM_HIDDEN_TRANS, DIM_HIDDEN_TRANS // getWeight return np array
  MU_COV_INP = DIM_HIDDEN_TRANS
  
  如果 Transaction_type 是 simple_gated
  g_gate: DIM_STOCHASTIC, DIM_HIDDEN_TRANS
  h_gate: DIM_STOCHASTIC, DIM-HIDDEN_TRANS
  g_gate1: DIM_HIDDEN_TRANS, DIM_STOCHASTIC
  h_gate1: DIM_HIDDEN_TRANS, DIM_STOCHASTIC
  MU_COV_INP = DIM_STOCHASTIC
  
  如果 Transaction_type 是 simple_gated
  MU/COV
  weight: DIM_STOVHSTIC, DIM_STOCHASTIC 
  bias:DIM_STOCHASTIC
  
  # 如果 Transaction_type 是 mlp
  weight: DIM_HIDDEN_TRANS, STochastic
  bias:DIM Stochastic 
``` 


  这是 Emission 的程序解析，DIM_STOCHASTIC 就是z的维度
  
  ```python
  
  如果 emission_type 是 mlp:
  假设 emission_type = 2
  第0层 : Dim_Stochastic, DIM_HIDDEN
  第1层 : DIM_HIDDEN. DIM_HIDDEN
  
  如果 emission_type 是 conditional
  第0层： Dimstochastic + Dim observations, DIM_HIdden
  第1层： Dim_Hidden, Dim_Hidden
  
  如果 data_type 是binary
  Emission: Dim_hidden, dim_observations
  ```
</td>
</tr>

<td>
<tr>
* 这是_createInferenceParams函数的解析求解
  
  第一层

```python
  
DIM_INPUT: DIM_OBSERVATION
RNN_SIXE = rnn_size
DIM_HIDDEN = RNN_SIZE
DIM_STOC = DIM_STOCHASTIC
第一层： dim_input: Dim_Observations
RNN_SIZE: rnn_size
DIM_HIDDEN:rnn_size
DIM_STOC: Dim_STOCHASTIc
由 Observation 到RNN的全连网络： dim_input, dim_output
````

第二层创建LSTM

```python

_createLSTMWeights
第一步判断LR
RNN_SIZE
for suffix in suffices
  for l in range //多少层
      W_lstm: RNN_SIZE, RNN_SIZE * 4
```

第三层

```python
如果是mean_field则不管
如果是 structured
DIM_INPUT = dim_Stocastics
如果有用到 generative_prior
Dim_INPUT = rnn_size
q_W_St_0 DIM_INPUT, rnn_size

```
第四层

```python
q_W_mu: RNN_SIZE, DIM_STOCHASTIC
q_W_cov: RNN_SIZE, DIM_STOCHASTIC
if var_model == LR
q_W+mu_r = self._getWeight((RNN_SIZE, dim_stochastic))
```

</tr>
</td>

