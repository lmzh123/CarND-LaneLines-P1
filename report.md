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
[image8]: ./test_images_output/result_0.jpg "Hough"
[image9]: ./test_images_output/result_5.jpg "Hough"


### 1. Lane lines detection pipeline.

First step in the pipeline is to simply load the image and convert it to gray scale for it to be easier to process and discard the color information which is not goint to be taken into account this time.

Original image             |  Gray scale image 
:-------------------------:|:-------------------------:
![][image1]                |  ![][image2]


In order to avoid salt and pepper noise the image is lightly blurred using a Gaussian Filter with a 3x3 kernel. 

![][image3]

The next step in the pipeline is to perform Canny edge detection whose low and high thresholds were raised to 150 and 225 correspondingly looking to avoid noisy edges by only retaining the most sharp ones in the image, hopefully, the ones corresponding to the lane lines.

![alt text][image4]

Using the function `region_of_interest()` the image obtained using Canny is masked with a polygonal region using the `cv2.bitwise_and()` function from OpenCV. The vertices of this region were determined as follows:

| Point | X        | Y        |
| ----- |:--------:| --------:|
| 1     | 50       | h        |
| 2     | (w/2)-50 | (h/2)+50 |
| 3     | (w/2)+50 | (h/2)+50 |
| 4     | w-50     | h        |

Mask                       |  Masked edges image 
:-------------------------:|:-------------------------:
![][image5]                |  ![][image6]

Finally from these resulting edges the lines are obtained using the Hough's transformation. OpenCV posses the `cv2.HoughLinesP()` which performs the transformation and determines which of these segments correspond to lines and which don't based on different parameters. From these parameters the *threshold* and *minLineLength* were tuned looking for consistent and long lines, this means raising both parameters experimentaly.

![alt text][image7]

### 2. Lines extrapolation

Using the slope of a line equation it can be determined which of the remaining lines correspond to both the left and right line using an if statement within the `draw_lines()` function. For instance if the slope of a line is positive it corresponds to the right line otherwise it belongs to the left line.

![](https://latex.codecogs.com/gif.latex?m%20%3D%20%5Cfrac%7By_%7B2%7D-y_%7B1%7D%7D%7Bx_%7B2%7D-x_%7B1%7D%7D)

Once the left and right lines are grouped the average of such lines are obtained using the function `np.average()`. From there the equations of these lines are calculated using the `np.polyfit()` function. Because of this first order equation it is possible to extrapolate this mean line to the same extents of the region of interest used before.

 Hough's lines             |  Mean extrapolated lines
:-------------------------:|:-------------------------:
![][image8]                |  ![][image9]

For matters of displaying the thickness of the line drawn is increased to 10.

### 3. Optional challenge

Trying to improve the performance of this pipeline with the challenge video a small change was made that even tought was not a complete solution to the problem, it improved the results obtained. It can be noticed that the car's bumper can be seen by the camera and that bigger barriers are located at the side of the road.

To tackle this problem the lines were filtered according to their slope, in this case the lines that where close to be completely horizontal were discarded and this corresponds to avoid lines whith slopes between -0.1 and 0.1.

```
if ((y2-y1)/(x2-x1)) > 0.1:
    right = np.vstack((right, np.array([x1, y1, x2, y2])))
elif ((y2-y1)/(x2-x1)) < -0.1:
    left = np.vstack((left, np.array([x1, y1, x2, y2])))
```
### 4. Potential shortcomings

This pipeline is highly tuned for this particular setup and as shown by the challenge video it could have problems with the following.

* Overfitted: This algorithm is not robust enough to handle well when there are other lines in our region of interes besides the lane lines.
* Rotations or traslations of the camera: It is highly dependant of getting the same kind of images everytime and any change in the region of interes could be a problem.
* Light conditions: Edge detection can be a problem depending of the amount of light reflected by the road and also by the shadows that could be seen in or region of interest.

### 5. Possible improvements

For improvements of this pipeline the following is proposed:

* After the canny edge detection a set of morphological operators could be applied with a kernel that looks for diagonal lines. This could help to discard any other kind of lines that we are not interested in having.
* After obtaining the gray scale image it could be helpfull to apply an histogram equalization to it so the sharpnes of the image is improved and it is more inmune to light changes.
* Further tuning can be done for clustering which lines correspond to the left and right lines. For instance an outlier detection algorithm such as RANSAC along with a clustering algorithm like k-nearest neighbor could lead to a better model and more robust when different lines are seen within our region of interest. 
