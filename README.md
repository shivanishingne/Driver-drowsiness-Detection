# Driver-drowsiness-Detection
Built using Python, OpenCV, and dlib libraries, this computer vision project intends to detect whether the driver is getting drowsy in real-time, and will set off an alarm to alert the driver.

### 💽Requirements:
The code is implemented using Python ([version 2.7](https://www.python.org/download/releases/2.7/) or higher will work)

### 💽Dependencies:
* **SciPy**: We’ll need the SciPy package so we can compute the Euclidean distance between facial landmarks points in the Eye Aspect Ratio calculation.
* **imutils**: We’ll also use the imutils package, a series of computer vision and image processing functions to make working with OpenCV easier.
* **Thread**: We'll import the Thread class so we can play our alarm in a separate thread from the main thread to ensure our script doesn’t pause execution while the alarm sounds.
* **playsound**: We'll need the playsound library to play simple sounds like our MP3 alarm.
* **dlib**: To detect and localize facial landmarks we’ll need the dlib library.


### 💽Algorithm:
1. Localize the face in the video frame
2. Detect the key facial structures on the face ROI
3. Extract out the eyes landmarks.
4. Calculate EAR (eye aspect ratio)
5. Threshold the EAR to determine if it is below certain threshold and increment the counter.
6. If the person's EAR is below threshold for more than certain number of frames, alert the driver by playing the sound.

### 💽Command to run the application:
```
python detect_drowsiness.py \
   --shape-predictor shape_predictor_68_face_landmarks.dat \
   --alarm alarm.wav
```

---
#### Eye Aspect Ratio:
* Each eye is represented by 6 (x, y)-coordinates, starting at the left-corner of the eye (as if you were looking at the person), and then working clockwise around the eye.
* The Eye Aspect Ratio is given by the following formula:
```
E.A.R = ||p2 - p6|| + ||p3 - p5||
        _________________________
             2 * ||p1 - p4||
```
* The EAR will be approximately constant when the eye is open, and when the eye is closed. However, the ratio will be much smaller than the ratio when the eye is open.
* During a blink, the value will rapidly decrease towards zero.
(Ref: Soukupová and Čech’s 2016 paper, [Real-Time Eye Blink Detection using Facial Landmarks](http://vision.fe.uni-lj.si/cvww2016/proceedings/papers/05.pdf))
* In our drowsiness detector case, we’ll be monitoring the eye aspect ratio to see if the value *falls* but does *not increase again*, thus implying that the person has closed their eyes.


#### dlib's face detector:
* The dlib's face detector is an implementation of **One Millisecond Face Alignment with an Ensemble of Regression Trees** paper by Kazemi and Sullivan (2014).
* The face detector detects 68 coordinates for any given face.
* dlib's framework can be trained to predict any shape. Hence it can be used for custom shape detections as well.
* Here, we are using dlib's pre-trained face detector based on the modification of the standard **Histogram of Oriented Gradients** + **Linear SVM** method for object detection.
---


### Reference:
* Special thanks to [Adrian Rosebrock](https://pyimagesearch.com/). This project is inspired from his blog: Drowsiness detection with OpenCV.
* Soukupová and Čech’s 2016 paper, [Real-Time Eye Blink Detection using Facial Landmarks](http://vision.fe.uni-lj.si/cvww2016/proceedings/papers/05.pdf)
