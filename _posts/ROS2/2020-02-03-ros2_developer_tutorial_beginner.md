---
layout: post
title: ROS2 Developer Tutorial
category: ROS2
tag: [ROS2]
---

## ROS2 Developer Tutorial 내용을 정리한 글 입니다.

<br>

# Prerequisites : Colcon build

colcon 설치
<pre class="prettyprint">
sudo apt install python3-colcon-common-extensions
</pre>

<br>

ros workspace는 주로 src subdirectory를 갖고, colcon을 통해 build할 수 있다.<br>
빌드 후에는 보통 세개의 디렉토리가 나온다.<br>

- build : 중간 산출물
- install : 각 package들이 install됨
- log : colcon 빌드 때 나오는 로그들

<br>
<br>
<br>

# Workspace 생성하기

폴더 생성 및 예제코드 git clone
 <pre class="prettyprint">
cd ros2_ws/tutorial
mkdir src
git clone https://github.com/ros/ros_tutorials.git -b dashing-devel
</pre>

<br>

dependencies resolve<br>
(이미 되어있다면 #All required rosdeps installed successfully이 출력됨)

<pre class="prettyprint">
sudo rosdep install -i --from-path "directory_name" --rosdistro dashing -y
</pre>

<br>
package dependency는 package.xml 파일에 저장됨.
<br>

colcon build (ros2 source해야 진행됨)
<pre class="prettyprint">
colcon build
또는 colcon build --symlink-install(rebuild 시간 절약)
</pre>

<br>

새 ws를 source할 때에는 새 터미널에서 하는게 좋다.
<pre class="prettyprint">
source /opt/ros/dashing/setup.bash
cd ros2_ws/tutorial/src/install
source local_setup.zsh
</pre>

<br>

동작 확인
<pre class="prettyprint">
ros2 run turtlesim turtlesim_node
</pre>

<br>
<br>
<br>

# Package

- package == container for ROS2 code
- package 시스템 = ament / 빌드 툴 = colcon (ament로 package를 생성하고, colcon으로 빌드한다.)
- package 최소 구성 - package.xml 
(package의 meta info), CMakeList.txt
- pkg 만들기 (pkg명은 소문자로 / --node-name으로 노드 이름 지정가능)
<pre class="prettyprint">
ros2 pkg create --build-type ament_cmake "package_name"
</pre>

<br>

예제
<pre class="prettyprint">
cd ros2_ws
ros2 pkg create --build-type ament_cmake --node-name my_node my_package
cd my_package
colcon build
cd install
source local_setup.zsh
ros2 run my_package rmy_node
</pre>

<br>
<br>
<br>

# Simple publisher & subscriber

예제 코드를 활용한 간단한 실습 (https://index.ros.org/doc/ros2/Tutorials/Writing-A-Simple-Cpp-Publisher-And-Subscriber/#cpppubsub)
<pre class="prettyprint">
cd ros2_ws
ros2 pkg create --build-type ament_cmake cpp_pubsub
cd cpp_pubsub/src
wget -O publisher_member_function.cpp https://raw.githubusercontent.com/ros2/examples/master/rclcpp/minimal_publisher/member_function.cpp
wget -O subscriber_member_function.cpp https://raw.githubusercontent.com/ros2/examples/master/rclcpp/minimal_subscriber/member_function.cpp
</pre>

<br>

cpp의 include 의존성있는 패키지를 package.xml과 CMakeList.txt에 추가한다.<br>

- package.xml
<pre class="prettyprint">
&lt;exec_depend&gt;rclcpp&lt;/exec_depend&gt;
&lt;exec_depend&gt;std_msgs&lt;/exec_depend&gt;
</pre>

<br>

- CMakeLists.txt
<pre class="prettyprint">
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)

add_executable(talker src/publisher_member_function.cpp)
ament_target_dependencies(talker rclcpp std_msgs)

add_executable(listener src/subscriber_member_function.cpp)
ament_target_dependencies(listener rclcpp std_msgs)

install(TARGETS
  talker
  listener
  DESTINATION lib/${PROJECT_NAME})
</pre>

<br>

빌드 후 실행
<pre class="prettyprint">
cd ..
colcon build
cd install
source local_setup.zsh
ros2 run cpp_pubsub talker
ros2 run cpp_pubsub listener
</pre>

<br>
<br>
<br>

# Simple service & client

service request & response의 구조는 .srv파일에 의해 정해짐<br>

두 수의 합을 요청하고 답을 받는 예제 프로그램 (https://index.ros.org/doc/ros2/Tutorials/Writing-A-Simple-Cpp-Service-And-Client/#cppsrvcli)<br>
--dependencies를 이용하면 package.xml과 CMakeList.txt에 자동으로 의존성 추가됨

<pre class="prettyprint">
cd ros2_ws
ros2 pkg create --build-type ament_cmake cpp_srvcli --dependencies rclcpp example_interfaces
</pre>

<br>

exampe_interfaces에 srv파일이 미리 정의되어 있음

<pre class="prettyprint">
int64 a
int64 b

int64 sum
</pre>

<br>
사이트를 참고하여 service와 client node를 작성한다.<br>
CMakeLists.txt에 추가

<pre class="prettyprint">
add_executable(server src/add_two_ints_server.cpp)
ament_target_dependencies(server
rclcpp example_interfaces)

add_executable(client src/add_two_ints_client.cpp)
ament_target_dependencies(client
  rclcpp example_interfaces)

install(TARGETS
  server
  client
  DESTINATION lib/${PROJECT_NAME})
</pre>

<br>

테스트
<pre class="prettyprint">
cd cpp_srvcli
colcon build
cd install
source local_setup.zsh
ros2 run cpp_srvcli server
ros2 run cpp_srvcli client 2 5
</pre>

<br>
<br>
<br>

---
참고한 사이트 및 서적 : https://index.ros.org/doc/ros2/Tutorials/Colcon-Tutorial/ <br>
https://index.ros.org/doc/ros2/Tutorials