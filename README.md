# Sign Detection

The objective of this repo is to detect signs in images and label it with it value. We are going to use Open CV selective search and a CNN to classify boxes.
To do that, we first work with the dataset to create a new class which was not included in the original dataset to detect if there is not a sign in our box. 

## About the dataset

The final dataset is available at this url : [dataset](https://we.tl/t-9Hn9f9SfZC)

At the beginning, the files contained 35000 train images and 12000 test images. There was also a csv file with the path of the data, the bounding box ( that we will not use ine this project ) and the class label of the image. 
The first step was to create a class with no sign in it. To do so, we take 6 images containing 1 sign from google image. Then whe label it with LabelImg ( available ![here](https://github.com/tzutalin/labelImg) ) to get the coordinates of the box where the sign is. When we ge those coordonates, we put it in a csv file to use it in the first script of the project.

## no_sign_script

In this python script we are going to process through the image that we previously labelled to extract other image which don't contain a sign. To begin, we pass it on the open CV selective search function, which extract all the boxes where it analyze that there might be an object. With all the boxes and their coordonates, we can compare them with the iou (see [here](https://www.pyimagesearch.com/2016/11/07/intersection-over-union-iou-for-object-detection/) ) function and if there is less than 5% of overlaping we extract and save the newly created image in a folder that we later use as the no sign class. This part is inspired by [this](https://www.pyimagesearch.com/2020/07/13/r-cnn-object-detection-with-keras-tensorflow-and-deep-learning/) tutorial about object detection. 


## Create the CNN 

This part is a classical path to create a network, and all is explained in the notebook in the data_sign_model_creation folder.

## Inference 

### Images

In this section, we will re use the cv2 selective search function. We will pass all the boxes returned in our model previously created. If the confidence pourcentage of our model is lesser than 95% or if our model predict that the class is "no_sign" we will not keep those. For the one that are saved, we will calulate the IOU and if there is no overlapping with an other box, save it and display it with it class.

### Video

We use open CV to read a video, that we analyze frame by frame and save a new video with the modified frame which display the boxes of the different signs that we detect.
