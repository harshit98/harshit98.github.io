---
layout: post
title: "GSoC 2018: RNN and LSTM Networks - Part II"
date: 2018-05-23
category: archive
comments: true
author: "Harshit Prasad"
tags: [gsoc, rnn, lstm, neural-networks, software-engineer, cern]
---


![lstm](https://drive.google.com/uc?export=view&id=1uaDaloW8Ecdli-N4w2N-FLJ5T_8XZlRu){:width="720px"}
{: style="text-align: center;"}

> Getting a brief idea about how Recurrent Neural Network works along with explanation of RNN layer structure in TMVA module.

In the first blog, I described about my project on which I’ll be working this summer. You can find my part-I blog here. In this blog, I’ll be discussing about structure of RNN and LSTM networks in TMVA-DNN. The current structure includes RNN network with TBasicRNNLayer class. The layer is fully functional with forward and backward propagation. Soon, I’ll be going to implement LSTM layer with such features and post a blogpost highlighting each feature.

There are tons of resources to learn about RNN and LSTM networks but I found this blog post: The Unreasonable Effectiveness of Recurrent Neural Networks by Andrej Karpathy which is quite good for understanding Recurrent Neural Networks along with this blog-post: Understanding LSTMs by Christ Olah which is very well-written for understanding the role of LSTM networks. Let’s discuss RNN network in this blog.

In a RNN, the information cycles through a loop. When it makes a decision, it takes into consideration the current input and also what it has learned from the inputs it has received previously.

![lstm_2](https://drive.google.com/uc?export=view&id=1qi8aSvKtRQHttyGNBZpszZUc3DtanlYg){:width="720px"}
***Figure-1: The unrolled RNN network can be understood as computational graph. With the help of previous hidden-state, next state can be computed.***
{: style="text-align: center;"}

Suppose a sentence given as input to train the model. The RNN network can handle data from the beginning of the sentence which will allow more accurate predictions of a new word at the end of a sentence. In reality, this isn’t necessarily true for vanilla RNNs due to vanishing gradient problem.

![lstm_3](https://drive.google.com/uc?export=view&id=1C6vrR8A8Dx-bKM0fHA4XDod4ardPty2F){:width="720px"}
***Figure-2: (left) The function computed by RNN at each time step. (right) the function computed by the whole RNN network.***
{: style="text-align: center;"}

This is a major reason why RNNs faded out in terms of performance for a while until some great results were achieved using a LSTM network. Adding the LSTM to the network is like adding a memory unit that can remember each words from the very beginning of the input.

![lstm_4](https://drive.google.com/uc?export=view&id=1dY6Ns00pJPBLLfVkvY9kmwcQRVRH4KAT){:width="720px"}
***Figure-3: The repeating module of LSTM network contains four interacting layers.***
{: style="text-align: center;"}

I’ll be covering working of LSTM network in my next blog post. It will help to understand how network has been implemented in TMVA module. Let’s discuss RNN layer implementation.

The RNN layer structure has been defined as follows in TMVA:

<div style="text-align:center"><img src="https://drive.google.com/uc?export=view&id=1K8NZWHV4FHGlu6ZwssYII-BN1gC9_9v8"/></div>

***Figure-4: Implementation of TBasicRNNLayer class (1)***
{: style="text-align: center;"}

These are variables which will be used through the RNN layer to perform certain tasks. According to the RNN equation:

<div style="text-align:center"><img src="https://drive.google.com/uc?export=view&id=1Vza7hNVkARKeOe59aO2xtcyx4N9TK5Lz"/></div>

***Figure-5: Recurrent Equation.***
{: style="text-align: center;"}

We’ve already initialised variables with respect to recurrent equation. The TBasicRNNLayer class also contains function prototypes that will be called to perform certain tasks such as: initialising hidden state, forward propagation, backward propagation, write weight matrices to XML file etc.

<div style="text-align:center"><img src="https://drive.google.com/uc?export=view&id=1nhA3uyF9i5YakznSDPtfhQ9og4M9XbIA"/></div>

***Figure-6: Implementation of TBasicRNNLayer class (2)***
{: style="text-align: center;"}

The ***Forward()*** function is being used to compute and return the next hidden state in RNN network followed by ***CellForward()*** function which is a forward for a single cell (time unit) and ***Backward()*** function to back propagate the error at each timestep followed by ***CellBackward()*** function which is a backward for a single cell. The RNN layer is fully functional.

The LSTM network acts as memory cell to recurrent networks. The cell is responsible for “remembering” the values. For training a LSTM network, the backpropagation algorithm comes useful which is a little bit tricky to implement and challenging since in LSTM we’re dealing with multiple inputs.

My current work includes the implementation of forward propagation in LSTM which is almost done along with CPU supported tests. In my next story, I’ll be discussing about how LSTM network works and how I’ve designed the class TBasicLSTMLayer.

### Reference

- [Understanding LSTM Networks - colah's blog](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)
