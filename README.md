# Drone-Obstacle-Avoidance-ROS
### Prerequisite
Installation of ROS and Gazebo
### Installation 
Install `mavros` and `mavlink` from source:
```
cd ~/catkin_ws
wstool init ~/catkin_ws/src

rosinstall_generator --upstream mavros | tee /tmp/mavros.rosinstall
rosinstall_generator mavlink | tee -a /tmp/mavros.rosinstall
wstool merge -t src /tmp/mavros.rosinstall
wstool update -t src
rosdep install --from-paths src --ignore-src --rosdistro `echo $ROS_DISTRO` -y

catkin build
```
Add a line to end of `~/.bashrc` by running the following command:
```
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
```

update global variables
```
source ~/.bashrc
```

install geographiclib dependancy 
```
sudo ~/catkin_ws/src/mavros/mavros/scripts/install_geographiclib_datasets.sh
```
Install `ardupilot` : 
```
cd ~
git clone https://github.com/khancyr/ardupilot_gazebo.git
cd ardupilot_gazebo
```
***Ubuntu 18.04 only*** checkout dev
```
git checkout dev
```
build and install plugin
```
mkdir build
cd build
cmake ..
make -j4
sudo make install
```
## Obstacle Avoidance
### Artificial Potential Field :
In order to ensure the efficient passage and safety of the inspection UAV in the complex environment,
the traditional spherical repulsive field of the artificial potential field method is improved into an
ellipsoidal repulsive field, where the UAV velocity direction and the long semi-axis are always co-linear,
and the ellipsoidal size is related to the magnitude of the components of the UAV velocity in the X-axis
and Y-axis, and the Z-axis component is equal to the Y-axis component. As shown in Figure 2, the drone
is located in the center of the ellipsoid, and the magnitude and direction of its velocity are represented
by the vector, the positions of the obstacles are A and B, and the magnitude and direction of their
velocities are represented by the vector and respectively. There exists a virtual ellipsoidal repulsive field
around the UAV determined by the magnitude of its own motion velocity, and the obstacles located at
the edges of the ellipsoidal potential field have different distances to the UAV. Therefore, the improved
ellipsoidal repulsive field can give a larger repulsive influence to the obstacles located at high collision
possibilities, and the improved repulsive force also decreases with increasing distance as the
conventional repulsive force. 
![Screenshot from 2022-05-22 23-15-07](https://user-images.githubusercontent.com/77221967/169708679-fcb37364-2173-401c-bb72-da15bc5de55b.png)
### Simulation 
The simulation environment is set as a 100m*100m*100m area with six obstacles randomly distributed
in the space, and the relevant parameters set in this paper are shown in Table 2 according to the
discussion of the parameters of the artificial potential field method in the literature [7]. The simulation
results are shown in Fig. (a) shows the traditional artificial potential field method, and Fig. (b)
shows the improved artificial potential field method.
###
![Screenshot from 2022-05-22 23-33-04](https://user-images.githubusercontent.com/77221967/169709314-c1135970-928a-4b63-88c4-cdf37c16dee8.png)

