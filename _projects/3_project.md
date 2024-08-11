---
layout: page
title: Formation control using vision tracking
description: Using a RGB camera for 2D pose estimation, move a set of robots while adhereing to geometric constraints between them.
img: assets/robotvision/image29.gif
importance: 3
category: work
---

### Team
Raj Surya, Santo Santhosh

### Objective
To move a set of robots in a fixed formation by using external RGB camera for 2D pose estimation of the robots. The pose estimation should allow for real time control of the robot hence must be fast.

### Setup
The development is done on Gazebo, which is a simulation software. We use 3 holonomic robots in a environment with random obstcales of different sizes and colors. The main focus is to develop a perception system that is fast enough to estimate 2D pose of multiple robots at once and the environment along with the robots were used to validate the perception system only.

### Approach
The geometry of the robot with its color and other factors are well known and are constant.Owing to the constrains, we decided to build our own set of kernals to segment the robots in an image instead of using a learning based approach. This also allowed for faster inference from an image compared to a segmentation model.

The input image from the RGB camera is first converted to grey scale. Since the robots are primarily red in color, only the RED channel from the RGB image was processed to obtain the greyscale image. Next a gaussion blur was used to smoothen the image.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/Results/image17.png" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/Results/p6.jpg" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/Results/p3.jpg" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Input Image
    Center: Greyscaled Image
    Right: Image without gaussian blur
</div>
To identify objects from the scene, thresholding was used that elimates the background. Now from the set of objects, only shapes similar to the robots were filtered out using contouring.

Since the camera is at a fixed location, the robots distance from the camera is bounded. Using this range of distance, camera's position and the robots shape, the area maximum and minimum area of the robot as seen in the image was computed. This range of area was then filtered out from the objects refined using contouring earlier.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/Results/p10.jpg" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/Results/p5.jpg" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
 </div>   
<div class="caption">
    Left: Thresholding
    Right: Countoruing
</div>
It was then possible to calculate the center and orientation of the robot precisely. Using projective geometry (as the robots are assumed to be operating on a flat surface, which is a very reasonable assumption for a indoor multi robot system's environment, example warehouse), the coordinates are projected from 2D image space to 3D world coordinates.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/Results/p4.jpg" title="head" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Filtering based on area and center calculation.
</div>

This information is then fed into a PD control plant to compute error between relative distances of the robots and output a correction linear and angular velocity.
Using the velocities, the 

This closed loop control always maintains the formation of the robots.

### Results
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/Results/p9.jpg" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/image27.gif" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
 </div>   
<div class="caption">
    Left: Path of robots in a straight line motion.
    Right: Video of robots moving in a straight line.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/Results/p8.jpg" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/image29.gif" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
 </div>   
<div class="caption">
    Left: Path of robots in a zigzag line motion.
    Right: Video of robots moving in a zigzag line.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/Results/p7.jpg" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/robotvision/image32.gif" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
    
<div class="caption">
    Left: Path of robots in reverse.
    Right: Video of robots moving in reverse.
</div>

### Limitations
- The system is limited by the speed of the robots. If the robots are too fast, the projective geometry can fail as it is built on the assumption that between two frame the distance travelled by the robots are small and comparable to the estimates obtained by extrapolating the input velocities and the current robots poses.
- Sharp turns can cause confusion to the perception system. The previous poses of the robots are stored and sharp turns can cause wrong correlation between the robots current pose and previous pose.
- Occulsions deptrive the perception system to estimate the pose of robots hence causing its failure. Some other means such as IMU data might be used during this time for position calculation.
- The perception system relies on the geometry of the robots, hence objects with similar geometry can confuse the perception system.
- The system is limited by the field of view of the camera (this can be overcome by placing multiple cameras and fusing their data for more accurate estimation) and the FPS of the camera.

Certainly by fusing data from other pose estimation techniques, like IMU, odometry calculation using wheel speeds etc, the system can be robust to these limitations and can perform more accurately.