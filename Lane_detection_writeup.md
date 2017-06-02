# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

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

####Image processing pipeline

The pipeline consists of the following steps:
1. Read in the image and make a copy using np.copy
2. Convert it to grayscale
3. Apply the gaussian_blur function with a kernel of 5x5
4. Apply the openCV canny function with a low threshold of 130 & high threshold of 150
5. Define a region of interest- In the interest of time, I had to hardcode the region of interest values. It is possible to calculate the region of interest using the image shape values and assigning the vertices of the ROI polynomial as fractions of the image size. 
I defined the ROI as below:
    leftbottom = (140,539)
    rightbottom= (900,539)
    lefttop=(400,325)
    righttop=(570,325)

 6. I defined the ROI vertices given below as a numpy array and applied the ROI on the image returned by the canny function

 7. Next the parameters required for the hough_lines finction are defined:
rho = 1 # distance resolution in pixels of the Hough grid 
theta = np.pi/180 # angular resolution in radians of the Hough grid
threshold = 5     # minimum number of votes (intersections in Hough grid cell)
min_line_length = 15 #minimum number of pixels making up a line
max_line_gap = 50    # maximum gap in pixels between connectable line segments

 8. The hough_lines function is called as given below:
hough_lines(maskedImage,rho,theta,threshold,min_line_length,max_line_gap)
where masked image is the image returned by applying the region of interest on the canny edge detected image
9. The image returned by the hough_lines finction is a black image with only the lines drawn. The openCV weighted function is used to superimpose the lines on the original image to give the output.


The draw_lines function had to be modified to extrapolate the line segments to the bottom of the image.
To do this, I divided the image into left half and right half using x=500 as the midpoint. 

Then calculated the lowest and highest points of the left line and lowest and highest points on the right line.

Then I calculated the slope (m) of both the ines and the distance form the x axis when y=0 (b)

   mleft = (midyleft-lowy)/(midxleft-lowx)
    bleft = lowy - (mleft*lowx)
    botleftx = math.floor((539-bleft)/mleft)
    
    mright = (midyright-highy)/(midxright-lowx)
    bright = highy - (mright*highx)
    botrightx = math.floor((539-bright)/mright)
  
  calculated the x values where y=539 for both left line and right line

Using the openCV line function, drew a line connecting the top point to the calculated bottom x and y.

### 2. Identify potential shortcomings with your current pipeline


The draw_lines function needs to be improved to work with shorter line segments to handle curves. I will learn more about the openCV library and image processing and attempt it later.

The ROI definition is currently hardcoded. Needs to be changed to define a good ROI for a front facing camera using image shape values.
That would also help with the challenge video


### 3. Suggest possible improvements to your pipeline

The draw_lines function needs to be improved to work with shorter line segments to handle curves. I will learn more about the openCV library and image processing and attempt it later.

The ROI definition is currently hardcoded. Needs to be changed to define a good ROI for a front facing camera using image shape values.
That would also help with the challenge video
