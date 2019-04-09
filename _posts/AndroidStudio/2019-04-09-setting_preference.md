---
layout: post
title: Preference Fragment로 설정창 구현하기
category: AndroidStudio
tag: [AndroidStudio]
---

## Preference Fragment로 안드로이드 설정창을 쉽게 구현할 수 있다.
<br>
설정창에서 선택되거나 입력된 값은 파일로 저장되어 앱이 다시 시작되어도 유지되며, 
키 값을 이용하여 SharedPreferences에서 불러올 수 있다.<br>

# 1. gradle 설정하기

기존 PreferenceFragment가 deprecated될 예정이라 PreferenceFragmentCompat을 사용할 수 있도록 모듈 gradle을 설정한다.
<pre class="prettyprint">
    implementation "com.android.support:preference-v7:28.0.0"
</pre>
<br>

# 2. xml 파일 작성

체크박스, editText, 스위치, 목록 선택 등 여러 가지 설정창을 구현할 수 있다.<br>
PreferenceCategory는 여러 설정을 하나의 카테고리로 묶을 수 있으며, summary는 설정된 값을 보여준다.<br>
목록에서 선택하는 설정을 구현하려면 array.xml을 작성해야 한다.<br>
- 참고 : https://developer.android.com/guide/topics/ui/settings.html#Fragment<br>

<pre class="prettyprint">
&lt;PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"&gt;
  &lt;PreferenceCategory android:title="서버 관련 설정"&gt;
    &lt;EditTextPreference
      android:key="ip"
      android:title="ip address"
      android:defaultValue="192.168.0.1"
      android:summary="192.168.0.1"/&gt;

  &lt;/PreferenceCategory&gt;
&lt;/PreferenceScreen&gt;
</pre>
<br>

# 3. SettingActivity와 SettingFragment 작성

기존의 PreferenceActivity, PreferenceFragment가 아닌 AppCompatActivity와 PreferenceFragmentCompat를 각각 extends한다.<br>

<pre class="prettyprint">
public class SettingsActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        getSupportFragmentManager().beginTransaction()
            .replace(android.R.id.content, new SettingFragment())
            .commit();
    }

    public static class SettingFragment extends PreferenceFragmentCompat {

        public final static String KEY_IP = "ip";
        public final static String DEFAULT_IP = "192.168.0.1";

        SharedPreferences prefs;
        Preference ipPreference;

        @Override
        public void onCreatePreferences(Bundle bundle, String s) {
            addPreferencesFromResource(R.xml.settings_preference);
            prefs = PreferenceManager.getDefaultSharedPreferences(getActivity());
            prefs.registerOnSharedPreferenceChangeListener(prefListener);

            ipPreference = findPreference(KEY_IP);
            String ip = prefs.getString(KEY_IP, DEFAULT_IP); //저장된 값 가져옴
            ipPreference.setSummary(ip); //저장된 값을 summary에 뿌려줌
        }

         //설정창에서 change 생기면 알아차림       
        SharedPreferences.OnSharedPreferenceChangeListener prefListener = new SharedPreferences.OnSharedPreferenceChangeListener() {
            @Override
            public void onSharedPreferenceChanged(SharedPreferences sharedPreferences, String key) {
                if (key.equals(KEY_IP)) {
                    SharedPreferences.Editor editor = prefs.edit();
                    String ip = prefs.getString(KEY_IP, DEFAULT_IP);
                    editor.putString(key, ip); //새 값으로 변경
                    editor.apply();
                    ipPreference.setSummary(ip);
                }
            }
        };
    }
}
</pre>
<br>

# 4. 저장된 설정값 가져오기
<pre class="prettyprint">
    public static String getIp(Context context) {
        SharedPreferences prefs = PreferenceManager.getDefaultSharedPreferences(context);
        return prefs.getString(SettingFragment.KEY_IP, SettingFragment.DEFAULT_IP);
    }
</pre>
<br>


참고한 사이트 : 
https://mainia.tistory.com/1877,<br>
https://blog.hanumoka.net/2018/01/06/android-20180106-android-PreferenceFragment/,<br>
https://gogorchg.tistory.com/entry/Android-Preferencefragment-deprecated<br>