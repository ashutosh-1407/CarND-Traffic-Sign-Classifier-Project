# **Traffic Sign Recognition** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/visualization.jpg "Visualization"
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./examples/random_noise.jpg "Random Noise"
[image4]: ./examples/placeholder.png "Traffic Sign 1"
[image5]: ./examples/placeholder.png "Traffic Sign 2"
[image6]: ./examples/placeholder.png "Traffic Sign 3"
[image7]: ./examples/placeholder.png "Traffic Sign 4"
[image8]: ./examples/placeholder.png "Traffic Sign 5"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the numpy library to calculate summary statistics of the traffic signs dataset. Below are few of the details:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

I visualized the data by plotting how many images are there in each of the class in training, validation and test dataset. It seems that the distribution is more or less similar in all the three datasets. 

[plots/train_plot.png]
[plots/valid_plot.png]
[plots/test_plot.png]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

For the pre-processing steps, I just normalized the images using the equation{img = img * (0.99 * 255) + 0.01}. This gave me pretty good results.

#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| BatchNorm				|											    |
| Max pooling	      	| 2x2 stride,  outputs 14x14x6 				    |
| Dropout               | keep_prob = 0.75                              |
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 10x10x16 	|
| BatchNorm				|											    |
| Max pooling	      	| 2x2 stride,  outputs 5x5x16 				    |
| Dropout               | keep_prob = 0.75                              |
| Flatten               |                                               | 
| Fully connected		| 400 -> 120   									|
| BatchNorm				|											    |  
| Dropout               | keep_prob = 0.75                              |
| Fully connected		| 120 -> 84    									|
| BatchNorm				|											    |
| Dropout               | keep_prob = 0.75                              | 
| Fully connected		| 84 -> 43    									|
 
Finally softmax is applied and loss is minimized using adam optimizer.

#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

For training the model, I used Adam Optimizer with learning rate as 0.001, batch_size as 128, 20 epochs, mu of 0 and sigma of 0.1 for initializing the weights of layers. This provided me with good accuracy on validation set.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 99.6%.
* validation set accuracy of 95.6%
* test set accuracy of 88.55%.

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?
-> LeNet is a good starting point for many problems, so I decided to give it a try to do german traffic signs classification.

* What were some problems with the initial architecture?
-> The architecture was too simple for this classification task, so it was not able to preform as good as we need the results to be.

* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
-> First change was to include 3 channels since german traffic signs are colored images. Also high accuracy on train set but not as good on validation set, so I decided to add BatchNorm and Dropout to introduce regularization and reduce overfitting. I used BatchNorm and Dropout after both the covolutional and fully connected layers. It proved pretty good.

* Which parameters were tuned? How were they adjusted and why?
-> I tried to adjust the batch_size, keep_prob(in dropout). Batch_size since it should not be too small or too big for the network to get trained better. Also, to add regularization, dropout is one of the best choice when the dataset consists of images.

* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?
-> Since the problem was to classify images, thus convolutional neural network is a go to model for this case. Starting with the basic LeNet model, dropout helped in regularization. And BatchNorm helped since if the points get spread out after passing through a convolutional or fully connected layer, they were again normalized stablizing the training.

If a well known architecture was chosen:
* What architecture was chosen?
* Why did you believe it would be relevant to the traffic sign application?
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well?
 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

[downloaded_images/pic1.png]
[downloaded_images/pic2.png]
[downloaded_images/pic3.png]
[downloaded_images/pic4.png]
[downloaded_images/pic5.png]

It seems that the model got confused with the 4th image, may be because of the number of the vehicles. Otherwise, the model performed really well on the downloaded german traffic signs.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction. The achieved accuracy is 80% which is less than what was achieved in test set. This can be because of very few number of images in this case(only 5).

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Bumpy road      		| Bumpy road   									| 
| Priority road			| Priority road									|
| Stop					| Stop											|
| Vehicles over 3.5 metric tons prohibited| End of no passing			|
| Bicycles crossing		| Bicycles crossing      						|


The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. This compares favorably to the accuracy on the test set of ...

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The top five soft max probabilities for the images are as below:

1st image, the probabilities are far enough to predict the correct class here.

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .23         			| Bumpy road   									| 
| .13     				| Bicycles crossing								|
| .12					| No Vehicles									|
| .06	      			| Road Work					    				|
| .004				    | Road narrows on the right						|

2nd image, the model is pretty sure in this image.

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .38         			| Priority Road   								| 
| .09     				| Right-of-way at the next intersection 		|
| .04					| Traffic signals								|
| .03	      			| End of all speed and passing limits			|
| .03				    | No Entry      						    	|

3rd image, the probabilities are far enough to predict the correct class here.

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .13         			| Stop      									| 
| .07     				| No Entry 										|
| .06					| Speed limit (20km/h)							|
| .05	      			| Bicycles crossing					 			|
| .02				    | Yield             							|

4th image, the probabilities are extremely close and the model got confused here.

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .064         			| End of no passing								| 
| .060    				| Vehicles over 3.5 metric tons prohibited 		|
| .045					| Roundabout mandatory							|
| .034	      			| No Passing					 				|
| .032				    | Slippery road      							|

5th image, here the probabilities are pretty close but the model classified well.

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .10         			| Bicycles crossing   							| 
| .08     				| Children crossing 							|
| .06					| Speed limit (20km/h)							|
| .04	      			| Bumpy Road					 				|
| .02				    | General caution      							|

### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


