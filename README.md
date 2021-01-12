### ROS Industrial Training

This repository contains practice code of [ros industrial training melodic branch](https://industrial-training-master.readthedocs.io/en/melodic/).

[![ROS CI Action Status](https://github.com/Yeshasvitvs/ros-industrial-training/workflows/ROS%20CI/badge.svg)](https://github.com/Yeshasvitvs/ros-industrial-training/actions?query=workflow%3A%22ROS+CI%22)

##### Observations

- Following [ROS-Setup](https://industrial-training-master.readthedocs.io/en/melodic/_source/session1/ROS-Setup.html), it is quite handy to go through [Wiki page of ROS Qt Creator plug-in](https://ros-qtc-plugin.readthedocs.io/en/latest/index.html). This will help new developers to get [Qt configured](https://ros-qtc-plugin.readthedocs.io/en/latest/_source/Import-ROS-Workspace.html) with ROS Workspace, and handle the development of ROS projects from QtCreator.

- Concerning Continuous Integration (CI), there is an infrastructure [industrial_ci](https://github.com/ros-industrial/industrial_ci) from ros industrial. However, given the active development on [ros-tooling](https://github.com/ros-tooling), I decided to use the Github Actions based CI from ros-tooling. This is updated at [ros-ci.yml](.github/workflows/ros-ci.yml).


- [ros industrial training melodic branch](https://industrial-training-master.readthedocs.io/en/melodic/) is based on [catkin build system](http://wiki.ros.org/catkin/Tutorials). However, the Continuous Integration (CI) based on [ros-tooling](https://github.com/ros-tooling) with [action-ros-ci](https://github.com/ros-tooling/action-ros-ci) considers [colcon build system](https://index.ros.org/doc/ros2/Tutorials/Colcon-Tutorial/). Colcon build system needs ros packages to be installed in order to discover ros packages and use ros commands as pointed in [this reference from aws](https://docs.aws.amazon.com/robomaker/latest/dg/troubleshooting-colcon.html). Accordingly, the installation of `myworkcell_core` and `myworkcell_support` ros packages are updated in [this commit](https://github.com/Yeshasvitvs/ros-industrial-training/commit/51dd5e232841e66f0653e7e41bb5ee7591459a55).

##### Documentation Missing (Minor) Details

- Under [Download and Build a Package from Source](https://industrial-training-master.readthedocs.io/en/melodic/_source/session1/Installing-Existing-Packages.html#download-and-build-a-package-from-source), a ros package [fake_ar_publisher](https://github.com/ros-industrial/fake_ar_publisher) is suggested. This is a necessary package to work through the rest of the training. However, for the ros industrial training based on ros melodic, you need to clone to [master branch of fake_ar_publisher](https://github.com/ros-industrial/fake_ar_publisher/tree/master)

  `git clone -b master https://github.com/jmeyer1292/fake_ar_publisher.git`

- Under [Coordinate Tranforms using TF](https://industrial-training-master.readthedocs.io/en/melodic/_source/session3/Coordinate-Transforms-using-TF.html#scan-n-plan-application-guidance) excercise, you need to include `tf2_geometry_msgs/tf2_geometry_msgs.h` header file or else the transform function throws error.

- Under [Using MoveIt! with Physical Hardware](https://industrial-training-master.readthedocs.io/en/melodic/_source/session3/Build-a-Moveit!-Package.html#using-moveit-with-physical-hardware), while creating `myworkcell_planning_execution.launch` file, you may need to change the `arg` name from `config` to `rviz_config`. This is because the autogenerated `moveit_rviz.launch` file is created with `rviz_config` as the argument name.

  ```
    <include file="$(find myworkcell_moveit_config)/launch/moveit_rviz.launch">
      <arg name="config" value="true"/>
    </include>
    ```

    to

    ```
    <include file="$(find myworkcell_moveit_config)/launch/moveit_rviz.launch">
    <arg name="rviz_config" value="true"/>
    </include>
    ```

- Under [Launch the Planning Environment](https://industrial-training-master.readthedocs.io/en/melodic/_source/session3/Motion-Planning-RVIZ.html#launch-the-planning-environment), running `roslaunch myworkcell_moveit_config myworkcell_planning_execution.launch` file launches rviz without motion planning. The user need to add the motion planning class manually. Created and added a new rviz configuration file to be launched with motion planning in [this commit](https://github.com/Yeshasvitvs/ros-industrial-training/commit/9f1c491c476c4e21c5a0653885eeb891c335cf13)

- Under more to explore point of [Scan-N-Plan Application: Guidance](https://industrial-training-master.readthedocs.io/en/melodic/_source/session4/Motion-Planning-CPP.html#scan-n-plan-application-guidance), you will need to use move group `getCurrentPose()` api call. This may thrown an error if the `asynchronos spinner` is started after the call to  `start()` of ScanNPlan application ([Reference Issue](https://github.com/ros-planning/moveit/issues/1187)). Check [this commit](https://github.com/Yeshasvitvs/ros-industrial-training/commit/311d296274b95d611c4130ed3f0a4c8dafeda276)

- Under [Application Demo 1 - Perception-Driven Manipulation](https://industrial-training-master.readthedocs.io/en/melodic/_source/demo1/Bring-up-ROS-system-in-simulation-mode.html#start-in-simulation-mode), you need to add the following planning node to `ur5_setup.launch` file before running rviz node. Comment this node from `ur5_pick_and_place.launch` file. 

  ```
  <include file="$(find ur5_collision_avoidance_moveit_config)/launch/move_group.launch">
    <arg name="publish_monitored_planning_scene" value="true" />
  </include>
  ```
