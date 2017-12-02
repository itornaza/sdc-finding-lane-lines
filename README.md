# Finding Lane Lines on the Road

Identifies lane lines on the road, in an image, and in a video stream using a python notebook

[//]: # (Image References)
[image0]: ./test_images_pipeline/pipeline_0.jpg "Self driving car front camera view"
[image1]: ./test_images_pipeline/pipeline_1.jpg "Grayscale"
[image2]: ./test_images_pipeline/pipeline_2.jpg "Gaussian blur"
[image3]: ./test_images_pipeline/pipeline_3.jpg "Canny edge"
[image4]: ./test_images_pipeline/pipeline_4.jpg "Mask region"
[image5]: ./test_images_pipeline/pipeline_5.jpg "Hough transform, averaging and extrapolation"
[image6]: ./test_images_pipeline/pipeline_6.jpg "Overlay of detected lines on the original camera view"

## Installation

Set up the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md)

Go to the root of the project and run the following on the terminal:
```
$ source activate carnd-term1
$ jupiter notebook
```
## Reflection

### 1. Description of the pipeline and a brief explanation for modifications on the draw_lines() function.

My pipeline consists of 6 steps. First, I convert the original image from the self driving car from camera view to grayscale, then I apply a gaussian blur filter. The resulted more smooth grayscale image is now pushed into canny edge filter in order to get clear line segments that can be further processed. To exclude unwanted lines I further apply a region masking on the image to get only the lines at the area interest. That means that only lines that are in a more narrow area in front of the car are taken into consideration. Then, this masked image is subjected to Hough transformation in order to detect the actual lane line segments on the road, either if they are continuous or not and no matter their color. Along wit the Hough transform the lines are averaged and extrapolated so they can be seen as two continuous red lines that overlay the lane lines. In the end, I overlay the detected lanes over the original image to provide a usable result.

In order to draw a single line for the left and right lanes, I modified the draw_lines() function by first distinguishing between left and right lines. This can be done in many ways, so I chose to classsify them using the slope of the lines given by the formula m = (y2-y1)/(x2-x1).

![img](http://latex.codecogs.com/svg.latex?m%20%3D%20%5Cfra%7By2%20-%20y1%7D%7Bx2%20-%20x1%7D)

In essence, the left and right lines have opposite slopes. To get the slopes correctly I consider that the X-Y axis origin lies at the top-left corner of the image. In this way, the left line has a negative slope as we conceptualy convert the origin to the bottom-left corner. After the left and right lines are distinguished, I collect the (x, y) datapoints for each line to further process them. Using the np.polifit() function I get an averaged line and then through the np.polu1d() function I get the line's' y = m*x + b equation. I then use this equation to compute the begining and the end of each line that are finally plotted on the image.

**Stage 0 - Original image from the front camera view** 

![alt text][image0]

**Stage 1 - Grayscale**

![alt text][image1]

**Stage 2 - Gaussian blur**

![alt text][image2]

**Stage 3 - Canny edge**

![alt text][image3]

**Stage 4 - Mask region**

![alt text][image4]

**Stage 5 - Hough transform and averaging**

![alt text][image5]

**Stage 6 - Overlay on the original image**

![alt text][image6]

### 2. Potential shortcomings with my current pipeline implementation

One potential shortcoming would be if the lines are curved like on a road turn. Then the straight lines that I currently use will be enadequate to handle the situation.

Another shortcoming could be if another vehicle is taking over and is on a line segment in front of our car. Then the line would be desrupted and might cause problems in my detection pipeline.

### 3. Suggest possible improvements to the pipeline

A possible improvement would be to improve on the line averaging by using higher order polynomial functions to detect curved lines.

Another potential improvement could be to minimize the flickering of the lines on a video stream. This may be handled by frame averaging on the video.
