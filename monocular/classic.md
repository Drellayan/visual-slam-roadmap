### 1. Visual odometry - Nister 2004
> **Visual Odometry** <br>
David Nist ́er, Oleg Naroditsky, James Bergen <br>
Sarnoff Corporation <br>
CN5300, Princeton NJ 08530 USA

>Nister等人首次提出术语“Visual Odometry”,[88] 因为它和车轮里程计的概念相似。他们提出了在单目和双目立体系统中通过视觉输入获取相机的运动的开创性方法。他们专注于在存在异常值（假特征匹配）的情况下估计相机运动的问题，并且提出了使用**RANSAC**[44]的异常值的去除方案。也是第一个在**所有帧**中跟踪特征而不是在连续帧中匹配特征的方案。这具有避免在基于互相关的跟踪期间的特征漂移的益处[101]。他们还提出了使用**3D到2D重投影误差的基于RANSAC的运动估计**（参见“运动估计”部分），而不是使用3D点之间的欧几里德距离误差。与3D到3D误差相比，使用3D到2D重新投影误差可以提供更好的估计[56]。

#### Abstract
+ a stereo head or a single moving camera based on video input. 
视频端作输入的单目或立体相机 <br>
输入要求（如视频分辨率码率）720 × 240 resolution/13Hz
+ The front end of the system is a feature tracker. <br>
  Point features are matched between pairs of frames and linked into image trajectories at video rate.
+ <font color=red>？</font> a geometric hypothesize-and-test architecture <br>
运动估计 visual odometry, i.e. motion estimates from visual input alone
+ GPS, inertia sensors, wheel encoders, etc 融合
+ an autonomous ground vehicle

#### 1. Introduction
+ real-time vision processing
+ All of the processing runs at video rates on a 1GHz Pentium III class machine.

#### 2. Feature Detection
+ Harris corners

#### 3. Feature Matching
+ A feature in one image is matched to every feature within a fixed distance from it in the next image.
+ uniform weighting
+ mutual consistency check <br>
  highest normalized correlation

#### 4. Robust Estimation
+ error accumulation and propagation
+ <font color=red>？</font> chooses which frames to use for relative orientation

4.3 <font color=red>？</font> Preemptive RANSAC and Scoring
+ a preemptive scoring scheme
+ We assume Cauchy distribution for the reprojection errors
+ For scoring the three-view relative pose estimates, we use a trifocal Sampson error

#### 5. Results
+ ground truth <br>
with a Differential Global Positioning System (DGPS) as well as a high precision Inertial Navigation System (INS)
+ a horizontal field of view of 50◦, and image fields of 720 × 240 resolution
<br>the visual odometry's frame processing rate was limited to around 13Hz
>![](/monocular/img/VO5.png "Figure5")
<br> Figure 5: Vehicle positions estimated with visual odometry (left) and DGPS (right). These plots show that the vehicle path is accurately recovered by visual odometry during tight cornering as well as extended operation. In this example the vehicle completes three tight laps of diameter about 20 meters (travelling 184 meters total) and returns to the same location. The error in distance between the endpoints of the trip is only 4.1 meters.

>![](/monocular/img/VO6.png)
<br> Figure 6: Visual odometry vehicle position (light red) superimposed on DGPS output (dark blue). No a priori knowledge of the motion was used to produce the visual odometry. A completely general 3D trajectory was estimated in all our experiments. In particular, we did not explicitly force the trajectory to stay upright or within a certain height of the ground plane. The fact that it did anyway is a strong verification of the robustness and accuracy of the result.

>![](/monocular/img/VO7.png)
<br> Figure 7: Yaw angle in degrees from INS and visual odometry. The correspondence is readily apparent. In most cases, visual odometry yields subdegree accuracy in vehicle heading recovery. The accumulated yaw angle is shown, except for on the bottom right, were the frame to frame yaw angle discrepancy is shown.

#### 6. Summary and Conclusions
+ feature tracking and robust estimation

