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
