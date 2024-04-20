"pcl_lab" is a utility exploiting PCL, created by rokugatsu as a product of his study. It consists of several funtions.
It has been sured run on the ROS2 humble on ubuntu 22.04.

# cloudAligner
It is a point cloud map creation tool allow user check the process and modify it by manual in case misalignment happened or required partial update.

##### Point cloud map: Tsukuba Center, Tsukuba Ibaraki pref. Japan
<img src="https://github.com/rokugatsu/pcl_lab/assets/120123933/73afaace-516a-4c40-9897-8eaa0c86d2a8" width="640">

## Overview
It creates a point cloud map by combining several sub-point cloud maps.  
<img src="https://github.com/rokugatsu/pcl_lab/assets/120123933/5aea3845-3f80-47cb-b43c-e94f5067d261" width="640">
[CloudAlignOnepage.pdf](https://github.com/rokugatsu/pcl_lab/files/15049263/CloudAlignOnepage.pdf)

## How to use
### Download from github and build
```
cd YOUR_WORK_SPACE
git clone https://github.com/rokugatsu/pcl_lab.git
colcon build --symlink-install --packages-up-to pcl_lab
source ./install/setup.bash 
```
### Set up
Modify yaml to fit your environment.

#### pcl_lab/config/pcl_lab.param.yaml
|ros__parameters|Default Value|Note|
| ------------- | ------------- |---|
|use_sim_time  |true  |---|
|input_pointcloud  | "/velodyne_points"   |Topic "PointCloud2" of source point cloud|
|input_odom| "/odom_raw"|Odometry topic. No use at the current version|
|threshold_align_fitness_score|1.0|It affects accuracy of measuring track based on LIDAR.In case track is far from real, set smaller value to improve it.|
|save_pcd_dir|"SET_YOUR_DATA_DIRECTORY"|Directory to store map_materials, submaps, and point cloud as outcome.|  
|threshold_dist_save_pcd|10.0|Interval distance to save source point cloud as map_material_xx_yyyy.pcd.|
|Submap_Range|60.0|Interval distance to create submap_nn.pcd as submap_xx.pcd.|
|SumAlign_fitnessScore|500.0|It affects accuracy of configuring submap. In case misalignment occured,Set smaller value.|
|leaf_size|0.5|Voxel size of source point cloud.|
### Run
```
ros2 launch pcl_lab cloudAligner.launch.xml
```
In another terminal, replay bag file contains point cloud
```
ros2 bag play -r 1 YOUR_BAG_FILE
```
#### 1. Check the track if not far from the real 
The track affects all of the process. In case far from the real, modify the value of yaml file, then try again.
<img src="https://github.com/rokugatsu/pcl_lab/assets/120123933/39e364c8-29b9-45ce-8fff-c058085c4a7b" width="640">

#### 2. Point cloud map creation starts just after bag file replay finished or suspended.  
Eventually, Pointcloud map has appeared. You can also find bi-products of this process including map-materials, submaps and partial point cloud maps. 
Please exploit them to your debug.

<img src="https://github.com/rokugatsu/pcl_lab/assets/120123933/1ede48eb-fe53-445a-aeef-fec075d852f2" width="640">

