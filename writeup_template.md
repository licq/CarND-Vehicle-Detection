##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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
[image1]: ./output_images/car_not_car.png
[image2]: ./output_images/car_hog_features.png
[image8]: ./output_images/not_car_hog_features.png
[image4]: ./output_images/sliding_window.jpg
[image5]: ./output_images/bboxes_and_heat.png
[image6]: ./output_images/labels_map.png
[image7]: ./output_images/output_bboxes.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the sixth code cell of the IPython notebook (or in lines # through # of the file called `some_file.py`).  

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:


![alt text][image2]
![not car hog features][image8]

####2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters including include or not spatial bin features, color histogram features, hog features and different parameters of orientation, different color space and finally HSV color space with all features included and orientation 8 has the best performance.

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM in code cell 9. First, I extracted all features including spatial bins, color histograms and hog features, then I use StandardScaler to scale different features, after that, I split the dataset to train and test with test percent 0.2. After that I use the fit method of svc class method to train the model.

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I looked at the photos in the test_images folder and see the car position and size and finally I chose to find cars in the y axis 400 to 656 using scale 1.5 and 380 to 572 using scale 1.

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text][image4]
---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_output_video.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I implemented the sliding window technique on each frame of the video, and added windows with prediction = True to the heatmap. To speed up the process a little bit I used 2 different window scales only.

The generated heatmaps often had false positive predictions. To reduce these I summed the heatmaps over the last 20 frames and used thresholding to get less false positives. I set a minimum size for the bounding boxes. This reduced false positives too.


---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Two problems. First, the process is too slow for real world usage. I took more than 10 minutes to process video of 50 seconds.

Second, the find region and scale is hard to tune. 

