## 6-PACK - Wang 2019
> **6-PACK: Category-level 6D Pose Tracker with Anchor-Based Keypoints**
<br> Chen Wang2, Roberto Mart ́ın-Mart ́ın1, Danfei Xu1, Jun Lv2, Cewu Lu2, Li Fei-Fei1, Silvio Savarese1, Yuke Zhu1,3
<br> 1 Department of Computer Science, Stanford University, USA  
2 Department of Computer Science, Shanghai Jiao Tong University, China  
3 NVIDIA Research, USA  
https://sites.google.com/view/6packtracking.

### Abstract
the NOCS **category-level** 6D pose estimation benchmark

### I. INTRODUCTION
+ manipulation and navigation
+ instance-level 6D tracking
+ category-level 6D tracking
<br> reduce category-level 6D tracking to a 3D detection and 6D pose estimation problem.
+ unsupervised learning approach
+ runs at 10Hz on a GTX1070 GPU

Fig. 1
+ estimates object pose by accumulating relative pose changes over time
+ detect and track a set of 3D category-based keypoints (yellow dots) based on **anchors** (red dots)
+ compute the 6D object pose change via least-squares optimization

### II. RELATED WORK
### III. PROBLEM DEFINITION
+ This setup was defined in NOCS
+ Following [41], we define 3D keypoints that are geometrically and semantically consistent throughout a temporal sequence
### IV. MODEL
+ Each anchor(a coarse centroid of the object) summarizes the volume around it with a distanceweighted sum of the individual features of the RGB-D points in its surrounding
+ both symmetric and non-symmetric categories

Initialization
+ We generate a set of keypoints and refine the given pose to be at the centroid of the set
+ centroid of the keypoints is close to the instance centroid

A. Anchor-based Attention Mechanism
+ Each anchor contains a feature representation of the surrounding volume around it
+ coarse attention-based anchor selection
+ fine-grained keypoint generation  
as offset points from the selected anchor
+ combine color and geometric information into a fused feature
+ DenseFusion [45] feature embedding
+ detect the one closest to the object centroid

