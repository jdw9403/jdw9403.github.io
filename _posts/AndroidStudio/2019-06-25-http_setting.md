---
layout: post
title: Android Pie 업데이트 이후 http 설정
category: AndroidStudio
tag: [AndroidStudio]
---

# Android Pie 업데이트 이후, http으로 접속하려면 추가 설정이 필요하다.

가장 손쉬운 방법은 https로 바꾸는 것이지만, http를 그대로 사용할 때에는 manifest에 아래 한 줄을 추가해줘도 된다.

<pre class="prettyprint">
    android:usesCleartextTraffic="true"
</pre>
<br>

아래와 같이 application내에 넣어주면 된다.

<pre class="prettyprint">
  &lt;application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="Test App"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:usesCleartextTraffic="true"
    android:theme="@style/AppTheme"&gt;

    &lt;activity
      android:name=".activity.MainActivity"
      android:screenOrientation="sensorLandscape"&gt;
      &lt;intent-filter&gt;
        &lt;action android:name="android.intent.action.MAIN"/&gt;
        &lt;category android:name="android.intent.category.LAUNCHER"/&gt;
      &lt;/intent-filter&gt;
    &lt;/activity&gt;
  &lt;/application&gt;
</pre>
<br>