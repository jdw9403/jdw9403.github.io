---
layout: post
title: Cross Reference 해결하기
category: C++
tag: [C++]
---

## Cross Reference 해결하기

<br>

# Cross Reference가 발생하는 이유

A라는 클래스와 B라는 클래스가 있고, 서로를 멤버 변수로 갖고 있다고 하자.<br>
이러한 경우, A.h에는 B를 include하고, B.h에는 A를 include하게 된다.<br>
A.cc를 컴파일하면 A.h의 #ifndef A, #define A를 타게되고, B를 include하기 위해 B.h로 간다.<br>
하지만 B에는 A가 include되어 있어 A.h로 가게되는데, 이미 A를 define했기때문에 실질적인 A.h내용을 알 수 없게 된다.<br>

<br>
<br>

# Cross Reference를 해결하는 방법

A.h에만 B를 include하고 코드를 작성한다.

<pre class="prettyprint">
#ifndef A_H
#define A_H

#include "B.h"

class A {
private:
  B b;
}

#endif //A_H
</pre>
<br>

B.h에는 A를 include하지 말고, class 선언만 해준다.<br>
A를 멤버 변수로 선언할 때에는 포인터로 지정해준다.(메모리 자리를 할당해줌)<br>

<pre class="prettyprint">
#ifndef B_H
#define B_H

class A;   //class 선언만 해줌

class B {
private:
  A *a;   //포인터로 선언
}

#endif //B_H
</pre>

<br>
<br>