---
layout: post
title: ROS2 dashing 버전 설치 및 셋팅(ubuntu 18.04)
category: ROS2
tag: [ROS2]
---

## Ubuntu 18.04에 ROS2 dashing 버전 설치 및 셋팅하기

<br>

# ROS2 설치

ROS2 dashing 버전은 https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Install-Debians/#id7 을 참고해서 설치한다.<br>
(현재 19.04에는 설치 불가 / eloquent 등 다른 버전 설치 방법도 있음)

Setup Locale과 Install Additional*은 생략 가능

<pre class="prettyprint">
sudo apt update && sudo apt install curl gnupg2 lsb-release
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo sh -c 'echo "deb [arch=amd64,arm64] http://packages.ros.org/ros2/ubuntu `lsb_release -cs` main" > /etc/apt/sources.list.d/ros2-latest.list'
</pre>

<pre class="prettyprint">
sudo apt update
sudo apt install ros-dashing-desktop
sudo apt install ros-dashing-ros-base
</pre>

<br>

rosdep 설치 및 초기화
<pre class="prettyprint">
sudo apt update
sudo apt install -y python-rosdep
sudo rosdep init
rosdep update
</pre>

<br>

python3 라이브러리 설치
<pre class="prettyprint">
sudo apt install -y libpython3-dev
</pre>

<br>




터미널을 새로 열 때마다 실행 필요 (따로 script를 만들어도 좋다.)<br>
ROS_DOMAIN_ID를 설정하면 같은 망에 있어도 분리가 가능하다.

<pre class="prettyprint">
source /opt/ros/dashing/local_setup.zsh
</pre>

<details>
  <summary>ros2_script.zsh</summary>

  <pre class="prettyprint">
  source /opt/ros/dashing/local_setup.zsh
  export ROS_DOMAIN_ID=33</pre>

</details>

<br>


자동 완성 기능
<pre class="prettyprint">
sudo apt install python3-argcomplete
</pre>

<br>

간단한 테스트 (서로 다른 터미널에서 실행)

<pre class="prettyprint">
ros2 run demo_nodes_cpp talker
</pre>


<pre class="prettyprint">
ros2 run demo_nodes_py listener
</pre>

<br>
<br>
<br>

# IDE에서 실행하기

install/local_setup.zsh를 source한 후, 해당 터미널에서 IDE를 실행하면 IDE에서 빌드 및 실행이 된다.<br>
(환경변수가 설정된 IDE만(snap으로 설치 등))<br>
<pre class="prettyprint">
source install/local_setup.zsh
clion
</pre>

<br>
<br>
<br>

---
참고한 사이트 및 서적 : https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Install-Debians/#id7