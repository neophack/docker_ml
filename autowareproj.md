# 源码

https://github.com/tier4/AutowareArchitectureProposal.proj

# 下载数据 可以挂载或者sftp传入docker 

https://gitee.com/neophack/autowareprojdata/releases/1

解压到/workspace/datasets
```
├── metadata.yaml
├── planning_simulator_sample_map
│   ├── lanelet2_map.osm
│   └── pointcloud_map.pcd
├── rosbag_sample_map
│   ├── lanelet2_map.osm
│   └── pointcloud_map.pcd
└── sample.625-2.bag2_0.db3
```

# 浏览器打开，打开右上角工具vnc

# 打开workspace中的autoware.proj

# 编译autoware.proj

$ `source /opt/ros/galactic/setup.zsh`

$ `source ~/.bashrc`

$ `colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release`

# 运行 rosbag demo

$ `cd /workspace/autoware.proj/`

$ `source install/setup.zsh`

$ `ros2 bag play /home/ml/rosbag2/sample/rosbag_sample_map/sample.625-2.bag2_0.db3 -r 0.2`

$ `ros2 launch autoware_launch logging_simulator.launch.xml map_path:=/workspace/datasets/rosbag_sample_map/ vehicle_model:=lexus sensor_model:=aip_xx1`

# 运行 planning demo

$ `cd /workspace/autoware.proj/`

$ `source install/setup.zsh`

$ `ros2 launch autoware_launch planning_simulator.launch.xml map_path:=/workspace/datasets/planning_simulator_sample_map/ vehicle_model:=lexus sensor_model:=aip_xx1`

# 运行 rviz2 

$ `source /workspace/autoware.proj/install/setup.zsh`

$ `rviz2` 

打开 /home/ml/workspace/AutowareArchitectureProposal.proj/src/autoware/launcher/autoware_launch/rviz/autoware.rviz

