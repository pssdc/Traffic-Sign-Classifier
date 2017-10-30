
# Traffic-Sign-Classifier

**Build a Traffic Sign Recognition Project**

Goals of the project
* Load the traffic signal data set and provide summary including exploratory visualization. 
* Preprocess the data if needed and include the reasoning behind it. 
* Provide details of model architecture.
* How the model was trained. Provide details of the parameters and hyper parameters. 
* Download new German traffic sign images from internet and test the model. 
* Provide top 5 softmax probabilities of predictions. 

## Rubric Points

Basic summary of the the dataset

* I used the pickle object serialization to load the dataset.
The original dataset included
* Training Set:   34799 samples
* Validation Set: 4410 samples
* Test Set:       12630 samples

After preprocessing and addition dataset generation following was the final sample size
* Training Set:   39264 samples
* Validation Set:  9816  samples
* Test Set:       12630 samples


### Visualization 
  1. First, I plotted a bar chart to check the frequency of each class in the dataset. Based on the Class vs frequency of labels we observe that the number of samples for the classes are not uniform. The difference between the minimum (180) and maximum (2010) frequency of classes was quit large. This may lead to training being skewed towards the classes with large frequency. To even out the frequency of samples, additional data is generated.

 2. All 43 images are also plotted (in Jupyter notebook) for visualization. 

### Data preprocessing 

  1. Extra images are generated for the classes that have less than 900 samples. The number 900 is chosen after several iterations. It provides the right balance between validation accuracy and training speed. 
  2. The extra images are generated by randomly rotating them between -20 to +20 degrees. Opencv library function is used to do this operation. 
  3. Many different formulas were used for data normalization but I settled with  data /128 - 1 as it gave me the best validation  accuracy. 
  4. To improve the training speed, images were also converted to grayscale using Opencv library. Opencv grayscale function required additional processing to add the 'depth' of 1. The 'newaxis' function from Numpy was used to add the 'depth'.  
  5. Data was also shuffled using sklearn utils for random distribution of the inputs before training. 
  6. Initially I used the validation set from the original data set but after several days of iterations I was not able to get my accuracy above 96%.  I recreated my validation dataset again from the preprocessed dataset using sklearn utility. This increased my accuracy from 96% to 99.4%. 


### Model Architecture
 I started with the Lenet lab solution architecture.
  
| Layer         	     	|     Description	                       	|                      |
|:---------------------:|:---------------------------------------------:|:----------------|
|Layer 1                |                               |                 |
|       |Input         		| 32x32x1 grayscale image  |
|	 |Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x6 |
| 	|RELU			|				 |					
| 	|Max pooling	      	| 2x2 stride,  outputs 14x14x6 	|
| Layer 2 |	| 	|
| 	  |Convolution 5x5	    | 1x1 stride, valid padding, outputs 10x10x16    |
|         |RELU			|	|		
|   | Max pooling	      | 2x2 stride,  outputs 5x5x16 				|
| | Flatten output = 5x5x16 = 400||					|						
| Layer 3 |||
| |Fully connected | input 400, output 120     |
| |RELU ||
| Layer 4||	|
|  | Fully connected		| input 120, output 84  |
| |RELU					|	|
| |Fully connected		| input 84, output 43  |
| |RELU|||  
     
     
* With the above architecture, I iterated the EPOCHS, learning rate. My output validation accuracy ranging from 85 - 90%.
       Testing accuracy came to 89%.

* I updated the model as follows

| Layer         		|     Description	        	|                 |
|:----------:|:------------------:|:--------------|
| Layer 1     |||
| |        Input   |	32x32x1 grayscale image |
||	Convolution |    5x5	1x1 stride, valid padding, outputs 28x28x32 |
||	RELU |	
||	Max pooling |	2x2 stride, outputs 14x14x32 |
||||
|        Layer 2 |||
|	|Convolution	|5x5	1x1 stride, valid padding, outputs 10x10x54|
|	|RELU |||	
|	|Max pooling	|2x2 stride, outputs 5x5x64|
| |Flatten the output (5x5x16). Output = 1600 ||
|       Layer 3 |||
||	Fully connected |input 1600, output 800|
||	RELU ||	    
|  Layer 4 |||
||	Fully connected |input 800, output 400|
||	RELU 	||
|    Layer 5 |||
||Fully connected |input 400, output 120|
| |	RELU 	||
||    Dropout with 0.9 probability||
|        Layer 6 |||
||	Fully connected | input 120, output 84|
||	RELU ||	
||     Dropout  with 0.9 probability ||  
|       Layer 7 |||
||	Fully connected | input 84, output 43|
||RELU ||

  This architecture improved my validation accuracy from 90% to 96%.
  Testing accuracy came to 92.8 % 

- How the model was trained. Provide details of the parameters and hyper parameters. 
  I spent lots of time with the original Lenet modal. I played with learning rates, EPOCHS, mu and sigma but I could not get the validation accuracy above 90%. 
  Later I change my model as described above. With the updated model I used following settings
  EPOCHS = 20
  Learning Rate = 0.001
  Batch size = 128
  mu = 0
  sigma = 0.09
  dropout keep probability = 0.9
  Adam optimizer

  I spent several days to improve the validation accuracy without any success. After looking at some other solutions on github, I regenerated my validation dataset from the preprocessed dataset (instead of using the validation set provided in the original data set). This suddenly improved my validation accuracy from 96% to 99.4%. Most likely the accuracy is improved due to the consistent preprocessing and scaling among the training and the validation dataset. 



### Testing on new images

 I downloaded 5 images from the internet, visualized them and applied same grayscale and normalization transforms. 
 The data set accuracy with these images came out to be 100%. I think the images are relatively simple hence the accuracy is 100%. 

### Softmax probabilities

The model predicts 4 out of 5 images correctly. Model has difficulty in predicting "Road work" sign. I think it is relatively the most complex sign off all. Currently I have created my incremental dataset just by only rotating the images. I think, the model predictions can be further improved by adding various other transforms (color exposure, wrapping, scaling, cropping etc.) in the additional dataset.

 

  




  


  




  


  
