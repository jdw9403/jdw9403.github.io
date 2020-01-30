---
layout: post
title: ROS2 Tutorial
category: ROS2
tag: [ROS2]
---

## ROS2 Tutorial 내용을 정리한 글 입니다.

<br>

# Node

- Node : 하나의 일을 수행하는 모듈 

- topic, service, action으로 각 노드들은 통신한다.

- 하나의 실행파일(c++, python program..)은 하나 이상의 노드를 가질 수 있다.

- package에서 executable 실행

<pre class="prettyprint">
ros2 run "package_name" "executable_name"
</pre>

<br>

- 현재 실행중인 노드 리스트 출력
<pre class="prettyprint">
ros2 node list
</pre>

<br>

- node, topic등 재정의 (ex)
<pre class="prettyprint">
ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle 
</pre>

<br>

- 노드 정보 출력
<pre class="prettyprint">
ros2 node info "node_name"
</pre>


<br>
<br>

# Topic

- publisher, subscriber가 같은 topic을 가지면 통신 가능(1대다, 다대1, 다대다 모두 가능) - smp의 event처럼 subscribe하면 전부 들을 수 있음

- 활성화된 토픽 리스트 출력 (-t : 토픽 타입도 출력)
<pre class="prettyprint">
ros2 topic list
</pre>

<br>

- 해당 토픽으로 publish되는 메세지들 출력
<pre class="prettyprint">
ros2 topic echo "topic_name"
</pre>

<br>

- 해당 토픽의 publisher, subscriber 수 출력
<pre class="prettyprint">
ros2 topic info "topic_name"
</pre>

<br>

- 해당 토픽으로 직접 pub (pub --once: 한번만 / pub --rate 1 : 1hz로 계속 송신)
<pre class="prettyprint">
ros2 topic pub "topic_name" "msg_type" "args"
</pre>

<br>
<br>

# Service

- service : call-and-response model(vs topics' publisher-subscriber model) / smp의 method call과 동일

- 즉, smp의 service, client == ros의 nodes / smp의 event == ros의 topic publish, subscribe / smp의 method call == ros의 service

- 활성화된 service call 리스트 출력 (-t : 서비스 타입도 출력)
<pre class="prettyprint">
ros2 service list
</pre>

<br>

- 직접 service call (callback도 들어옴)
<pre class="prettyprint">
ros2 service call "service_name" "service_type" "args"
</pre>

<br>

- node, topic, service, action등을 입력할 땐 /을 앞에 붙인다.

- 해당 타입의 arguments들 보여줌 (직접 topic pub하거나 service call할 때 인자 확인용으로 사용)
<pre class="prettyprint">
ros2 interface show "type_name.srv"
</pre>

<br>
<br>

# Parameter

- parameter : node들의 설정값

- 각 노드들의 param들 출력
<pre class="prettyprint">
ros2 param list
</pre>

<br>

- 해당 노드의 param값 출력
<pre class="prettyprint">
ros2 param get "node_name" "parameter_name"
</pre>

<br>

- 해당 노드의 param값 수정(현재 세션에서만)
<pre class="prettyprint">
ros2 param set "node_name" "parameter_name" "value"
</pre>

<br>

- 해당 노드의 현재 param값들을 파일로 저장(현재 workspace에 저장됨)
<pre class="prettyprint">
ros2 param dump "node_name"
</pre>

<br>

- 저장된 파일로 param 설정 후 시작
<pre class="prettyprint">
ros2 run "package_name" "executable_name" --ros-args --params-file "file_name"
</pre>

<br>
<br>

# Action

- action : 오랫동안 수행되는 task용 - goal, result, feedback으로 구성

- service call과 비슷하지만 service와 다르게 도중에 중지할 수 있고, 지속적인 feedback을 준다는 차이가 있다.

- action에는 server와 client가 있다.(서버 통신처럼 client가 요청하는 방식)

- 활성화 된 action 리스트 출력 (-t : 타입도 출력)
<pre class="prettyprint">
ros2 action list
</pre>

<br>

- 해당 action 정보 출력
<pre class="prettyprint">
ros2 action info "action_name"
</pre>

<br>

- 해당 action의 args 출력
<pre class="prettyprint">
ros2 interface show "type_name.action"
</pre>

<br>

- 직접 send goal (--feedback : feedback을 계속 출력 / value는 yaml format)
<pre class="prettyprint">
ros2 action send_goal "action_name" "action_type" "values" 
</pre>

<br>
<br>

# Launch

- launch file을 만들어서 여러 노드들을 한번에 실행시킬 수 있다.
<pre class="prettyprint">
ros2 launch "package_name" "launch_file_name"
</pre>

<br>

- 작성법 : https://index.ros.org/doc/ros2/Tutorials/Launch-Files/Creating-Launch-Files/#ros2launch 참고

<br>
<br>

---
참고한 사이트 및 서적 : https://index.ros.org/doc/ros2/Tutorials/