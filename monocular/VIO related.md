## 1. MSCKF
[MSCKF那些事（一）MSCKF算法简介](https://zhuanlan.zhihu.com/p/76341809)

VIO相对于VO的好处：
1. **恢复尺度**：IMU能够提供尺度信息(加速度计)，解决单目VO的无法恢复尺度的问题。
2. **应对纯旋转**：在纯旋转的情况下，VO位姿解算会出现奇异，VIO可以利用IMU的陀螺仪(角速度计)来估计纯旋转运动。
3. **应对短时间图像特征缺失**：在出现图像过曝、图像过暗、环境纹理不足时，VO会无法工作，而VIO能在VO失效的情况下利用IMU积分来进行运动估计，能够应对短时间的视觉特征缺失（IMU积分越久累积误差会越大），比VO具有更高的鲁棒性。
4. **精度更高**：VIO融合了两种传感器的信息，位姿估计的精度要更高。

### 3. MSCKF核心思想
MSCKF的目标是解决EKF-SLAM的维数爆炸问题。  
MSCKF不是将特征点加入到状态向量，而是将不同时刻的相机位姿(位置p和姿态四元数q)加入到状态向量  
历史的相机状态会不断移除，只维持固定个数的的相机位姿（Sliding Window）

### 4.MSCKF算法步骤
![](/home/yanhan/Projects/SLAMproject/visual-slam-roadmap/monocular/img/msckf1.jpg)
图中X表示状态向量，P表示对应的协方差矩阵，红色表示当前步骤发生改变的量。
1. 首先初始化状态向量和协方差
2. 然后进行IMU积分，状态向量和协方差都发生改变
3. 接着将新的相机状态加入到状态向量中，扩充协方差矩阵（新相机自身的协方差以及对X<sub>IMU</sub>的协方差）
4. 进行观测更新，所有状态和协方差都会发生改变。（注意：第一次因为只有一个相机状态，形成不了重投影约束，所以第一次观测更新并不会做任何事情）
5. 当相机状态个数超过限制时，删除最历史的一个相机状态及其对应的协方差项。
6. 重复2-5。

***
代码：  
[源码](https://github.com/KumarRobotics/msckf_vio)  
[单目](https://github.com/UMiNS/MSCKF_VIO_MONO)  
[python版本](https://github.com/rohiitb/msckf_vio_python)