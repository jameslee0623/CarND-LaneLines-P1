# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[image0]: ./examples/laneLines_thirdPass.jpg "Land Lines"

[image1]: ./examples/grayscale.jpg "Grayscale"

[image2]: ./examples/line-segments-example.jpg "Line-segments"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.
    # 1. Read in the image and convert it to grayscale
    gray = grayscale(image)
![alt text][image1]
    # 2. Define a kernel size and apply Gaussian smoothing
    blur_gray = gaussian_blur(gray,kernel_size)
    # 3. Define our parameters for Canny edges detecting
    edges = canny(blur_gray, low_threshold, high_threshold)
    # 4. Defining a four sided polygon to mask for region_of_interest
    masked_edges = region_of_interest(edges, vertices)
![alt text][image2]
    # 5. Define the Hough transform parameters and run on edge detected image
    line_image = hough_lines(masked_edges, rho, theta, threshold, min_line_length, max_line_gap)
    # 6. Draw the lane lines on the image
    image = weighted_img(line_image, image) 
![alt text][image0]

In order to draw a single line on the left and right lanes, I modified the hough_lines() function by adding two fuctions in it. 
    # 1. Separate lines into left_lines and right_lines by slope and x position
    left_lines, right_lines = separate_lines(img, lines)
    # 2. Linear regression to find "averaged" slope and intercept, then Extend out the averaged lane line to the region_of_interest(vertices) for both lane
    left_right_lanes = trace_both_lane_lines(img, left_lines, right_lines)


### 2. Identify potential shortcomings with your current pipeline

For the challenge video, there are some cases that when the lighting condiction isn't good enough for lane detection. So there might be NO lines(x, y) to input to the linear regression(stats.linregress(x, y)). It will trigger an exception. For now, I just print out an error message and draw the original "lines" in green. 


### 3. Suggest possible improvements to your pipeline

The possible improvment to solve that problem will be convert the image to HSL color space, which could helps us in better isolating the lanes.
