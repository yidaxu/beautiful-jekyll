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
* Inference Network or Recognition Network:
 * Just a neutral netork which approximates the intractable posterior
 
### Nottion 
$$\theta$$ denote the parameter of generative model.
$\phi$ denote the parameter of inference network.
