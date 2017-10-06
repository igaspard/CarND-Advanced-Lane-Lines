## Advanced Lane Finding Project - Gaspard Shen

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/undistort_output.png "Undistorted"
[image2]: ./test_images/test5.jpg "Road Transformed"
[image3]: ./output_images/binary_combo_example.png "Binary Example"
[image4]: ./output_images/warped_example.png "Warp Example"
[image5]: ./output_images/lane-lines_example.png "Fit Visual"
[image6]: ./output_images/example_output.png "Output"
[video1]: ./project_video_out.mp4 "Video"

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in `advlaneline.ipynb`

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result of the 'test_images/straight_lines1.jpg'

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images `test5.jpg`:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

After I try lots of the combination of color and gradient thresholds to generate a binary image (thresholding steps at Section 3 in IPython notebook `advlaneline.ipynb`). In the end, I use the sobel, direction and magnitude of the gradient and color. Here's an example of my result for this step. As you can the lanes line was marked clearly which help to following steps.

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in the section 4 of the IPython notebook). The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[580, 450],
     [760, 450],
     [160, 688],
     [1140,688]])

offset = 30
dst = np.float32(
    [[offset, offset],
     [img.shape[1]-offset, offset],
     [offset, img.shape[0]-offset],
     [img.shape[1]-offset, img.shape[0]-offset]])
```

This resulted in the following source and destination points:

| Source        | Destination   |
|:-------------:|:-------------:|
| 585, 450      | 30,  30       |
| 160, 688      | 30,  690      |
| 1140,688      | 1250,30       |
| 760, 450      | 1250,690      |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

First use histogram to find out the two peak point, those are the left and right lane-line candidate. Then using the sliding windows search to postions the polynomial.
Then I did some other stuff and fit my lane lines kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in the section "Measuring Curvature" in IPython notebook function LaneCurvature()

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in clase Line(), inside the find_lane() methods.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_out.mp4)

---
### Discussion

#### Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

1. At first, the thresholded method result was not good, the right lane line was not clear. So the right lane line was result was not good. Then I tried several combination, finally can make the lane line much clear than beginning.
2. Regarding to the src/dst of the perspective transform, i haven't find a way instead of hardcode. Sometimes in the `challenge_video.mp4` the lane-line look like too ahead and make the lane line mark not very well.  
