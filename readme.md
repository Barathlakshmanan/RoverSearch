## Project: Search and Sample Return

---


**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook). 
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands. 
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./misc/rover_image.jpg
[image2]: ./calibration_images/example_grid1.jpg
[image3]: ./calibration_images/example_rock1.jpg 
[image4]: ./misc/result.png
[image5]: ./misc/result1.png
[image6]: ./misc/grid_img.png
[image7]: ./misc/perspective_transform.png
[image8]: ./misc/color_segmentation.png
[image9]: ./misc/coordinate_transform.png

## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it!

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.
To identify rock and obstacle, the function 'color_thresh' in line #6 of file perception.py is modified. Selecting obstacle was straight forward - it is the area other than the drivable path. In order to identify the rock samples, the color space of the image was converted from RBG to HSV and appropriate thrshold was selected using trial and error method to isolate the region.
Input image for  'color_thresh' function:

![alt text][image3]

The result of thresholding is shwon in image below (in the order of drivable path, obstacle, rock)

![alt text][image8]

#### 2. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 
The following steps were added to process_image() function:
1. Perspective transform was applied to the input image using predefined source and destination points. (See image below)
![alt text][image9]
2. Color threshold was applied to the transformed image to identify drivable path and obstacles.
3. To find rocks, color thrshold was applied to the original image and then the it was transformed.
4. The drivable path was converted from rover-centric pixel positions to polar coordinates.
5. Necessary parameters were updated to Rover class object and returned.
The video is in output folder of this project.

### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.
The decision_step() was pre-populated and it was good enough to meet the criteria. For the perception_step() the threshold values obtained from notebook analysis was directly used for color segmentation. Beyonf the code from notebook analysis. Code was added to calculate the drivale path locations angle and length from robot's center in order to calculate the steering angle.  

#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

1. The world map value was increased by 1 for drivable path and obstacle whereas for rock it was always set to 255. This was done to make sure that the rocks are identified wven with a single pixel (100 pixel in robot cord gets mapped to 1 in world coord).
2. The pipeline will fail if it is has some bad initial state such that the robot will try to map only certain region of the map. Adding some amount of randomness to steering angle will overcome this issue. 
3. Optimizing fidelity & accuracy by having a better model of the bot, optimizing color threshold function and collecting rocks are the areas I would like to work going forward!

Recorded video is available in output folder. Following properties were set for the simulator.

Screen Resolution: 1024 x 768

Graphics Quality: Good


![alt text][image5]


