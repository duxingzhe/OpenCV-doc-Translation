Goal

Whenever you work with video feeds you may eventually want to save your image processing result in a form of a new video file. For simple video outputs you can use the OpenCV built-in cv::VideoWriter class, designed for this.

* How to create a video file with OpenCV
* What type of video files you can create with OpenCV
* How to extract a given color channel from a video

As a simple demonstration I'll just extract one of the BGR color channels of an input video file into a new video. You can control the flow of the application from its console line arguments:

* The first argument points to the video file to work on
* The second argument may be one of the characters: R G B. This will specify which of the channels to extract.
*The last argument is the character Y (Yes) or N (No). If this is no, the codec used for the input video file will be the same as for the output. Otherwise, a window will pop up and allow you to select yourself the codec to use.

For example, a valid command line would look like:

```
video-write.exe video/Megamind.avi R Y
```