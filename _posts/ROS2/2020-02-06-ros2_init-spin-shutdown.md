---
layout: post
title: ROS2 Init, Spin, Shutdown
category: ROS2
tag: [ROS2]
---

## ROS2 Init, Spin, Shutdown 기본 내용을 정리한 글 입니다.

<br>

일반적인 ROS 프로그램은 아래와 같은 순서로 작동한다.

1. Init
2. Nodes 생성
3. Spin
4. Shutdown

<br>

Init은 해당 context에서 node가 만들어지기 전에 반드시 수행되어야 한다.<br>
(init())

<br>

create_node()나 인스턴스화를 통해 node를 만든다.

<br>

node가 만들어지면, node를 spin시켜 작업을 수행시킬 수 있다. (보통 blocking)<br>
spin은 메세지 큐를 돌면서 메세지를 확인하는 작업이라 생각하면 된다.<br>
pub같이 송신하는 작업은 굳이 spin이 필요없는 듯 보임.<br>

인자로 node를 안 넣어주는 spin은 exectutor에 rclcpp::executor::Executor::add_node()로 넣어주면 된다.(removeNode())<br>
몇몇 spin은 timeout을 설정할 수도 있다.(timeout=None이면 계속 block, 0이면 안 기다림)

- spin(node, ..) : exectuor가 shutdown될 때까지 메세지 큐 순회(block) (executor=None이면 global exectuor)

- spin_until_future_complete(node, future, ..) : future가 완료될 때 까지(+shutdown될 때까지) 메세지 큐 순회


일반 spin은 blocking이지만(내부에서 루프돌며 계속 메세지 큐 확인) 아래는 non-blocking이다.<br>
즉, 메세지 큐를 계속 확인하려면 사용자가 외부에서 직접 루프문을 돌려줘야된다.

- spin_some(node) : 메시지 큐 한 번 순회하고 넘어감

- spin_once(node) : 메세지 큐에 있는 작업 하나 수행인듯(?)


addNode()로 여러 노드를 넣어줄 수도 있지만, spin_some()으로 각 노드들을 돌릴 수도 있을 듯하다.
<pre class="prettyprint">
while(true) {
    spin_some(node1);
    spin_some(node2);
}
</pre>
<br>

마지막으로 shutdown 해준다.
(shutdown())

<br>
<br>
<br>


---
참고한 사이트 및 서적 : http://docs.ros2.org/latest/api/rclpy/api/init_shutdown.html<br>
http://docs.ros2.org/dashing/api/rclcpp/classrclcpp_1_1executor_1_1Executor.html#a34509baf643c9a7f88bf0b7f4a983955