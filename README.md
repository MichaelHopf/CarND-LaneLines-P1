
# **Finding Lane Lines on the Road** 
<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

## Writeup Michael Hopf




### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The general idea of my pipeline was as follows:
 - Grayscale the image.
 - Apply Gaussian blur.
 - Apply Canny edge detection.
 - Define two masks for the left and right lane.
 - Do the Hough transformation for each lane.
 - Define a new function that deletes line segments given by the Hough trasnformation that do not align well with the general orientation.
 - Do a weighted least squares fit with the endpoints of the remaining line segments. Longer line segments carry more weight.
 - Extrapolate the fitted lines.

These steps were already sufficient to get good results on the six images. Also, applied to the first two videos, the fit was good. However, to get a nicer result, I did a convex combination of the current lanes with the lanes calcualted in the pervious frame. This way, the result was much smoother.

Applying this pipeline to the challenge worked okay, but definitely much worse than for the other two videos. The main problems were the shadow as well as the bright part of the road directly afterwards. In particular, the Canny edge detection did not gather enough information to obtain good results.

I applied a rather simple idea: The upper and lower endpoints of the lanes have approximately the same distance all the time. So, with a few lines of code, I included a correction after the lane computation that prevented too large changes.



### 2. Identify potential shortcomings with your current pipeline


My lanes for the challenge video are definitely improvable. They still jump around too much. Also, the fit for the other two videos seems good, but it is not completely perfect. In particular, this happens when the road is bumpy.


### 3. Suggest possible improvements to your pipeline

Some improvements one could implement:
- The roads itself are smooth, i.e., the change of the lanes from one frame to the next should be about the same.
- For the weighted least square fit, one could add more weight to the edges close to the car since these can be identified better.
- The road has a fixed width. This should be of help when the information for one lane is bad but for the other one is good. Then, one could use the reliable lane to compute the other one.
- Compare more frames than just the current one and the last one.

