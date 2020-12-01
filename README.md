# FriendBlend
FriendBlend is an application that merges two portraits of different people to create a single, multi-person photo. The 
protraits should have same background. The application is very useful when person A takes photo of person B, person B
takes photo of person A with the same background. The application produces a blended image having both persons in the 
image. </br>
The application is implemented in C++ and the implementations depends on OpenCV liibrary.

## Run
Put the images int .. </br>
To run the application, type `make test_friendBlend`. </br>
These two images were blended together using the application to produce the following blended image.

## Working of the Application
There are four stpes involved behind the working algorithm of the application:
###  1. Color Correction
Before the blending process, the lighting from the two input images should be approximately the same otherwise
the blended image could have some artifacts. We perform histogram equalization on both the images for the color 
correction.
    
### 2. Face and Body Detection
We have two image merging process : Alpha blending and Grabcut. Depending on the location of the humans, one of
the image merging or blending chosen. To find the location of the human, we detect the face and get the bounding
box using Haar Cascade models in OpenCV. From the face bounding box, we estimate the bounding box for the human body.
    
### 3. Images alignment
The images need to be aligned before we merge them. We compute a homography to warp one of the images so that the perspectives from the two photos are the same prior to merging.

####  a. Keypoint Detection
Our application uses Oriented FAST and Rotated BRIEF (ORB) detector for keypoint detection in the images. Further,
we also prune the keypoints inside the bounding boxe of the humans.

####  b. Keypoint Matching
Keypoints in the two images are matched by Hamming distance using Brute Force Matcher. 

####  c. Computing the Homography
After we have found a good set of keypoint matches, homography is find using RANSAC based method.

### 4. Image Blending
FriendBlend uses two techniques to merge the images depending on whether the persons are close or far apart.

#### a. Persons Far Apart : 
We blend the region between the bounding box of the persons using Alpha blending. The left region
from the left person image and the right region from the right persion image is copied directly into the 
final blended image.
    
#### b. Persons Close Together
When the persons are close, the person which appears in front is cropped in the foreground using GrabCut. This cropped
pixels of the person is put on the top of the other image to get the final blended image.
