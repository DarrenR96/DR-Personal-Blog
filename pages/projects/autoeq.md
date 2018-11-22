---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
permalink: /projects/autoeq/
categories: projects
---

## Realtime Auto Equalizaiton System Based on Acoustic & Visual Feedback

This project was a 9-month long venture in conjunction with the Univeristy of the West Indies.

_The actual project report is 80 pages long and quite technical. If you want to know more about it, send me an [email](mailto:darrenramsook@outlook.com)._

![alt text](/img/lms.png "Project in action")
_Fun Audio Processing_

---

### Project Introduction

The playback quality of sound is a critical aspect on the impact it has to the psychoacoustic effect of the listener. Music plays a key role in the bar and restaurant industry as the genre and tempo of the music played has an influence on the amount of money spent by the average patron. However most of these environments are noisy and dynamic in nature, leading the audio signal to decay and become corrupted. The traditional response to counteract this problem would be to raise the entire volume of the output channel signal to the audience’s liking in order to mask the inconsistencies in the signal. This approach requires manual participation and knowledge of the room acoustics by the user to properly equalize the signal. Even if the signal is equalized properly, any deviation in the environment would mean that the quondam settings would need to be redone to satisfy the new environment. This paper details the design and prototyping of an Automatic Equalization (AutoEQ) system.

---

### General Idea

This system detected acoustic and visual changes in the environment and equalized the audio signal to suit. The acoustic changes were monitored through an audio interface and microphone connected to a computer. The noise was then filtered out and the signal was compared against an equalization template based on the genre of music. The visual changes, which were related to the position of people in the room, was detected through a camera interfaced with a computer. This positioning of listeners in the room was then related to the positioning of the speakers in the room. The information obtained by both these processes then allowed to system to adjust the signal accordingly and played back through a multichannel speaker system. This system was designed to be operated without regular human input.

---

### Image Feedback

The OpenCV library was used to extract the location and number of people in a given image. A Background Subtraction with Kalman Filtering approach was utilized to do this.  
Tracks were created as structure to maintain the status of an object being tracked. Within the track structure the following information was recorded for each object:

- Identification Number of the track
- The bounding box of the object
- Kalman Filter used for tracking
- Counter that indicates the number of frames since the track was initially detected
- Number of frames that the track was visible
- Number of frames that the track was invisible

![alt text](/img/opencv1.png "Project in Action") ![alt text](/img/opencv2.png "Project in Action")

---

### Audio Feedback

Adaptive filtering technqiues were utilized and compared against each other to determine the best noise removal filter. This was found the be the LMS filter. This was done through a computer simulation and real world experiment where a singal is corrputed and then fed into the Noise removal sub system.
