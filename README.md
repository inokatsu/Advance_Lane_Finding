

# Advanced Lane Finding Project
![alt text][image0]
#### The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image0]: ./output_images/P4_result_video.gif "result_gif"
[image1]: ./output_images/undisorted_chessboard_images/undistorted_chess_board.jpg "Undistorted"
[image7]: ./output_images/undisorted_chessboard_images/calibration2_out.jpg "chess board"
[image2]: ./output_images/undisorted_road_images/test6_out.jpg "Road Transformed"
[image3]: ./output_images/combined_image.png "Binary Example"
[image4]: ./output_images/birds_eye_view_images//straight_lines1wraped_out.jpg "Warp Example"
[image5]: ./output_images/histogram.png "Historgram"
[image8]: ./output_images/slide_window.png "Sliding Window Search"
[image6]: ./output_images/final_output_image.png "Output"
[video1]: https://youtu.be/99i7jlgrorc "Video"


---


## Camera Calibration


I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

![alt text][image7]


I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]
  
  
## Image processing

#### 1. Apply a distortion correction to raw images

I used distortion coefficients which I got from camera calibration process and calibrate the road image like this:
![alt text][image2]

### 2. Color Transformaion

I used sobel operator x direction and S channel of HSL to generate a binary image. The example of image of this step is following:

![alt text][image3]



### 3. Prospective transform

The code for my perspective transform includes a function called `perspetive_transform()`, in the 8th code cell of the Jupyter notebook.  The function takes as inputs an image (`img`) and return warped image(bird eye view image):


The following source and destination points are chosen:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 740, 460      | 1280, 0        | 
| 1230, 670      | 1280, 720   |
| 70, 670     | 0, 720      |
| 545, 460      | 0,  0        |

The undistort image and the transformed image are following:

![alt text][image4]


### 4. Detect lane pixels (sliding window search)

To find the base likely positions of the 2 lanes, I calculated the peak position from the histogram.

![alt text][image5]

Then, I performed a sliding window search.  
![alt text][image8]

### 5. Determine the curvature of the lane and vehicle position with respect to center

I did this calculation in the function `calc_curvature` for the curvature of the lane and function `draw_ploy` for the vehicle position. I converted from pixels space to meters in the calculation

### 6. Final output

Finally, I performed an inverse perspective transformation and conbined the processed image with the original image.
![alt text][image6]

---

### Pipeline (video)

Here's a [link to my video result](https://youtu.be/gc-QphMgn78)

---

### Discussion

#### 1. Further improvement

Further improvement woudl be to fit this lane finding algorithm for the other driving conditions like driving on steep hills, in heavy traffic, rain, snow etc. 
