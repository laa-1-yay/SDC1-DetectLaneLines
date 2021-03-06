# **Finding Lane Lines on the Road** 


Code : Laav_Lane_Lines_Detection.ipynb   

Summary : writeup_Laav_Lane_Lines_Detection.md



**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/test_region_area.png "REGION OF INTEREST"
[image2]: ./test_images_output/solidYellowCurve2_out.png "LANE DETECTION"
[image3]: ./test_images_output/test.png "LANE DETECTION"

---

### Reflection

### 1. Description ofr pipeline.    

My pipeline consisted of 5 steps :  

- Put an iterator for each sample image
- For each image, I first converted it to grayscale
- Performed gaussian blur
- Did Canny Edge detection
- Assigned values to vertices for Region of interest
- Apply Hough Transformation on the Region of interest
![alt text][image1]
- Tweaked the values of kernel size, rho, theta, threshold, min_line_len, max_line_gap (Resultant lines were continous and long)
![alt text][image3]
- Got an image with lanes drawn on it after calling Hough Transformation
	-  First with normal draw_lines
	-  Then with draw_lines_improved (draw one single thick line from bottom till center after avg/ extrapolating to detect lane)    
		-  First segragate left lines and right lines (according to slope)       
		-  Used numpy.polyplot to find value which returns slope and intercept after fitting multiple lines   
		-  **P.S.** Tried two other methods to achive avg/ extrapolation (explained in improvements)   
- Merged that image with my original image to get final output
![alt text][image2]
- Did the same with video (multiple images)
- Stored all the outputs in corresponding output folders



### 2.Potential shortcomings with current pipeline


Following are the shortcomings :  
- When lanes are curved, there is problem in detection
- If noisy data is present in the region of interest, final lane lines are changed
- All this is dependent on region of interest; slight chang would need new adjustments, dependent on the positioning of camera on the car


### 3. Possible improvements to pipeline

A possible improvement would be to detect outliers in the segregated left lane lines and right lane lines, remove them and then average the lane lines and draw a final single lane line.

Worked on this - 

1. SOLUTION 1 :    
Used mean and standard deviation on array of segregated slopes to remove outliers and then find the mean after cleaning array of slopes. 
Stuck at how to find the corresponding value of intercept; in order to use both slope and intercept in calculating x1 and x2 for drawing a line when we know y1 = bottom most point and y2 = height of centre of image (or region of interest height)

2. SOLUTION 2 :    
Find the upmost and bottom most x values (we already know the y values ) and draw a line from bottom to up / OR / from bottom to centre of region of interest
   (Working fine, but problem with example.mp4)

