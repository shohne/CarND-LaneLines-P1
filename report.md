# **Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect about the solution, its shortcomings and possible improvements.

### Pipeline Overview

Each image or video frame is processed following the steps:
1. Filtering white and yellow image channels;
2. Creating a join image with white and yellow channels;
3. Converting image to grayscale;
4. Appling Canny filter to detect edges;
5. Isolating only a region of image (lane lines are located at the center and bottom);
6. Executing Hough algorithm to detect lines;
7. Filtering detect lines in step 6 to consider only lines with *correct* slope;
8. Grouping filtered lines in two groups: left lines and right lines;
9. Calculating the *mean* line;
10. Extrapolating the *mean* line to cross the bottom border and goes to the middle height;

### Detailed Steps
Visualize the pipeline showing the result for each step. Consider the original image:

![](./report_images/original.png)

In **step 1** there are two images, one selecting the yellow channel:

![](./report_images/yellowchannel.png)

Another image with white channel:

![](./report_images/whitechannel.png)

In **step 2**, we join yellow and white images:

![](./report_images/joinwhiteandyellowchannel.png)

In **step 3**, we convert to grayscale:

![](./report_images/gray.png)

Now, in **step 4**, we apply Canny filter to detect edges:

![](./report_images/gray.png)

**Step 5**: isolate only the area of interest:

![](./report_images/onlyregionofinterest.png)

Nex, **steps 6 through 10**:
- execute Hough algorithm to detect lines in image;
- select lines with *correct* slope (absolute value between 0.5 and 10.0) [*draw_lines function*];
- classify lines in two groups using its slope direction: separate right lines and left lines [*draw_lines function*];
- calculate the *mean* line for each group [*draw_lines function*];
- extrapolate these lines to cross the horizontal line at the bottom (x axis) and horizontal line in the middle [*draw_lines function*];

As result:

![](./report_images/houghandrawlines.png)

Finally, join original image and lane lines image:

![](./report_images/orignialpluslinesdetect.png)


### Shortcomings

The main drawback of this method is that detects only straight lines, but real lanes could be curved too. The movie *challenge.mp4* shows a car driving in a highway turning right, it is easy to see that the proposed pipeline fails to detect the *correct* lane. 

Another shortcoming is not workng well when there is very little contrast between lane lines and the pavement.

### Possible Improvement

I believe that the most effective and cheap improvement would be to calculate the lanes getting the current image and considering previous frames too. The reason is that lanes do not change abruptly in a short period of time.

Another obvious improvement would be the detection of curved lanes (there is a Hough procedure to detect curved lines).
