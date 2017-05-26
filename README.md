[//]: # (Image References)
[image0]: ./examples/Intro.png "Introduction"
[image1]: ./examples/lenet.png "LeNet"
[image2]: ./examples/bar_chart_train_dataset.png "Train Distribution Set"
[rslt_stop]: ./examples/result_stop.png "Result Probability Stop Sign"
[rslt_priority]: ./examples/result_priority_road.png "Result Probability Priority Road"
[rslt_50kph]: ./examples/results_50kph.png "Result Probability Speed Limit 50kph"
[rslt_roadwork]: ./examples/results_road_Work.png "Result Probability Road Work"
[rslt_General_Caut]: ./examples/result_generalcaution.png "Result Probability General Caution"
[Grayscale]: ./examples/grayscale.png "Grayscale"
[HSV]: ./examples/HSV.png "HSV"
[YUV]: ./examples/YUV.png "YUV"
[Rotate]: ./examples/Rotate.png "Rotate"
[Enhance]: ./examples/Enhance.png "Enhance"
[Translate]: ./examples/Translate.png "Translate"
[test_image]: ./examples/augumented_test_image.png "Augumented Image"
[augument]: ./examples/augument.png "Augument"


# Traffic Sign Recognition

![Intro][image0]

This project builds a Traffic sign recognition classifier to identify German traffic sign. In this project a [LeNet-5](http://yann.lecun.com/exdb/lenet/)  architecture propsed by Yann LeCun. This architecture is a conventional neural netwrok that was designed to recogonise the hand written visual patterns from the image with minimal preprocessing.

![LeNet][image1]


The following steps are used to create the classfier, pipeline and training process.

- Load the data.
- Understanding and Visualising the data.
- Define training set, validation set and test set.
- Design of Pipeline.
- Training of Network.
- Run Benchmark model without any preprocessing.
- Preprocessing the data for usage.
    - Use of different normalisation methods.
    - Apply different image augmentation methods.
- Measure of system performance.
- Tunning of system performance.
- Run classifier on Test data.


### 1. Data Set Summary & Exploration

The given data set is database of German traffic signs collected for a project run by the institute for Informatics [German Traffic Sign Dataset](http://benchmark.ini.rub.de/?section=gtsrb&subsection=dataset).

The data provide in the course are pickled python object which contains a dictionary of images and its labels. The data is loaded using the pickle module and allocated as original data set. The data is first analysed to understand the image size, type and dataset information.


* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is 32x32x3
* The number of unique classes/labels in the data set is 43

#### 2. Exploratory visualization of the dataset.

Understanding the dataset is key step to build a robust model. The training set consist of several sets of traffic and understanding how they are distributed was the initial step. The following is a bar chart showing the training data set. It is very evident that some signs have a larger dataset when compared to others. 

![Training dataset Distribution of traffic signs][image2]

### Design and Test a Model Architecture

#### 1. Pre processing techniques

I decided the preprocessing techniques in the three steps.

1. Image Augumentation
2. Colour Space
3. Normalisation

##### Image Augumentation

In real life scenario a camera mounted on different vehicles such as passgener car or truck has differnt perpective of the traffic sign images. In order address such a scenario, three image augumentation methods were tested

1. Image Translation - Needed as the traffic sign could be partially blocked in the field of view of the camera. The effect of translation is illustrated by an example image before and after the pre processing.

![Translate Colour Space][Translate]

2. Image Rotation - Mild rotation is needed as there situation the traffic sign is seen from uneven road gradient and traffic sign damaged by weather conditions. The effect of Rotation is illustrated by an example image before and after the pre processing.

![Rotate Colour Space][Rotate]

3. Image Enhancement - The camera records the images in combination with different lighting conditions, exposure etc., The image need not be always sharp. A gaussian blur filter is a very effective method to reduce noise and smoothen the given image. Th following is an example of the enhancement on a traffic sign.

![Enhance Colour Space][Enhance]

##### Colour Space

1. Grayscale - In the original paper decribing the LeNet Architecture, Yann LeCun described that grayscale as effective means to improve the model performance in training for a feature but also faster training as the matrix is simplied. The training set was converted to grayscale.

![Gray Scale][Grayscale]

2. HSV and YUV - Other colour space attempted to improve the features to standout for the model to be more roust in training.

![HSV Colour Space][HSV]


![YUV Colour Space][YUV]

Surpsiringly the different colour space did not yield promising results as expected. This may be possibly that the small features are better represented in RGB space than gray scale for the model.

##### Normalisation

The final step in the pre processing methods is to identify normalisation methods. Two differnt normalisation methods was experimented.

1. Feature Score - This normalisation is one of the most common and work better when the data is normally distributed.

2. Min Max Scaler or Feature Scaling - Feature scaling is used to bring all values in to a range of [0,1]. It is also called unity based normalisation.

The feature score method peformed better thatn min max scaler and hence it was used in the final preprocessing.

In order to build a robust model, the training data set should also be robust. Hence I augumented the entire training set and concatenated them to make double size data, which have images that are blurry, unsharp but also an equivalent preprocessed alias. The entire data set was final step was to normalise the entire dataset with feature score normalisation.

The following is an example of an original image and an augmented image:

![Augument][augument]

The difference between the original data set and the augmented data set is the following ... 


####2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer           	| Shape    	| Description                |
|:----------------:	|:---------:|:--------------------------:|
| Input           	| 32x32x3  	| RGB Image                  |
| Convolution     	| 28x28x6  	| 1x1 Stride, Valid Padding  |
| Activation      	| 28x28x6  	| ReLU                       |
| Max Pooling     	| 14x14x6  	| 2x2 Stride, Valid Padding  |
| Convolution     	| 10x10x16 	| 1x1 Stride, Valid Padding  |
| Activation      	| 10x10x16 	| ReLU                       |
| Max Pooling     	| 5x5x16   	| 2x2 Stride, Valid Padding  |
| Flatten         	| 400      	|                            |
| Fully Connected 	| 120      	|                            |
| Activation      	| 120      	| ReLU                       |
| Fully Connected 	| 84       	|                            |
| Activation      	| 84       	| ReLU                       |
| Fully Connected 	| 43       	|                            |
| Softmax         	| 43       	| ReLU                       |

####3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an ....

####4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* validation set accuracy of ? 
* test set accuracy of ?

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?
* What were some problems with the initial architecture?
* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
* Which parameters were tuned? How were they adjusted and why?
* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?

If a well known architecture was chosen:
* What architecture was chosen?
* Why did you believe it would be relevant to the traffic sign application?
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well?
 

### Test a Model on New Images

#### 1. Test Images and ideas behind choosing them

The five traffic sign that I choose to test on the model are a speed limit, general caution, priority road, road work and stop signs. The following is the test images after rescaling and augumentation.

![Augumented Road Signs][test_image]

The speed sign was chooses for two reason. This is the sign that model can be frequently subjected and with various number sign with similar characteristics, the model can predict it incorrectly. For example the image with speed sign 50 could appear as 80 with right level of noise and angle. The image also had a watermark, which could potential reduce the softmax probabilites

The seconds image was "General Caution". This was one of the intresting choices because there many similar looking signs in the dataset. For example Road narrows left, Road Narrows Right or Traffic Signal could appear as General caution sign to a model. The exclamation feature in the center of the triangle has similarity to many other warning signs.

The priority traffic sign was one the unique in the data set. A priority sign and end of priorty sign are the only two that has a Rhombus shape. double border and two colour. This sign was probably the most unique for the model to predict. This sign also had part of the roof of the building that would have containminated the prediction with other signs.

The road work was the favourite choice road sign becasue in reality there are usually multiple sign combined in a construciton zone. In the test image there is barricade that could be seen as level crossing sign. The road work image as share its similarity of a person in the center with pedestrians and childrens crossing sign.

The fifth image is a stop sign. This also has unique octogonal shape, with stop letters as characters. In general the LeNet 5 architecture performs very well for letter. Hence this is an easy choice for the model to predict.


#### 2. Model Predictions


The following are the results of the prediction from the model. 


| Image			        |     Prediction	       | 
|:---------------------:|:------------------------:| 
| Speed Limit 50 Kmph 	| Speed Limit 50 Kmph	   | 
| General Caution		| Traffic Signal     	   |
| Priority Road			| Priority Road            |
| Road Work	      		| Road Work 			   |
| Stop Sign 			| Stop Sign         	   |


The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. This compares favorably to the accuracy on the test set of 84.5%.


#### 3. Softmax Probabilites and Understanding Predictions


The code for making predictions on my final model is located in the last 4 cells 121, 22 and 41st result cell of the Ipython notebook.

The first image, the model is very sure that this is a speed limit of 50 Kmph (probability of 0.99), and the image does contain a 50 Kmph sign. The top five soft max probabilities are below. There was significantly low chance this image could be 80 kmph speed sign. The augumentaion of the image cleared up the watermark.

| Prediction            | Probability   |
|:---------------------:|:-------------:|
| Speed limit (50km/h)  | 0.99996674061 |
| Speed limit (80km/h)  | 0.00003324157 |
| Speed limit (100km/h) | 0.00000000000 |
| Speed limit (30km/h)  | 0.00000000000 |
| Speed limit (60km/h)  | 0.00000000000 |

![Softmax Probability of Speed Limit 50kph][rslt_50kph]

The second image was general caution but the model incorrectly identified as traffic signal with very high confidence. This could potential be due to the close resemblence of General caution and traffic signal signs. More learning situations improvement in image augumentation can help address the situation.

| Prediction           | Probability   |
|:--------------------:|:-------------:|
| Traffic signals      | 0.99986636639 |
| General caution      | 0.00013365022 |
| Pedestrians          | 0.00000000000 |
| Speed limit (20km/h) | 0.00000000000 |
| Speed limit (30km/h) | 0.00000000000 |


![Softmax Probability of General Caution][rslt_General_Caut]

The model was 100 % confident that the third image was Priority road and it correct predicted this traffic sign. This might be due to the very unique shape and charectistics of this traffic sign.

| Prediction           | Probability  |
|:--------------------:|:------------:|
| Priority road        | 1.0000000000 |
| Speed limit (20km/h) | 0.0000000000 |
| Speed limit (30km/h) | 0.0000000000 |
| Speed limit (50km/h) | 0.0000000000 |
| Speed limit (60km/h) | 0.0000000000 |

![Softmax Probability of Priority Road][rslt_priority]

The fourth image was correctly predicted as with probability of 1.0 as road work. This was an intresting choice as this sign had chances to be predicted wrong due to the back ground of the image and other similar signs.

| Prediction                            | Probability |
|:-------------------------------------:|:-----------:|
| Road work                             | 1.000000000 |
| Right-of-way at the next intersection | 0.000000000 |
| Turn right ahead                      | 0.000000000 |
| Children crossing                     | 0.000000000 |
| Road narrows on the right             | 0.000000000 |

![Softmax Probability of Roadwork][rslt_roadwork]

The fifth image was the stop sign and model predicted with very high certainity that it was a stop sign.

| Prediction                                   | Probability   |
|:--------------------------------------------:|:-------------:|
| Stop                                         | 0.99997258186 |
| No entry                                     | 0.00002637984 |
| Speed limit (120km/h)                        | 0.00000104451 |
| No passing                                   | 0.00000000176 |
| No passing for vehicles over 3.5 metric tons | 0.00000000002 |

![Softmax Probability of Stop][rslt_stop]




