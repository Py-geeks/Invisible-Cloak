# Invisible-Cloak
Everyone must be familiar with Harry Potter and the Deathly hallows. The third hallow or gift from death was an invisible cloak. You can make the same with image processing. Lets see how!!!

### Pre-reqs:
1-openCv-This library stands for open source computer vision.This python library designed to solve computer vision problems.This library contains various functions to manipulate arrays
and many more.you can impliment this with all kinds of IDLEs.
2-numpy -NumPy is a python library used for working with arrays. It also has functions for working in domain of linear algebra, fourier transform, and matrices. NumPy was created in 2005 by Travis Oliphant. It is an open source project and you can use it freely.

### Installing libraries:
         pip install numpy
         pip install opencv-python as cv2(variable)

### Importing libs:
         import cv2
         import numpy as np
         import time
         
### Object creation:
         video = cv2.VideoCapture(0,cv2.CAP_DSHOW)
         time.sleep(3)
         
### Taking loop for video validation(capturing background)
         for i in range (60):
             check,background = video.read()
             background = np.flip(background,axis = 1)

         while(video.isOpened()):
             check,img = video.read()
             if check==False:
                 break
             img = np.flip(img,axis = 1) 
             
We are using red cloth hence detecting them

             hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
             lower_red = np.array([0,120,50])
             upper_red = np.array([10,255,255])
             mask1 = cv2.inRange(hsv, lower_red, upper_red)
             
Red color from top of color pallete

             lower_red = np.array([170,120,70])
             upper_red = np.array([180,255,255])
             mask2 = cv2.inRange(hsv, lower_red, upper_red)
             
Red color from bottom of color pallete

             mask1 = mask1 + mask2
             mask1 = cv2.morphologyEx(mask1, cv2.MORPH_OPEN, np.ones((3,3), np.uint8))
             
Morphing is used to avoid noise of recorded video

             mask1 = cv2.morphologyEx(mask1, cv2.MORPH_DILATE, np.ones((3,3), np.uint8))
             
Red part in vdo-white

             mask2 = cv2.bitwise_not(mask1)
             
forming mask2 oppo to mask1,Red part in vdo-black

             res1 = cv2.bitwise_and(img,img, mask = mask2)
             
Red part in vdo-black,rest shows realtime video images

             res2 = cv2.bitwise_and(background,background, mask = mask1)

Red part in vdo shows real video,rest=black /oppo to res1
blending res1&2

             final = cv2.addWeighted(res1, 1, res2, 1, 0)
             cv2.imshow("final",final)
             key = cv2.waitKey(1)
             if key == ord('q'):
                 break


