---
layout: post
title: "GSoC 2018: Backpropagation Feature in LSTM Network - Part IV"
date: 2018-08-10
category: archive
comments: true
author: "Harshit Prasad"
tags: [gsoc, backward-propagation, neural-networks, software-engineer, cern]
---


![backward_propagation](https://drive.google.com/uc?export=view&id=1YckoBf2YCB4B2_Z4KhX24IBVq8hCGP8H){:width="720px"}
{: style="text-align: center;"}

> Working on the backpropagation process inside an LSTM cell.

In this blog post, I’ll be discussing how backpropagation through time works in LSTM cell and how the current implementation of it has been done in TMVA for LSTM layer design. We call it ‘backpropagation through time’ because we’re repeating this process at each timestep. If observed carefully, this is similar to the normal backpropagation process.

Here is the pictorial representation of the LSTM cell:

![lstm_gates](https://drive.google.com/uc?export=view&id=1Zh0MJZlaUJyz11P6KVjM4J8-73pw5RE3){:width="720px"}
***Figure-1: The pictorial representation of the LSTM cell with four interacting gates.***
{: style="text-align: center;"}

The LSTM Cell backpropagation is different from RNN because in each timestep during backpropagation, the values of the input gate, forget gate and output gate should also be updated.

Here are the mathematical equations for reference:

![gate_equations](https://drive.google.com/uc?export=view&id=1FRLEhGD9pn6JLBPpcz7lNnnL5udiujlA){:width="720px"}
***Figure-2: Updating each gate value during the backpropagation process in the LSTM cell.***
{: style="text-align: center;"}

![lstm_weights_equations](https://drive.google.com/uc?export=view&id=1bg7PPPoVbgfQOWQkSxCf-7BgFUmUTBTG){:width="720px"}
***Figure-3: Final parameters update during the backward pass in the LSTM cell. dW represents input weights, dU represents state weights and db as biases.***
{: style="text-align: center;"}

The current implementation follows the final update of internal parameters: state weights, input weights, and biases. The implementation of each gate value update is still in progress since it requires many parameters to be passed from the ***Backward()*** method. I will be sharing the updates and performance regarding it in the next blog post.

#### Extras

- Above ⨀ is the element-wise product or Hadamard product.
- Inner products will be represented as ⋅
- Outer products will be represented as ⨂

The backward pass process in the TBasicLSTMLayer class has been implemented using the ***Backward()*** method to initialize tensor variables to store gradient values related to the hidden state, input gate, forget gate, output gate, and candidate gradient value. These values are passed to ***CellBackward()*** method to perform the final update of such internal parameters in the network.

I've mentioned a good reference of the LSTM network overall which gives a good numerical example for LSTM to verify layer design.

In my next story, I’ll be sharing the results of the LSTM layer, my final work and future work related to the TMVA-DNN project.

### References

- [Backpropogating an LSTM: A Numerical Example](https://medium.com/@aidangomez/let-s-do-this-f9b699de31d9)
