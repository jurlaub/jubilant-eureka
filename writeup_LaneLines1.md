# **Finding Lane Lines on the Road** 

## Writeup  - Joshua Urlaub


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

My pipeline consists of the steps outlined in the lessons. The majority of the steps can be seen in the assess_frame() function in the note book. This function calls a number of other methods to process a grayscale image and finally returns a matrix of the same size as the video frame that containes lines to be added to the original image / video frame. 

I experiemented with the pipeline a little before modifying the draw_lines() function. I will discuss this more in the shortcomings section. 

In the draw_lines function I essentially create a complete line (line from middle of the screen to bottom) by grouping lines by negative and positive slope, calculating and collecting the lower x intercept, and extrapolating a final line from the mean slope and mean x intercept. Almost all individual lines are drawn on the screen in a red color. The final average line is drawn in green.
There are a couple of error handling steps - horizontal and verticle lines are skipped over. Also, while processing video, I encountered a number of "ValueError: cannot convert float NaN to integer" errors that stopped the video processing. This occurred even when I switched from numpy.average to numpy.nanmean - which I expected to handle the issue. Without digging to much more into the problem, I decided to skip that frame when encountering the error. 




### 2. Identify potential shortcomings with your current pipeline


One shortcoming is that the pipeline basically skips frames to handle the float NaN to integer error. This creates some flickering on the screen and there is not currently a way of knowing how often this happens. Identifying the exact problem could be important. 


One big shortcoming is that the image filtering can only be optimized for a certain portion of the image frame. Maximizing for the near part of the frame occuludes the far part of the frame. One possible solution is to segment the image frame and optimize gaussian, canny, and hough space values for the subsection based on the distance from view. The problem is manifest by the drawn lines not perfectly matching to the lane markings as the road fades into the distance. 

Python is great for learning but not great for speed - switching to C++ could help with processing time. Along with that, I am sure there are a number of inefficencies with my code so that could also use some optimization.


The draw line function could use some more filtering with regards to filtering out line segments. Lines are often identified that cause the average line to go wild off the road. Looking at the data, some small lane markings are included in the list which cause abberations in the data. The Average then goes wildly off in the weeds. Statistical filtering could help with this. 


### 3. Suggest possible improvements to your pipeline


A big and important change would be for the system to track line data from frame to frame. The current system basically has amnesia when it comes to the previous frames. Each frame requires the pipeline to recalculate the lower x intercept and to figure out the upper line boundary. 


Along with tracking the lines, there are a number of other clues about the lane in the image. Some clues could be used as backup data if the primary lanes cues disappear. By clues, I mean the edge of the grass, the other lanes, the baracades, the other cars.



