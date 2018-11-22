# XVideoRecorder

由于`JCameraView`好久没有更新了，所以在此项目的基础上做了些细微改动(再次感谢JCameraView作者开源)
[JCameraView源码](https://github.com/CJT2325/CameraView)

> 添加依赖

`root build.gradle `
```
allprojects {
    repositories {
        ...
        maven {
            url 'https://jitpack.io'
        }
    }
}
```
`module build.gradle `
```
implementation 'com.github.fonuhuolian:XVideoRecorder:0.0.9'
```
> 添加权限

`跳转到视频拍摄页面前需要先进行动态权限申请`
```
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-feature android:name="android.hardware.camera" />
<uses-feature android:name="android.hardware.camera.autofocus" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_SETTINGS" />
```
> xml
```
 <com.cjt2325.cameralibrary.JCameraView
    android:id="@+id/jcameraview"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:iconMargin="20dp"
    app:iconSize="30dp"
    app:iconSrc="@drawable/ic_camera_enhance_black_24dp" />
```
> 初始化JCameraView控件
```
//设置视频保存路径
jCameraView.setSaveVideoPath(Environment.getExternalStorageDirectory().getPath() + File.separator + "ourpyw" + File.separator + "video");
// 设置视频的录制质量
jCameraView.setMediaQuality(MEDIA_QUALITY_MIDDLE);
// 设置视频录制的最大时长
jCameraView.setRecordMaxDuration(15000);

//JCameraView监听
jCameraView.setJCameraLisenter(new JCameraListener() {
    @Override
    public void captureSuccess(Bitmap bitmap) {
        //获取图片bitmap
    }

    @Override
    public void recordSuccess(String url, Bitmap firstFrame) {

        firstFrame.recycle();

        Intent intent = getIntent().putExtra("url", url);
        setResult(10, intent);
        finish();
    }
});

//设置只能录像或只能拍照或两种都可以（默认两种都可以）
jCameraView.setFeatures(JCameraView.BUTTON_STATE_ONLY_RECORDER);
// 设置文字描述
jCameraView.setTip("长按摄像");
// 设置摄像左侧按钮的监听事件
jCameraView.setLeftClickListener(new ClickListener() {
    @Override
    public void onClick() {
        finish();
    }
});

// 错误的监听
jCameraView.setErrorLisenter(new ErrorListener() {
    @Override
    public void onError() {
        finish();
    }

    @Override
    public void AudioPermissionError() {
    }
});

```
> JCameraView生命周期
```
@RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
@Override
protected void onStart() {
    super.onStart();
    jCameraView.onStart(this);
}

@Override
public void onResume() {
    super.onResume();
    jCameraView.onResume();
}

@Override
public void onPause() {
    super.onPause();
    jCameraView.onPause();
}
```

> 混淆

```
-dontwarn com.cjt2325.cameralibrary.**
-keep class com.cjt2325.cameralibrary.**{*;}
```

