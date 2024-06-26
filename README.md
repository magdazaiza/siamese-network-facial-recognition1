# siamese-network-facial-recognition


# Description

Welcome to our facial recognition project! This repository contains an implementation of a convolutional neural network (CNN) for facial recognition using a one-shot learning approach. Our project leverages state-of-the-art deep learning techniques, including the Siamese network architecture, to analyze facial images and determine whether they belong to the same person. With a focus on real-world applications, we aim to provide a robust solution for facial recognition tasks.


# Key Features
- Implementation of a Siamese network architecture based on the paper "Siamese Neural Networks for One-shot Image Recognition."

- Integration of logging tools for better insight into the training process.

- Experimentation with various parameters and techniques to optimize model performance.

- Analysis of the dataset, including size, distribution, and characteristics.

- Evaluation of the model's performance, including convergence times, loss, and accuracy metrics.

- Documentation of the experimental process, including architecture description, parameter choices, and results analysis.



# Report

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
![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/14c42473-fa0d-459d-88c7-2144f83b9dbd)

 
 ![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/35655a3f-1fc4-4c93-9095-633977d5a9bb)

We can see that in the first run the model did not learn. The reason for this is unclear to us, but we will attribute it to bad luck, as in other experiments the model turned out fine. (Problems with the google colab’s gpu usage prevented us from returning to the “vanilla” implementation to retry it)

We noticed that the dataset is very small, so there is a real danger of overfitting with the limited regularization in the original model. We tried to use dropout on the convolutional layers to solve this problem. We used dropout values of 0.3 on the convolutional layers. Our results were:
Dropout 0.3:
Model Convergence time: 1251 sec  ~= 21 min

![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/8ccaa5fc-d98c-4d6f-bc90-fb94254aa243)
![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/924e7fef-5e91-4ac4-93fb-5b9247e69e3d)
![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/0452050c-aa9a-416a-86c1-fc0fa1b1974a)

 
Where the final Test set loss was 0.329, and the Test binary accuracy was 0.922. 
The final Validation set loss was 0.902, and the binary accuracy was 0.695.



And finally the model we chose: Using Batch norm (3a)

We also used batch normalization. We know this can improve the performance of Dense neural nets, and we tried to see how it would turn out with Convolutional Networks. Our results were:

Model Convergence time: 2010sec ~= 33.5 min

Test set results (formatted [loss, binary accuracy]):  ![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/5e4e1e64-d52c-4d67-880b-13d62eb64801)

Validation set results (formatted [loss, binary accuracy]): ![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/b1c6933c-7e02-4939-a0dd-6911fce7b586)
 




Manual Evaluation (4d):

The following are pictures of the same person. The model correctly classified him:  
![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/9c6b7779-9b89-4008-9963-b225ec1f4bb5)
![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/9616961f-03ef-411d-99ea-2c9c56f4a560)

It makes sense that the model got this correct. This man is wearing a suit in both pictures and has the same face and features. (He is the same person, after all).

![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/13b5a486-d4e4-48a9-a136-ed2fdcde0d64)
![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/1f8a5402-775b-4680-aa62-d309ef8ab5b2)


  

The model made a mistake and classified both of these people as the same person.
We can see that these are two different people, but they are both wearing a suit and both smiling. They also have similar hairstyles. The background is different, but the model should have learned that the background (and maybe the exact placement of some hairs) is something that changes between images.

From this (admittedly limited) exploration of the model we suggest that the model focuses too much on clothing, and possibly on larger features. We might have resized the images to too small a shape, or the network could have been built for less fine features to begin with, as it was built on letter alphabets.









Weird regular: 
 ![image](https://github.com/magdazaiza/siamese-network-facial-recognition1/assets/96849106/55493332-632c-472e-a293-c12f2b4d437b)



