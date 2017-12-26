## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/camera_cal.jpg "Camera Calibration"
[image2]: ./output_images/undistorted_image.jpg "Undistorted camera Image"
[image3]: ./output_images/binary.jpg "Binary"
[image5]: ./output_images/combined_binary.jpg "Combined Binary"
[image6]: ./output_images/corners.jpg "Corners"
[image7]: ./output_images/warped.jpg "Warp Example"
[image8]: ./output_images/warped_roi_bin.jpg "Warped ROI"
[image9]: ./output_images/histogram.jpg "Find Lanes"
[image10]: ./output_images/sliding_fit_vis.jpg "Sliding Fit"
[image11]: ./output_images/sliding_search_vis.jpg "Sliding Search"
[image12]: ./output_images/measure_curve_vis.jpg "Measure Curve"
[image13]: ./output_images/highlight_lane_vis.jpg "Draw Lane"
[video1]: ./output_images/project_video_output.mp4 "Video"
[video2]: ./output_images/project_video_challenge_output.mp4 "Challenge"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

### Camera Calibration

The code for this step is contained in the first code cell of the IPython notebook located in "./calibrate_camera.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

I save the calibration and distortion coefficients to a file so that they can be read in by future porgrams and used to un-distort the images without having to re-run the camera calibration prcess again.

### Pipeline (single images)

#### 1. APplication of Camera Calibration and Distortion Coefficients.

To demonstrate this step, I have applied the correction to the below captured test image:
![alt text][image2]

#### 2. Image Thresholding.

To make lane detection more robust I perform gradient and color thresholding to generate a binary image which helps the lane lines to stand out. Teh combination of color and gradient thresholds to generate a binary image can be found at lines 94 through 120 in `processing_pipeline.ipynb`.  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp()`, which appears in lines 46 through 58 in cell 2 of the  `processing_pipeline.ipynb` notebook.  The `warp()` function takes as inputs an image (`img`), and applies pre-determined source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 200, 720      | 350, 720      | 
| 580, 460      | 350, 0        |
| 705, 460      | 980, 0        |
| 1130, 720     | 980, 720      |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image6]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image7]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Video pipeline output of Project video.

The video pipeline created performs reasonably well on the project video ![link to my video result][Video1]

---

### Discussion

My first pass at building the video pipeline showed that shadows would cause the algorithm to lose track of the lane detection.  Filtering the image a little more made the algorithm work well on the project video.  This solution will not work as well where the captured images of the road are less clear or lane markers are not as well defined.  The algorithm would certainly have issues with varying weather conditions and the amount of daylight.  To create a more robust algorithm would require to re-think the histogram algorithm for determining lane lines and use an approach that takes more variables into consideration to determine where the lane exists in the image.  Adaptive color thresholding can also be used to adapt the lane detection to varying degrees of lighting which will also increase the reliability of the algorithm to determine where the lane exists in the image.  
