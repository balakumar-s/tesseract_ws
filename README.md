# Tesseract ROS Workspace

Tesseract motion planning library uploaded as used to benchmark in paper: CuRobo (ICRA 2023).

## Install instructions

Install ros noetic:

```
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu focal main" > /etc/apt/sources.list.d/ros-latest.list' \
&& apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 \
&& apt-get update && apt-get install -y \
  ros-noetic-desktop-full git build-essential python3-rosdep \
  && rm -rf /var/lib/apt/lists/*
```

Then follow the below instructions:

### For x86 machines

```
sudo apt install ros-noetic-ifopt lcov libbullet-extras-dev ros-noetic-ompl
sudo add-apt-repository ppa:ros-industrial/ppa
sudo apt update && sudo apt install taskflow 
```

### For arm64 machines

```
sudo apt install ros-noetic-ifopt lcov libbullet-extras-dev ros-noetic-ompl
git clone https://github.com/taskflow/taskflow.git && cd taskflow && mkdir build && cd build && cmake .. -D TF_BUILD_EXAMPLES=OFF -D TF_BUILD_TESTS=OFF && make install
sudo apt install libyaml-cpp-dev ros-noetic-octomap libassimp-dev liborocos-kdl-dev libbullet-dev ros-noetic-fcl libpcl-dev swig
```

If you get mno-avx error during compilation on arm:

comment out lines that have "-mno-avx" in file: trajopt_utils/cmake/trajopt_macros.cmake and also in descartes_light/descartes_light/cmake/descartes_light_macros.cmake

### Compile this catkin workspace

```
sudo apt install python3-catkin-tools
```

clone this repository into a catkin workspace and run

```
catkin config --cmake-args -DCMAKE_BUILD_TYPE=Release
catkin build
```

## Running benchmark from robometrics

To run benchmark, follow instructions at <https://github.com/balakumar-s/tesseract_wrapper>

## License

This package is a clone of many packages from <https://github.com/tesseract-robotics> . Please look at
the repos at <https://github.com/tesseract-robotics> for the licences.


## Commit tags

| Package | tag/commit | Modified?|
|---------|------------|----------|
|descartes_light| 0.3.0 |No|
|ifopt| 2.1.2|No|
|opw_kinematics| 0.4.4|No|
|ros_industrial_cmake_boilerplate| 0.2.14|No|
|tesseract| 3a14e2a071|No|
|tesseract_planning| 1627231f3d|added bitstar to ompl, updated trajopt profile to optimize for zero velocity at end|
|tesseract_python| 8d9db169c63c2b238c6fe8ea06042983cdc51a4a| No|
|trajopt| 0.2.0|disable bpmd |

We removed mesh files and test data files in the packages to reduce overall footprint.