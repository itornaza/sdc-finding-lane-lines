# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image0]: test_images_pipeline/pipeline_0.jpg "Self driving car front camera view"
[image1]: test_images_pipeline/pipeline_1.jpg "Grayscale"
[image2]: test_images_pipeline/pipeline_2.jpg "Gaussian blur"
[image3]: test_images_pipeline/pipeline_3.jpg "Canny edge"
[image4]: test_images_pipeline/pipeline_4.jpg "Mask region"
[image5]: test_images_pipeline/pipeline_5.jpg "Hough transform, averaging and extrapolation"
[image6]: test_images_pipeline/pipeline_6.jpg "Overlay of detected lines on the original camera view"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of 6 steps. First, I convert the image to grayscale, then I apply a gaussian blur filter. The resulted more smooth grayscale image is now pushed into canny edge filter in order to get clear line segments that can be further processed. To exclude unwanted lines I further apply a region masking on the image to get only the lines of interest. That means that only lines that are in a more narrow area in front of the car are taken into consideration. Then this masked image is subjected to Hough transformation in order to detect the actual lane line segments on the road, either if they are continuous or not and no matter their color. During the hough transform the lines are averaged and extrapolated so they can be seen as two continuous red lines that overlay the lane lines. In the end, I overlay the detected lanes over the original image to provide a usable result.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first distinguish between left and right lines. This is done using the slope of the lines. In essence, the left and right lines have opposite slopes. To get the slopes correctly I take into consideration that the X-Y axis origin lies at the top-left corner of the image. In this way the left line has a negative slope as we conceptualy convert the origin to the bottom-left corner. After the left and right lines are distinguished I collect the (x, y) datapoints for each line to further process them. Using the np.polifit() function I get an averaged line and then through the np.polu1d() function I get the line y=mx+b equation. I then use this equation to compute the begining and the end of each line that are finally plotted on the image.

To demonstrate how the pipeline works first we look at the original image: 

![alt text][image0]

Then after applying the grayscale step:

![alt text][image1]

The gaussian blur:

![alt text][image2]

Canny edge:

![alt text][image3]

Mask:

![alt text][image4]

Hough transform and averaging:

![alt text][image5]

Overlay on the original image:

![alt text][image6]

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the lines are curved like on a road turn. Then the linear lines that I currently use will be enadequate to handle the situation.

Another shortcoming could be if another vehicle is taking over and is on a line segment in front of our car. Then the line would be desrupted and might cause problems in my line detection algorithm.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to improve on the line averaging by using polynomial functions to detect curved lines.

Another potential improvement could be to minimize the flickering of the lines on a video stream.
