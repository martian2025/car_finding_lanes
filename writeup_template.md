# **Finding Lane Lines on the Road** 

## Writeup Template


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline is implemented in function *one_image_pipe* , which has the following steps :
1. Applied a *gaussian_blur* on the grayed image; this will make sure that the various short lines in all directions in the image will not generate unnecessary lines when applying the Hough transform
2. Extracted the edges by using the *canny* function
3. Defining a region inside the image where the lines should be considered by *Hough transform*, based on the look of the images
4. Applying the *Hough transform* to extract the Hough lines
5. Applying the found lines on top of the image , using draw_lines method . I modified the function to have 2 things done :
    5.1 Find all negative slope lines in the image and average their slope ml and offset bl . These represent the lane left line. Same for the positive slopes, representing the right lane line. In the same time extract the maximum and minimum y found accros all the lines, which will represent the lower and upper y coordinate limit for the extrapolated lines. THis will make sure that the extrapolated 2 lines are going to cover both lane lines equally
    5.2 Calculate the left and right segments ends x coordinates  of the lane by applying the found slopes ( ml and mr ) and offsets ( bl and br ) and then draw the 2 lines over the image
6. Apply the function *one_image_pipe* to each image from the video
    

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


Shortcoming 1 : the region to be used for Hough lines extraction is defined on an image by image basis, ignoring the previous imaages in the video

Shortcoming 2 : instead of an extrapolated line we should need an extrapolated quadratic curve extracted from Hough lines.

Shortcoming 3 : some road regions can have better contrast while other worser contrast. Therefore the edges extraction should have adaptive low_threshold and high_threshold. 

Shortcoming 4 : In some regions could be found many edges ( like shadows on the road from a tree ). 

Shortcoming 5 : There is no way devised to find the initial region of interest; the car can be in any position versus the lanes lines; 

 Note that these shortcomings I could extract them after trying to run the pipe on the challenge.mp4 video. 


### 3. Suggest possible improvements to your pipeline

A better way needs to be found to extract the region of concern. For each image it may be that the lines will curb to the right or left during the video. But curvature does not happen instantaneous, so we can use information about lane curvature from the previous image in the video to adapt the region of interest accordingly. For the firt image, since we do not know whether is curved or not, we shall start with very small region right next to  front of car and gradually extending it while adapting the region of interest

Another potential improvement could be  to find a way to remove the unusable edges . One way is to exclude edges that have slope and offset very different than the previous image found lines, but of course the firt image is going to be the one with no previous image, so will have to postpone lane extraction till a good extraction is done

