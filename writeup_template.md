#**Finding Lane Lines on the Road** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**
The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

[output1]: ./test_images/solidWhiteCurve_output.jpg "FinalOutputTest1"


[output2]: ./test_images/solidYellowLeft_output.jpg "FinalOutputTest2"

[outputWithDrawline1]: ./test_images/solidYellowCurve_outputv2.jpg "FinalOutputTest2"

[challenge_fitLine]: ./test_videos_output/challenge_fitLine.png "challenge_fitLine"



---
### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted the following steps:

1. Color selection (select white and yellow)

2. Apply rogion of interests.

3. Convert to grayscale and apply gaussian blur and canny edge detector

4. Apply hough transferrmation.

5. Combine detected edge with original image.

A example of final image showing here.

![alt text][output1]

![alt text][output2]


In order to draw a single line on the left and right lanes, I modified the draw_lines() function:

First, I seperate left and right by the sign of slope. To remove some random noise, I set the threshold to be 0.4, only slope larger than this value will be kept and classified.

Then, I define a helper function ave_line(), which transfer multiple line into a single line by cv2.fitLine(), which return the average slope and a point on the line. 

Finally I calculate two endpoints for each line, calculated by y value, and draw the line onto original image.

The final result shows here:

![alt text][outputWithDrawline1]

The video file can also be found in video files.

#### Challenge Video

I encontered serious problems with Challenge Video, especially with the following part, where a gray road appear in the video.

To overcome this problem, first I narrow down the color selection region. I convert image to HSV color space for better color accuracy.

Second, canny detector produce many noise in gray road, and causes large error. To reduce the effect of outliers in averaging lines, I change L2 distance in fitLine() to DIST_WELSCH.

The final results shows here. But sometime I still got incorrect prediction in this video.

![alt text][challenge_fitLine]

###2. Identify potential shortcomings with your current pipeline
Short coming:

1. The pipeline has a strong dependence on color selection. If the road line is not white or yellow, or even if we have a yellow car in front of camera, results would be much worse.

2. Classifying lines into right or left by slope is not accurate. A misclassification would introduce large error in calculating average slope.


###3. Suggest possible improvements to your pipeline

1. Modify fitLine. Currently outliers still strongly effect averaging line when using fitLine in openCV. Changing to another fitting methods (like )