# **Finding Lane Lines on the Road** 

## Luis Miguel Zapata

---

**Finding Lane Lines on the Road**

This projects aims to develop a simple Computer Vision pipeline in order to the detect the lane lines seen by a camera located in a car's bumper.  

[image1]: ./screenshots/original.jpg "Original"
[image2]: ./screenshots/gray.jpg "Grayscale"
[image3]: ./screenshots/smoothed.jpg "Smoothed"
[image4]: ./screenshots/canny.jpg "Canny"
[image5]: ./screenshots/mask.jpg "Mask"
[image6]: ./screenshots/masked.jpg "Masked"
[image7]: ./screenshots/lines.jpg "Hough"

### 1. Lane lines detection pipeline.

First step in the pipeline is to simply load the image and convert it to gray scale for it to be easier to process and discar the color information which is not goint to be taken into account this time.

Original image             |  Gray scale image 
:-------------------------:|:-------------------------:
![][image1]                |  ![][image2]


In order to avoid salt and pepper noise the image is lightly blurred using a Gaussian Filter with a 3x3 kernel. 

![][image3]

The next step in the pipeline is to perform Canny edge detection whose low and high thresholds were raised to 150 and 225 correspondingly looking to avoid noisy edges by only retaining the most sharp ones in the image, hopefully the ones corresponding to the lane lines.

![alt text][image4]

Using the function `region_of_interest()` the image obtained using Canny is maked with a polygonal region using the `cv2.bitwise_and()` function from OpenCV. The vertices of this region were determined as follows:

| Point | X        | Y        |
| ----- |:--------:| --------:|
| 1     | 50       | h        |
| 2     | (w/2)-50 | (h/2)+50 |
| 3     | (w/2)+50 | (h/2)+50 |
| 4     | w-50     | h        |

Mask                       |  Masked edges image 
:-------------------------:|:-------------------------:
![][image5]                |  ![][image6]

![alt text][image5]
![alt text][image6]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
