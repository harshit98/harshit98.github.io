---
layout: post
title: "GSoC 2018: Forward Propagation Feature in LSTM Network - Part III"
date: 2018-06-13
category: archive
comments: true
author: "Harshit Prasad"
tags: [gsoc, forward-propagation, neural-networks, software-engineer, cern]
---


![forward_propagation](https://drive.google.com/uc?export=view&id=1N6JFs14_ZROahToxjjTK1fD9hJoZX67U){:width="720px"}
{: style="text-align: center;"}

> Explanation of LSTM layer design, forward propagation in LSTM network and what challenges were faced.

In my last blog, I discussed about how RNN and LSTM network works. In this blog, I will be briefly explaining about how forward propagation in TMVA-DNN has been implemented. The back propagation part is currently in progress which I’ll discuss in my next story.

Let’s start with structure of LSTM cell. The figure below depicts a LSTM unit with four interacting layers.

![lstm_layers](https://drive.google.com/uc?export=view&id=1DJ_LLNeWpkj-RMAHGcZ3pZxwjt8hUDHL){:width="720px"}
***Figure-1: LSTM unit having four interacting layers. Each layer has it’s own importance.***
{: style="text-align: center;"}

According to the research paper ***“An Empirical Exploration of Recurrent Network Architectures”***: These four layers can be divided into four types of gates. LSTM cell has 2 states i.e ***"cell state"*** and ***"output state"***. The cell state helps to fight the vanishing gradient problem and on the other hand, output state allows LSTM network to make complex decisions over short period of time. This was one of the challenge to determine how many states are present in LSTM.

The second challenge was to determine if weight matrices and bias vectors are same or different. The article which I’ve mentioned above was quite helpful. LSTM network usually have 8 different type of matrices and 3 different bias vectors. Hence, we’re having multiple inputs which was quite tricky to implement in ***TBasicLSTMLayer*** class. Let’s discuss about the four gates which I’ve mentioned above.

First step is to store new information in cell state. This process can be divided into two parts. A ***sigmoid layer*** called "input gate layer" decides to add new information further followed by a ***tanh activation layer*** which creates a vector of new candidate values. The forget gate layer decides what information has to be thrown away from cell state. This decision-making process is followed by sigmoid activation function which outputs a number between **0** and **1**. The values nearer to 0 means to ‘forgets’ and the values nearer to 1 means ‘remember’. Next step comes to update the cell state. The candidate values are further used to scale and update new cell state by computing element-wise matrix multiplication (a.k.a Hadamard product) with input gate values. The result obtained undergoes scalar addition with element-wise matrix multiplication of forget gate values and previous cell state. We multiply forget gate values to previous cell state because it helps in forgetting the things, which we decided to forget earlier.

Then finally comes the output gate which helps to decide output value of the network and next output state. The output gate is followed by sigmoid activation function and we put new cell state values through tanh activation function to push back values between **-1** and **+1**.

The result undergoes Hadamard product with output gate values and computes the next output state.

![lstm_equations](https://drive.google.com/uc?export=view&id=1sZxcnAPMFrStns9CJ3FFtyLz06D7NfrJ){:width="720px"}
***Figure-2: LSTM equations which helps to keep the information. The equations are referred from the research paper which I’ve mentioned above.***
{: style="text-align: center;"}

Let’s discuss how I’ve implemented the forward propagation.

1. Defined 2 states: **cell state** and **output state**.

2. Defined 8 weight matrices and 4 bias vectors.

3. To maintain the code, created same functions ***Forward()*** and ***CellForward()*** similar to RNN layer. The working of both the functions is similar to RNN layer. Only difference is that, we’re dealing with multiple inputs and gates here.

4. Defined function for each gate. Each gate function is computing it’s own value and being called inside ***Forward()*** function.

5. The next cell state and next output state is being computed inside ***CellForward()*** function.

6. The benchmarking (i.e performance) part is left which I’ll discuss separately in other blogpost.

The current LSTM code supports CPU architecture. Third challenge which I faced was during compilation of code. Most of the errors were related to dimension mismatch of weight matrices and inputs which took me 1 week to find it out in my code. The RNN and LSTM architecture design has been inspired from ***Torch framework***.

Fourth challenging task was testing the code. The task difficulty was reduced after going through ***RNN*** tests in TMVA and ***Torch-LSTM*** tests. Tests are successfully passing now. The LSTM forward pass tests only supports CPU architecture at present.

I’ll be writing code and tests for GPU architecture as well related to LSTM layer and RNN layer if I’ll get time after completing high priority features. Currently, I’m working on backpropagation through time algorithm in LSTM. The next blog will be up soon. :)
