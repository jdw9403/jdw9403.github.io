---
layout: post
title: State Pattern (스테이트 패턴)
category: DesignPattern
tag: [DesignPattern]
---
## 객체의 상태를 캡슐화하고, 현재 상태를 나타내는 객체에게 행동을 위임하는 패턴
<br>

# 기존 코드

컴퓨터의 전원 버튼을 누른다고 하면, 다음과 같이 나타낼 수 있다.

> 1. 컴퓨터가 켜져있을 때 -> 꺼짐
> 2. 컴퓨터가 꺼져있을 때 -> 켜짐

<pre class="prettyprint">
public class Computer{
  final static int POWER_ON = 0;
  final static int POWER_OFF = 1;

  int curState = POWER_ON;
  
  //기타 메소드...

  //전원 버튼을 누름
  public void pushPowerButton(){
    if (curState == POWER_ON){
      System.out.println("전원 꺼짐");
      changeState(POWER_OFF);
    } else if (curState == POWER_OFF){
      System.out.println("전원 켜짐");
      changeState(POWER_ON);
    }
  }
</pre>
<br>

# 문제점 : 다른 상태가 추가된다면?

단순한 꺼짐, 켜짐 상태뿐만 아니라, **절전 상태**가 추가되면 코드가 복잡해진다.

> 1. 컴퓨터가 켜져있을 때 -> 절전 상태 
> 2. 컴퓨터가 꺼져있을 때 -> 켜짐
> 3. 컴퓨터가 절전상태일 때 -> 꺼짐

<pre class="prettyprint">
public class Computer{
  final static int POWER_ON = 0;
  final static int POWER_OFF = 1;
  final static int POWER_SAVING = 2;

  int curState = POWER_ON;
  
  public void changeState(int state){
    curState = state;
  }

  //전원 버튼을 누름
  public void pushPowerButton(){
    if (curState == POWER_ON){
      System.out.println("절전 모드");
      changeState(POWER_SAVING);
    } else if (curState == POWER_OFF){
      System.out.println("전원 켜짐");
      changeState(POWER_ON);
    } else if (curState == POWER_SAVING){
      System.out.println("전원 꺼짐");
      changeState(POWER_OFF);
    }
  }
</pre>

상태가 늘어날수록 **코드는 길어지고**, if문과 switch문이 복잡해져 **생각지못한 버그**가 발생할 수도 있다.
또한 다른 곳에서 같은 코드를 쓴다면, **매번 코드를 수정**해야한다.
<br>

# 스테이트 패턴 적용

객체지향 원칙에 따르면 바뀌는 부분을 **캡슐화** 해야된다.
이에 따라 스테이트 패턴에서는 **상태를 인터페이스로 캡슐화하고, 현재 상태를 나타내는 객체에게 행동을 위임한다.**

인터페이스는 다음과 같이 정의할 수 있다.

<pre class="prettyprint">
public interface PowerState {
  public void pushPowerButton();
}
</pre>

켜짐, 꺼짐, 절전모드는 위 인터페이스를 구현한다.

<pre class="prettyprint">
public class OnState implements PowerState {
  Computer computer;

  public OnState(Computer computer){
    this.computer = computer;
  }

  public void pushPowerButton(){
    System.out.println("절전 모드");
    computer.changeState(POWER_SAVING);
  }
}
</pre>


<pre class="prettyprint">
public class OffState implements PowerState {
  //생성자 및 변수 등..

  public void pushPowerButton(){
    System.out.println("전원 켜짐");
    changeState(POWER_ON);
  }
}
</pre>


<pre class="prettyprint">
public class SavingState implements PowerState {
  //생성자 및 변수 등..

  public void pushPowerButton(){
    System.out.println("전원 꺼짐");
    changeState(POWER_OFF);
  }
}
</pre>


반복되는 if문으로 인해 지저분했던 기존 코드는 다음과 같이 바꿀 수 있다.

<pre class="prettyprint">
public class Computer{
  PowerState onState;
  PowerState offState;
  PowerState savingState;

  PowerState curState;

  public Computer(){
    onState = new OnState(this);
    offState = new OffState(this);
    savingState = new SavingState(this);

    changeState(onState);
  }
  
  //기타 메소드...

  //스테이트 패턴이 적용된 코드
  public void pushPowerButton(){
    curState.pushPowerButton();
  }
</pre>
<br>

# 정리

![Image](/assets/DesignPattern/2019-04-02-state_pattern/diagram.png)

위의 스테이트 패턴 다이어그램을 보면, context는 여러 상태를 갖고 있고 request()가 호출되면 그 작업을 상태 객체에 위임한다.
State 인터페이스를 구현한 구상 상태 클래스에서 자기 나름의 방식으로 요청을 처리한다.(handle())

스테이트 패턴을 사용하면 다음과 같은 이점을 얻을 수 있다.

>- 복잡하고 버그가 발생할 수 있는 if문 투성이 코드가 이해하기 쉽고 간결해진다.
>- 각 상태는 변경에 대해서 닫혀있지만 Computer는 새로운 상태를 추가하는 확장에 대해 열려있다.
>  cf) OCP(Open Closed Principle : 확장에 대해서는 개방, 수정에 대해서는 폐쇄)

즉 스테이트 패턴은 다음과 같은 경우에 사용하

<br>
참고한 사이트 : https://victorydntmd.tistory.com/294
