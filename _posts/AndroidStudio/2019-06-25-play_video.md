---
layout: post
title: videoView로 동영상 재생하기
category: AndroidStudio
tag: [AndroidStudio]
---

<br>

# videoView로 동영상 재생하기

<pre class="prettyprint">
public class MainActivity extends AppCompatActivity {
    VideoView videoView;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        videoView = findViewById(R.id.videoView);
    }

    private void playVideo(int resId) {
        Uri videoUri = Uri.parse("android.resource://" + getActivity().getPackageName() + "/" + resId);
        videoView.setVideoURI(videoUri);
        videoView.setOnCompletionListener(onCompletionListener);
        videoView.requestFocus();
        videoView.start();
    }

    MediaPlayer.OnCompletionListener onCompletionListener = new MediaPlayer.OnCompletionListener() {
        @Override
        public void onCompletion(MediaPlayer mp) {
            videoView.start(); //onCompletionListener로 반복 재생 등 가능
        }
    };
}
</pre>
<br>
<br>