***
### 2. MSCKF - Mourikis 2007
> **A Multi-State Constraint Kalman Filter for Vision-aided Inertial Navigation**
<br> Anastasios I. Mourikis and Stergios I. Roumeliotis
<br> The authors are with the Dept. of Computer Science & Engineering, University of Minnesota, Minneapolis, MN 55455. Emails:{mourikis|stergios}@cs.umn.edu

#### Abstract
+ an Extended Kalman Filter (EKF)-based algorithm for real-time vision-aided inertial navigation
<br> VIO based on EKF
+ <font color=red>？</font> This measurement model does not require including the 3D feature position in the state vector of the EKF
+ computational complexity only linear in the number of features

#### I. INTRODUCTION
+ MEMS-based inertial sensors
<br> 将IMU技术普及
+ where GPS signals are unreliable
<br> 像室内这种有遮挡的地方
+ images are high-dimensional measurements, with rich information content
+ geometric constraints
<br> <font color=red>？</font> expresses these constraints without including the 3D feature position in the filter state vector
<br> 这种不包含三维特征位置的方法使时间复杂度达到了线性
#### II. RELATED WORK
+ estimate the pose of the camera only (i.e., do not jointly estimate the feature positions)
<br> 提高速度
#### III. ESTIMATOR DESCRIPTION
**B. Propagation**  
IMU状态的更新
#### IV. EXPERIMENTAL RESULTS

#### V. CONCLUSIONS

#### APPENDIX

***
### 3. PTAM - Klein & Murray 2007
> **Parallel Tracking and Mapping for Small AR Workspaces**  
> Georg Klein, David Murray  
> Active Vision Laboratory Department of Engineering Science University of Oxford  
> code: https://www.robots.ox.ac.uk/~gk/PTAM/
+ PTAM是第一个使用非线性优化，而不是使用传统的滤波器作为后端的方案。它引入了关键帧机制：我们不必精细的处理每一幅图像，而是把几幅关键的图像串起来，然后优化其轨迹和地图。
+ 存在的明显缺陷是：场景小，跟踪容易丢失。
#### ABSTRACT
+ split tracking and mapping on a dual-core computer  
  可以对非实时的mapping部分使用computationally expensive batch optimisation
+ produces detailed maps with thousands of landmarks which can be tracked at frame-rate
#### 1 INTRODUCTION
+ a comprehensive map is often not available
+ *extensible trac* in which the system attempts to add previously unknown scene elements to its initial map
+ a *remote expert* who can annotate the generated map
+ we treat the generated map as a sandbox in which virtual simulations can be created.  
  we estimate a dominant plane (a virtual ground plane) from the mapped points
#### 2 METHOD OVERVIEW IN THE CONTEXT OF SLAM
* Tracking and Mapping are separated, and run in two parallel threads.
* Mapping is based on keyframes, which are processed using batch techniques (Bundle Adjustment).
* The map is densely intialised from a stereo pair (5-Point Algorithm)
* New points are initialised with an epipolar search.
* Large numbers (thousands) of points are mapped.
#### 3 FURTHER RELATED WORK
#### 4 THE MAP
#### 5 TRACKING
At every frame, the system performs the following two-stage tracking procedure: 
1. A new frame is acquired from the camera, and a prior pose estimate is generated from a motion model. 
2. Map points are projected into the image according to the frame's prior pose estimate. 
3. A small number (50) of the coarsest-scale features are searched for in the image. 
4. The camera pose is updated from these coarse matches. 
5. A larger number (1000) of points is re-projected and searched for in the image. 
6. A final pose estimate for the frame is computed from all the matches found.

+ a Unibrain Fire-i video camera equipped with a 2.1mm wide-angle lens  
delivers 640×480 pixel YUV411 frames at 30Hz
+ a four-level image pyramid  
  the FAST-10 [23] corner detector  
  without non-maximal suppression
+ ? a decaying velocity model

#### 6 MAPPING
#### 7 RESULTS
#### 8 LIMITATIONS AND FUTURE WORK
#### 9 CONCLUSION

