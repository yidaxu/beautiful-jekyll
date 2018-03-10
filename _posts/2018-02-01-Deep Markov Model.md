---
layout: post
title: Deep Markov Model
bigimg: /img/path.jpg
tags: [Deep Learning, Kalman, Machine Learning , Note]
---
### Deep Markov models
When the generation model is unkown, we call it deep markov models.
Linear Emisssion is replaced by a multi-layer perceptrons(MLPS)
Transition Distribution is replaced by a multi-layer perceptrons(MLPS)

### Learning Algorithm
* Stochastic Gradient on a variational lower bound of the likelihood.

### Structured inference networks (模拟后验概率分布P(z|x))
* Parameterized by recurrent neural networks
  * The generative model is known and fixed
  * In prameter estimation when the funcional form of the model is known
  * Learning deep Markov models.
  
### Experiment 
* Ask question: what would have happended to patients had they not received treatement.
* Identifies the way certain medications affect a patient's health.


## Background
### Gaussian State Space Models (GSSM)
In other words, Kalman filter.
* Transition (转移模型 / 状态方程)  and Emission (传播模型 / 测量方程)
```python
 state_current ~ N (fn(state_previous), fn2(state_previous)) (状态方程)
 observation_current ~ Distribution(fn3(state_previous)) （测量方程）
```
### Variational Learning 
#### Inference Network or Recognition Network:
 * Just a neutral netork which approximates the intractable posterior
#### 用来模拟后验概率的函数的形式，是一个正态分布家族，其均值和方差由神经网络提供。
* 我们要通过q(z) = Gaussion(z) 来模拟posterior p(z|x) 在观测数据已知的情况下，隐藏状态出现的概率
* q(z)是一个正态分布，均值是由一个神经网路的输出决定的，方差是另一个神经网络的输出，而这连个神经网络的输入则是观测到的数据x
* 因此q(z)和观测数据，两个神经网络的参数相关


#### 结构特征
* 神经网络的输入时什么那？
* 根据 Deep Kalman Model的模型 我们可以推出后验概率密度的结构特征
      
 * p(所有状态|所有观察）= p(第一个个状态|所有观察) * p(第二状态|第一个状态， 第二个观察开始的所有观察) * 。。。 * p(最后一个状态|最后第二状态，最后一个观察)
 因此我们定义模拟概率的形式也是这样的。
 * q(所有状态|所有观察) = q（第一个状态|所有观察） * q(第二个状态|第一个状态，第二个观察开始的所有观察) * 。。。 * q(最后一个状态|最后第二状态，最后一个观察）
 * q(第i个状态|第i-1个状态，第i开始的所有观察) 是一个高斯分布
       * 高斯分布的均值是由神经网路1提供，其输入是（第i-1个状态，第i 开始的所有观察）
       * 高斯分布的方差是由神经网络2提供的，其输入是（第i-1个状态，第i开始的所有观察）
 * 尽管q是有所有观察的信息的，但是p的结构暗示我们此种q的结构是充足的
 
#### 如何优化神经网络的参数，从而逼近？
##### 目标函数的确立
* 两个概率越近，则KL divergence越低。
* 但是因为不知道后验概率因此不可以求。
* 转换成求ELBO， 因为ELBO是期望，可以通过采样的方法来观测均值，因此是可以求得。
* 因此优化ELBO，就相当于使两个概率分布越来越近。
* 两点是把最后一项KL的均值，通过我们之前定义的结构特征来分解成不同的小的均值，因此可以更好的更稳定的求解（有解析式对于简单的特征。）
 ##### 如何学习
 * 目标函数对于Generative Model(我们的Kalman) 和 Inference Model (我们的q)的参数都是可导的
 * 如果Generative Model 是固定的，我们只针对 Inference Model求导
 * 如果Generative Model 不固定，我们则针对两者都求导
 * 求导方式
     * 利用Stochastic Backpropogation 来进行优化Inference Model
     * 期望部分用抽样，KL部分有解析解
     
 #### 整体算法
 
* 输入：
   * 数据库
   * Inference Model (神经网络的结构）
   * Generative Model (kalman filter 的结构)
* 算法：重复以下过程，直到收敛
   * 选取一个数据点（第一个观察，第二个观察，。。。。。。，第n个观察）
   * 求估计的后验概率密度q的参数， 也就是所把观察，和前一时刻的状态送进神经网络。
   * 得到了估计的后延概率分布，我们队其进行抽样，得到一个潜在状态（第一个状态，第二个状态，。。。。，第n个状态）
   * 根据生成模型，我们可以计算出一个只和观察相关的概率 和 KL
   * 把x带入概率和KL，我们可以计算出目标函数 (只和 相关参数相关)
   * 求解 Drevatrive
   * 利用 Adam进行更新。
 
### Nottion 
Theta denote the parameter of generative model.
Pi denote the parameter of inference network.
