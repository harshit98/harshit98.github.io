---
layout: post
title: "GSoC 2018: Final Work Report"
date: 2018-08-10
category: archive
comments: true
author: "Harshit Prasad"
tags: [gsoc, final-report, neural-networks, software-engineer, cern]
---


![final_report_logo](https://drive.google.com/uc?export=view&id=1A1jd_QKDMEWe4froBjnUaxy171-cOY1l){:width="720px"}
{: style="text-align: center;"}

> Describes about my project status, future work and work experience with CERN mentors.

In this blog post, I’ll be summarising my GSoC 2018 project work. I was working on the project to develop an LSTM module for the CPU version and extend the TMVA-DNN library used in Particle Physics Applications like data processing, statistical analysis, and data visualization. The advantage of having LSTM module in TMVA-DNN will provide additional benefits of the machine learning environment in the ROOT framework for data analysis.

Previous blog posts in the past based on my project:-

- GSoC 2018: Starting with CERN — Part I
- GSoC 2018: RNN and LSTM Networks — Part II
- GSoC 2018: Forward Propagation in LSTM Network — Part III
- GSoC 2018: Backpropagation through time in LSTM Network — Part IV

You can checkout these blogs in the "Archive" section.

#### Current State of the Work

The current work of LSTM module includes the complete implementation of forward propagation and backpropagation feature. The LSTM layer was designed from scratch. The layer currently supports Reference and CPU architectures. For GPU, it’s still in progress and is a low priority at present.

1. The forward propagation has been implemented and it is working well for the CPU version. It takes time to compute the weights matrix for large data. There is a chance to optimize the performance for the large dataset as discussed during meetings.

2. The backward propagation has been implemented following the similar implementation design of the RNN backward pass method.

3. The current network design only updates internal parameters like state weights, input weights, and biases.

4. Benchmarked the performance on the ECAL dataset logged by the Large Hadron Collider (LHC). Achieved ~60% accuracy on test data. This accuracy can be improved by providing features such as training model in batches of data, experimenting and using correct optimizer and hyperparameters like learning rate and epochs.

#### Future Work

The network design of the LSTM layer and unit tests has been written by taking reference to ***torch-rnn*** module written in Lua.

The repository can be found here: [torch-rnn](https://github.com/jcjohnson/torch-rnn)

The future work related to LSTM layer design and improvements that can be done in the network are mentioned below:

1. Backpropagation design needs to be improved by showing updated values of each gate during the training process at each timestep.

2. Improving model accuracy by introducing training of the model using data in batches, correct use of hyperparameters like learning rate, epochs.

3. The model training session can also be improved by implementing and introducing new optimizers to achieve high training performance.

4. Implementation of GRU layer — a variant of LSTM networks.

### Conclusion and Experience

I have gained a lot of experience in the field of Deep Learning as this was my first intern in the field of Machine Learning. I got to know more about working principles in the field of Machine Learning. It is totally different from Software Development. Explored the best practices of development using C++14 and in-built libraries. It was a great experience working with CERN mentors. Their way of approach and communication is absolutely brilliant. Learned about Object-oriented programming and it’s best practices to follow within a software industry.

#### Special Mentions

[Lorenzo Moneta](https://www.linkedin.com/in/lorenzo-moneta-982b902/), [Saurav Shekhar](https://ch.linkedin.com/in/saurav-shekhar-16786958) and [Kim Albertsson](https://www.linkedin.com/in/kim-albertsson-a8946667/) for providing me the opportunity to work on this work project. All the help and suggestions provided by them during weekly meetings were really helpful to understand the project and made me forward towards the right direction of development.

It was a great experience working with my project partners who became really good friends at the end: [Siddhartha](https://ca.linkedin.com/in/siddhartha-rao-kamalakara), [Ravi](https://www.linkedin.com/in/ravikiran0606/), [Manos](https://www.linkedin.com/in/steremma/), and Anushree.
