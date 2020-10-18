---
layout: post
title: "GSoC 2018: Starting with CERN - Part-I"
date: 2018-05-18
category: archive
comments: true
author: "Harshit Prasad"
tags: [gsoc, internship, software-engineer, cern]
---


![cern](https://drive.google.com/uc?export=view&id=1bx-NfGaWu4r2PUY_DEBfn903lblKC1JZ){:width="720px"}
{: style="text-align: center;"}

Hi folks! I’m Harshit, a second year ECE undergrad student from LNMIIT, Jaipur, India. I’ve been selected in the Google Summer Of Code 2018 under CERN organisation this year. I’ll be working for the second time under this programme (GSoC) by Google.

Coming to my project, I’m selected in the TMVA team to develop Deep Learning Module for CERN. This module has to be integrated in Toolkit for Multivariate Analysis (TMVA) which is a multi-purpose machine learning toolkit integrated into the ROOT scientific software framework (quite a famous project of CERN) and is used in many particle physics applications like classification, tracking of particles, data-analysis etc.

My main work is to design Recurrent Neural Networks, further iterating the initial work by Saurav Shekhar (one of my mentor) and design LSTM networks. They have to be implemented supporting GPU architecture as well for which I’ll be using CUDA which is a parallel computing platform and API model created by Nvidia. The CPU architecture is based on BLAS library which is optimised for faster matrix multiplication. This year there are three students (including me) are selected in Deep Learning team. We all were going to work on CNNs but later in the meeting the work was distributed. Siddhartha will be working on Variational AutoEncoders, Manos will be working on CNNs and I’ll be working on RNNs and LSTM networks.

Recurrent Neural Network is a neural net that processes time series data that is what we call sequential data. It takes in as input - both the new input at the current time step and the output (or a hidden layer) of the net in the previous time step. The most popular type of RNN is probably the LSTM, which has a ‘cell state’ at each time step that changes with new input. For example: Large Hadron Collider (LHC) by CERN which is world’s largest and most powerful particle accelerator consist of the superconducting LHC magnets which are coupled with an electronic monitoring system which records and analyses voltage time series reflecting their performance. Here, RNN and LSTM networks can be helpful to monitor the performance of magnets easily.

In my next story, I’ll be discussing about the RNN and LSTM networks in more detail with a little more highlight on how I’ll be designing the BasicLSTMLayer class in TMVA.
