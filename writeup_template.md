# **Behavioral Cloning** 

---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/placeholder.png "Model Visualization"
[image2]: ./examples/center_2016_12_01_13_30_48_287.jpg "Grayscaling"
[image3]: ./examples/left_2016_12_01_13_34_38_867.jpg "Recovery Image"
[image4]: ./examples/right_2016_12_01_13_46_38_947.jpg "Recovery Image"
[image5]: ./examples/placeholder_small.png "Recovery Image"
[image6]: ./examples/placeholder_small.png "Normal Image"
[image7]: ./examples/placeholder_small.png "Flipped Image"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md or writeup_report.pdf summarizing the results

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py models/model_aug_left_right_cropping.h5
```

#### 3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

My model consists of a convolution neural network with 5x5 filter sizes and depths between 6 and 6 (model.py lines 48-60) 

The model includes RELU layers to introduce nonlinearity (code line 51, 53), and the data is normalized in the model using a Keras lambda layer (code line 50). 

#### 2. Attempts to reduce overfitting in the model

The model was trained and validated on different data sets to ensure that the model was not overfitting (code line 60). The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually (model.py line 59).

#### 4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. I used a combination of center lane driving, recovering from the left and right sides of the road ... 

For details about how I created the training data, see the next section. 

### Model Architecture and Training Strategy

#### 1. Solution Design Approach

The overall strategy for deriving a model architecture was to reduce the mean square error.

My first step was to use a convolution neural network model similar to the LeNet.

I thought this model might be appropriate because it's similar to image classification.

In order to gauge how well the model was working, I split my image and steering angle data into a training and validation set. 

The final step was to run the simulator to see how well the car was driving around track one. 
There were a few spots where the vehicle fell off the track.
To improve the driving behavior in these cases, I augment the training data.

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

#### 2. Final Model Architecture

The final model architecture (model.py lines 18-24) consisted of a convolution neural network with the following layers and layer sizes.
    
    model.add(Conv2D(6, 5, 5, activation='relu'))
    model.add(MaxPool2D())
    model.add(Conv2D(6, 5, 5, activation='relu'))
    model.add(MaxPool2D())
    model.add(Flatten())
    model.add(Dense(120))
    model.add(Dense(85))
    model.add(Dense(1))

#### 3. Creation of the Training Set & Training Process

To capture good driving behavior, I first recorded two laps on track one using center lane driving. Here is an example image of center lane driving:

![alt text][image2]

I then recorded the vehicle recovering from the left side and right sides of the road back to center so that the vehicle would learn to come back to the centre
if the vehicle is deviated to the sides in order to recover to the centre.

![alt text][image3]
![alt text][image4]


To augment the data sat, I also flipped images and angles thinking that this would augment the traning data.


After the collection process, I had 38572 number of data points. 
I then preprocessed this data by cropping and normalisation:

    model.add(Cropping2D(cropping=((70, 25), (0, 0)), input_shape=(160, 320, 3)))
    model.add(Lambda(lambda x: x/255.0 - 0.5, ))


I finally randomly shuffled the data set and put 20% of the data into a validation set. 

I used this training data for training the model. 
The validation set helped determine if the model was over or under fitting. 
The ideal number of epochs was 5 as evidenced by the validation error stop decreasing.
 I used an adam optimizer so that manually training the learning rate wasn't necessary.
