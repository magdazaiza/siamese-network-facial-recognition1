# siamese-network-facial-recognition
Assignment 2 Report

Analysis of dataset (2b)

![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/4f930451-5361-4c01-8ac2-ed3c9eccaca7)



Including validation as part of the train set, there are 2200 samples in the training set.
The validation set is 0.25 of the full training set.

Experimental Setup + Architecture description (2c)
Batch Size: 1024
Architecture parameters are as described in the paper:
Layer 1: 64 filters of size 10x10 (64 @ 10x10) -> 2x2 Max Pooling -> Convolutional Layer (128 @ 7x7) - > 2x2 Max Pooling -> CONVOLUTIONAL Layer (128 @ 4x4) -> 2x2 Max Pooling -> Convolutional Layer (258 @ 4x4) -> (Flatten) -> Dense Layer with 2048 Nodes.

Activation functions:
	- Relu for all convolutional layers
	- Sigmoid for last dense layer

Weight initialization sampled from:
	- A normal distribution N(0, 0.01) for convolutional layers
	- N(0.5, 0.01) for all the biases in the network
	- N(0, 0.2) for the Dense Layer in the network

Regularization:
	- L2 on each layer.
Early stopping:
	- Early stopping occurred when the validation loss has not improved for 10 epochs.



Experiments (3 + 4)
(The model we chose (3a) is at the end)
After the first time we ran the network as it was described in the paper, we noticed that there was overfitting to the training data. There was much better accuracy on the training data than on the validation data. These were the results of the first run: 
 
 
We can see that in the first run the model did not learn. The reason for this is unclear to us, but we will attribute it to bad luck, as in other experiments the model turned out fine. (Problems with the google colab’s gpu usage prevented us from returning to the “vanilla” implementation to retry it)

We noticed that the dataset is very small, so there is a real danger of overfitting with the limited regularization in the original model. We tried to use dropout on the convolutional layers to solve this problem. We used dropout values of 0.3 on the convolutional layers. Our results were:
Dropout 0.3:
Model Convergence time: 1251 sec  ~= 21 min
  
 
Where the final Test set loss was 0.329, and the Test binary accuracy was 0.922. 
The final Validation set loss was 0.902, and the binary accuracy was 0.695.



And finally the model we chose: Using Batch norm (3a)
We also used batch normalization. We know this can improve the performance of Dense neural nets, and we tried to see how it would turn out with Convolutional Networks. Our results were:
Model Convergence time: 2010sec ~= 33.5 min
  
Test set results (formatted [loss, binary accuracy]):  
Validation set results (formatted [loss, binary accuracy]):  




Manual Evaluation (4d):
The following are pictures of the same person. The model correctly classified him:  

It makes sense that the model got this correct. This man is wearing a suit in both pictures and has the same face and features. (He is the same person, after all).


  

The model made a mistake and classified both of these people as the same person.
We can see that these are two different people, but they are both wearing a suit and both smiling. They also have similar hairstyles. The background is different, but the model should have learned that the background (and maybe the exact placement of some hairs) is something that changes between images.

From this (admittedly limited) exploration of the model we suggest that the model focuses too much on clothing, and possibly on larger features. We might have resized the images to too small a shape, or the network could have been built for less fine features to begin with, as it was built on letter alphabets.









Weird regular: 
 


