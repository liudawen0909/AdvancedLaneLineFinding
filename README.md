## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


In this project, your goal is to write a software pipeline to identify the lane boundaries in a video, but the main output or product we want you to create is a detailed writeup of the project.  Check out the [writeup template](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) for this project and use it as a starting point for creating your own writeup.  

Creating a great writeup:
---
A great writeup should include the rubric points as well as your description of how you addressed each point.  You should include a detailed description of the code used in each step (with line-number references and code snippets where necessary), and links to other supporting documents or external references.  You should include images in your writeup to demonstrate how your code works with examples.  

All that said, please be concise!  We're not looking for you to write a book here, just a brief description of how you passed each rubric point, and references to the relevant code :). 

You're not required to use markdown for your writeup.  If you use another method please just submit a pdf of your writeup.

The Project
---
[image1]: ./output_images/camera_cal/calibration3.jpg "Camera calibration image"
[image2]: ./output_images/undist_pro/undist_img1.jpg "Undistortion image of chessboard"
[image3]: ./output_images/undist_pro/undist_img2.jpg "Undistortion image"
[image4]: ./output_images/thres_binary.jpg "Thresholded binary image"
[image5]: ./output_images/bird_view.jpg "Bird view image"
[image6]: ./output_images/find_lanes.png "Find lanes image"
[video1]: ./result_full.mp4 "Video"

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

1.Camera calibration
The purpose of camera calibration is to compute the trasformation parameters. We have OpenCV functions findChessboardCorners and drawChessboardCorners to identify the corners on multiple chessboard images taken from different angles.

Then I used another OpenCV function calibrateCamera to compute the camera calibration matrix and distortion coefficeints.
Here is the output of Carmera calibration
![alt text][image1]

2.Undistortion process
This is the way to ensure the shape of the object is consistently.
Here is the output of Undistortion process
![alt text][image2]
![alt text][image3]

3.Create a thresholded binary image
In this section I create function for x and y sobel, magnitude of gradient(which is using the square root of sum of sqare x and suqare y), direction of gradient,red channel of RGB and mix of s and l channel of HLS.
And I use the way ((gradx == 1) & (grady == 1)) | ((mag_binary == 1) & (dir_binary == 1)) to combine them together.
Here is the output of thresholded binary image
![alt text][image4]

4.Perspective transform
Here I follow the slids which teach me transfer the image to effectively view from another angle which is bird-view.
I choose the source and destination point, and apply the openCV function to perform the transformation.
Here is the output of Perspective transform
![alt text][image5]

5.Find the lane boundary,Determine the curvature of the lane and vehicle position
The code given by Udacity is used for this step. As an input, the image obtained after perspective transform and binary thresholds is used. In order to fit a polynomial to lanes, the steps carried in order are (function fill_LaneLines):

Take a histogram along all the columns in the lower half of the image
Find the peak of the left and right halves of the histogram
Identify the x and y positions of all nonzero pixels in the image
Fit a polynomial to each lane using the numpy function numpy.polyfit()
To calculate the radius of curvature of each lane line in meters, Udacity provided reference is used.
Calculated the average of the x intercepts from each of the two polynomials position = (rightx_int+leftx_int)/2
Calculated the distance from center by taking the absolute value of the vehicle position minus the halfway point along the horizontal axis distance_from_center = abs(image_width/2 - position)

The polynomial fitted above is ploted on a warped image. Also the space between the polynomials is filled in to shpw the lane car is currently in. Minv is used to unwrap the image back to original from bird's eye view. This distance from centre and radius of curvature is printed on the final image.
![alt text][image6]

6.Vedio processing
I used the function above to process the video frame-by-frame.

In the video pipeline two classes are created for left and right lanes respectively. 
In this pipeline, firstly I check whether lane is detected in rpevious frame. If it was, then it only checks lane pixels which are in close proximity to the polynomial calculated in the previous frame. This way, processing is faster as no need to scan through the entire image.
![alt text][video1]

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  If you want to extract more test images from the videos, you can simply use an image writing method like `cv2.imwrite()`, i.e., you can read the video in frame by frame as usual, and for frames you want to save for later you can write to an image file.  

To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `output_images`, and include a description in your writeup for the project of what each image shows.    The video called `project_video.mp4` is the video your pipeline should work well on.  

The `challenge_video.mp4` video is an extra (and optional) challenge for you if you want to test your pipeline under somewhat trickier conditions.  The `harder_challenge.mp4` video is another optional challenge and is brutal!

If you're feeling ambitious (again, totally optional though), don't stop there!  We encourage you to go out and take video of your own, calibrate your camera and show us how you would implement this project from scratch!

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

