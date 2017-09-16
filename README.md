[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)
# Udacity Self-Driving Car Engineer Nanodegree
# Project: Finding Lane Lines on the Road

## Abstract

In this project, a lane line detection algorithm is developed in Python to detect straight lane lines in videos shot by front facing camera in car. 

The algorithm uses Canny edge detection and Hough transformation to extract the coefficients of line segments in the image, and use first order curve fitting to approximate the lane lines. The algorithm is implemented and tested on different videos shot during the highway driving, and successfully demonstrated the detection of straight lane lines under different lane line color and lighting conditions.

## Description of the algorithm
The pipeline of the algorithm consisted of 5 steps as following:
1. For a input image, it is first converted into grayscale

![alt text](https://github.com/davidsky900/SelfDrivingCar-LaneLine/blob/master/myExamples/imageGray.png)

2. the output image from step 1 is then filtered by a gaussian blur filter for smoothing and then a Canny edge detector

![alt text](https://github.com/davidsky900/SelfDrivingCar-LaneLine/blob/master/myExamples/imageEdge.png)

3. the output image from step 2 is then cropped by a helper function to pick the region of interest, which is defined as a trapezoid in this project

![alt text](https://github.com/davidsky900/SelfDrivingCar-LaneLine/blob/master/myExamples/imageCut.png)

4. the selected edges from step 3 is then converted into line segments with Hough transformation.

![alt text](https://github.com/davidsky900/SelfDrivingCar-LaneLine/blob/master/myExamples/imageHough.png)

5. the line segments is then merged into a single left laneline and a single right laneline. In order to perform the merging, the draw_line()  function is modified to first split the detected line segments into left or right groups depending on the locaiton of the line points and its slope. Then, three methods have been tried, and method c was eventually chosen due to the stabled detected lines.

 Method a. the estimation of lanelines are generated by the average position of line segments. 
 
 Method b. the length of each line segments are computed, and the estimation of the lanelanes are determined by the longest segment. This method is adopted based on the observation that the shorter lines returned by the Hough transformation is not always accurate and will deviate the laneline estimation. This effect is obvious when the lanelines fragmented. 
 
 Method c. the estimation of slope and bias of the laneline is finished by least square fitting the concatenated points of the line segments
 
 ![alt text](https://github.com/davidsky900/SelfDrivingCar-LaneLine/blob/master/myExamples/imageFinal.png)


## Potential shortcomings
There are two shortcomings for the current algorithm.

a. In current laneline estimation method, the detected lanelines would vary a bit in the video as the longest line segment is changing slightly frame by frame. 

b. In general the above algorithm works quite well when the lanelines are straght. However, one potential shortcoming would be that when the lanelines are curved, then the Hough transformation and line fit method would fail on detecting curves. This is due to the fact the implemented Hough transformation assumes a straightline model and the laneline estimation method also used a straight line model. 

## Suggested improvements
The following are the suggested improvements for current algorithm. 

For issue a, a possible improvement would be to use least square fitting or weighted least square fitting to merge the line segments. 

For issue b, one possible solution is to use higher order polynomial fit step 5 instead of 1st order line fit. Or one can also use generalized Hough transformation in step 4 to detect curves. 