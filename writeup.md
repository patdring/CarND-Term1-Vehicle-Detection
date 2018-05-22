## Writeup Template
### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/hog_features.png
[image2]: ./output_images/featues_normalization.png
[image3]: ./output_images/heatmap.png
[video1]: ./output_videos/project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

In the second cell of the jupyter notebook, I randomly read images from the vehicle or non-vehicle data for quick experiments. I also determined how much data is contained in the respective data. These should be about the same size. The tests included color space, parameters, channels and HOG features. 

![alt text][image1]

Raw and normalized features were visualized and thus confirmed.

![alt text][image2]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters in the second cell and this resulted in the following (HOG-)parameters:

* orient = 11
* pix_per_cell = 8
* cell_per_block = 2

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

First of all, the color space YCrCb is used. This decision was made on the basis of experiments, research and discussions on the Internet. We refer to the following links:

* https://en.wikipedia.org/wiki/Talk%3AYCbCr

* https://gamedev.stackexchange.com/questions/125623/what-is-the-benefit-of-ycbcr?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa

In summary, however, the property can be called:
* Luma/chroma components are separated

11 orientations 8 pixels per cell and 2 cells per block has been selected (decision was made on the basis of experiments)

All HOG channels are used, also spatial, histogram an HOG features. These extracted features are then combined into a vector that will be normalized later.

20% of the data were selected as training data

The code for this is in the third cell.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

For this I used the tool Gimp (https://www.gimp.org/). On the one hand, to familiarize myself with this program for the future and because it showed good for this analysis (scales, areas, ...).

A sliding window search is implemented forth cell and used in fifth cell.

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales (1.2, 1.5, 1.7) using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result. 

The following classifier has been selected:

* A SVM with a search over specified parameter values for an estimator (GridSearchCV, for details see http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html)

* The best determined value for C is: 10

Here are some example images:

![alt text][image3]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

Here's a [link to my video result](./output_videos/project_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

In the 4th cell, functions are defined for:

* A function that can extract features using hog sub-sampling and make predictions
* A function to draw bounding boxes
* A function that add heat to each founded box and apply a threshold to help remove false positives

With the functions already defined and the classifier determined, all possible bounding boxes for vehicles are now searched for. False postives or overlays are filtered out with a so-called heatmap.

In addition, the entire area of the image is not used for a search.

Scaling factors are defined for near and distant vehicles (1.2, 1.5, 1.7).

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

General:
* The GridSearchCV approach with automated parameter search C saves development time.

Failures:
* In the processed video are still some false positives, this means that the sliding windows can be improved.

Possible improvements:
* More features based on histogram and color spaces in the feature vector could improve the accuracy and make it more robust.
* More training images could improve the detection and/or differentiation between vehicles and non-vehicles

A comparison with a Deep Leerning approach (CNNs like in P3) would be exciting.
