---
layout: post
title: State Pattern (스테이트 패턴)
category: DesignPattern
tag: [DesignPattern]
---

객체의 상태를 캡슐화하고, 현재 상태를 나타내는 객체에게 행동을 위임하는 패턴

<br>

# 기존 코드
화면을 터치하면 해당 위치에 점을 그리는 프로그램이 있다.
normal 상태에서는 터치를 받아도 반응하지 않고, add 상태에서만 점이 추가된다.
if문이나 switch문을 사용하여 간단하게 구현할 수 있다.

<pre class="prettyprint">
public class drawingProgram{
  final static int NORMAL = 0;
  final static int ADD = 1;

  int state = NORMAL;
  
  //기타 메소드...

  //터치를 받았을 때
  public void handleTouchEvent(MotionEvent e){
    if (state == NORMAL){
      //아무것도 하지 않음
    } else if (state == add){
      //화면에 점 그리기
    }
  }
}
</pre>

<br>

# 문제 발생 : 기능이나 상태가 추가된다면?
