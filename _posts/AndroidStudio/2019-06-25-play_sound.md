---
layout: post
title: MediaPlayer로 소리 재생하기
category: AndroidStudio
tag: [AndroidStudio]
---

<br>

#  MediaPlayer로 소리 재생하기

<pre class="prettyprint">
class Study {
    MediaPlayer player;

    private void playSound(int resId) {
        try {
            player = MediaPlayer.create(getContext(), resId);
            player.setOnCompletionListener(mp -> {
                player.reset();
                player.release(); //잡고있는 player를 놓아줘야 error log가 뜨지 않는다
            });
            player.setOnPreparedListener(mp -> player.start());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
</pre>
<br>
<br>
