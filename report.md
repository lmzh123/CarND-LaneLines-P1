# **Finding Lane Lines on the Road** 

## Luis Miguel Zapata

---

**Finding Lane Lines on the Road**

This projects aims to develop a simple Computer Vision pipeline in order to the detect the lane lines seen by a camera located in a car's bumper.  

[image1]: ./screenshots/original.jpg "Original"
[image2]: ./screenshots/gray.jpg "Grayscale"
[image3]: ./screenshots/smoothed.jpg "Smoothed"
[image4]: ./screenshots/canny.jpg "Canny"
[image5]: ./screenshots/masked.jpg "Masked"
[image6]: ./screenshots/lines.jpg "Hough"

### 1. Lane lines detection pipeline.

First step in the pipeline is to simply load the image and convert it to gray scale for it to be easier to process and discar the color information which is not goint to be taken into account this time.

Original image             |  Gray scale image 
:-------------------------:|:-------------------------:
![][image1]                |  ![][image2]


In order to avoid salt and pepper noise the image is lightly blurred using a Gaussian Filter with a 3x3 kernel. 

![][image3]

![alt text][image4]
![alt text][image5]
![alt text][image6]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
