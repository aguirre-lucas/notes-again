# Introduction
- FINN is really fast and memory efficient, making it a prime candidate for FPGAs
# Background
## Convolutional Neural Networks (CNNs)
-  A multi layer perceptron is a type of Artificial Neural Network (ANN) which has its neurons arranged in multiple layers, with neurons taking the output of all neurons of the previous layers as inputs. Below is the equation that calculates the output for the nth neuron in the lth layer of a fcn, where $w_{l,s}$ is the weight of the sth synapse connected to the input of the nth neuron in the lth layer, $b_{l,n}$ is a bias term and $f_{act}$ is the activation function, and Sl is the number of synapses connected to each neuron in the lth layer. 
- $$a_{l,n}=f_{act} \left ( \sum_{s=0}^{S_l}w_{l,n,s}a_{l-1,s}+b_{l,n}\right ) $$
- CNNs use only a small percentage of inputs from the previous output, which reduces the number off edges to be found in the graph. The math isn't necessary, just know it's similar to a fcn except for some details found in the paper.
## Binary Neural Networks (BNNs)
- With floating point precision comes redundancy when changes aren't drastic between layers, wasting a lot of space and potentially time.
- BNNs take it to the extreme by making all the arithmetic involved become only 1 bit.
- 3 aspects of binarization for neural network layers: binary input activations, binary synapse weights and binary output activations. If all 3 components are binary, it's fully binarized, else it's partially binarized.
- Previous work report a 98.7% accuracy with fully-connected networks using full binarization using only XNOR and bitcount operations for computing the neural network.
# BNNs on Reconfigurable Logic
- Heterogeneous streaming architecture.
- Constrained by FPGA and platform, desired classification throughput in FPS, clock frequency. 
- Popcount Accumulation:
	- Let Y be the number of inputs for a given neuron, with the number of +1 valued inputs as Y1 and -1 valued inputs as Y0. 
	- Y=Y0+Y1. 
	- The result for hardware is that the sum of a binary dot product can be implemented by a popcount operation that counts the number of set bits instead of accumulation with a signed arithmetic. 
## FINN design flow
- ![[Pasted image 20260317182706.png]]
- 