## 1. Why filter? - Strasdat 2012
## 2. ORB-SLAM - Mur-Artal 2015
> **ORB-SLAM: a Versatile and Accurate Monocular SLAM System**
<br> Ra ́ul Mur-Artal*, J. M. M. Montiel, Member, IEEE, and Juan D. Tard ́os, Member, IEEE,
<br> Universidad de Zaragoza

### Abstract
+ full automatic initialization
+ tracking, mapping, relocalization, and loop closing

### I. INTRODUCTION
+ a subset of selected frames (keyframes)
+ with significant parallax and with plenty of loop closure matches

以PTAM为main ideas，
place recognition来自*Bags of Binary Words for Fast Place Recognition in Image Sequences*，
scale-aware loop closing来自*Scale Drift-Aware Large Scale Monocular SLAM*，
large scale operation来自*Double window optimisation for constant time visual SLAM*和*Closing loops without places*

主要贡献：
+ same ORB features for all tasks
+ a covisibility graph可以在局部区域追踪建图，使整个系统可以在更大的环境下实时运行
+ loop closing based on the optimization of a pose graph that we call the Essential Graph来自a spanning tree
+ relocalization
+ initialization
+ map point and keyframe selection

## 3. Pop-up SLAM - Yang 2016
## 4. PL-SLAM - Pumarola 2017
## 5. ORB-SLAM2 - Mur-Artal 2017
## 6. CubeSLAM - Yang 2018
## 7. OpenVSLAM - Sumikura 2019
## 8. DeepFusion - LaidLow 2019
## 9. UcoSLAM - Munoz-Salinas 2019
## 10. ORB-SLAM3 - Campos 2020
## 11. DXSLAM - Li 2020