---
layout: post
title: thread와 handler 사용
category: AndroidStudio
tag: [AndroidStudio]
---

# Thread

안드로이드의 메인 스레드에서 작업이 지나치게 오래 걸리면 앱이 죽을 수 있다.(ANR)<br>
이를 방지하기 위해 오래 걸리거나 부하가 큰 작업은 별도의 스레드를 만들어서 수행하는데, 이를 **작업 스레드**라고 한다.<br>
네트워크와 관련된 작업은 반드시 별도의 스레드를 만들어 수행해야한다.<br>

## 1. Thread 이용

<pre class="prettyprint">
//익명
new Thread(){
    @Override
    public void run() {
        //작업
    }
}.start();
</pre>
<br>

아래와 같은 경우에는 객체 생성 후 myThread.start()를 호출한다.<br>

<pre class="prettyprint">
//클래스로 분리
class MyThread extends Thread {
    @Override
    public void run() {
        //작업
    }
}
</pre>
<br>


## 2. Runnable 이용

자바는 다중 상속이 안되므로 다른 클래스를 상속받을 때에는 Runnable Interface를 구현한다.<br>
이 경우에는 thread 객체 생성시 Thread thread = new Thread(myRunnable)과 같이 runnable 객체를 넣어준 후, thread.start()를 호출한다.<br>

<pre class="prettyprint">
class MyRunnable implements Runnable{
    @Override
    public void run() {
        //작업
    }
}
</pre>
<br>
<br>

# Handler

안드로이드 스튜디오에서는 메인 UI 스레드에서만 UI 변경이 가능하다.<br>
다른 작업 스레드에서 UI 변경을 하려면 **핸들러를 사용하여 메인 스레드에 전달**해야한다.(우체부같은 역할)<br>
메인 스레드에서 생성한 핸들러를 작업 스레드에 넘겨주면 post(), sendMessage()등을 사용하여 메인 스레드에 메세지를 전달할 수 있다.<br>

<pre class="prettyprint">
//메인 스레드에서 핸들러 생성(MainActivity 등)
Handler handler = new Handler();
//
//
//
class MyThread extends Thread {
    //넘겨받은 handler 객체

    @Override
    public void run() {
        //기타 작업
        handler.post(new Runnable()){
            @Override
            public void run() {
                //ui 관련 작업
            }
        });
    }
}
</pre>
<br>
<br>

참고한 사이트 : https://bitsoul.tistory.com/100