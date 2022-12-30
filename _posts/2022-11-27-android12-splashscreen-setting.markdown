---
layout: posts
title:  "[Flutter] Android12일 때 flutter_native_splash으로 스플래시 스크린 적용하는 법"
date:   2022-11-27 14:24:00 +0900
categories: flutter
---
<br>
<br>
진행하고 있는 플러터 프로젝트에서 스플래시 스크린을 바꾸기 위해 [flutter_native_splash](https://pub.dev/packages/flutter_native_splash)라는 라이브러리를 사용하였습니다.

아이폰에서는 설정한 이미지대로 스플래시 스크린이 잘 나왔지만 안드로이드 12 부터는 앱 아이콘이 스플래시 스크린 대신 나오는 현상이 발생했습니다.

이런 현상은 Android에서 기본적으로 설정한 것이기 때문에 이 라이브러리에서는 고칠 수 있는 방법이 없었습니다.

그래서 검색해봤더니 **android/app/src/main/AndroidManifest.xml** 에 아래처럼 코드를 추가하면 된다고 해서 바로 실행해보았습니다.

```xml
    <meta-data
        android:name="io.flutter.embedding.android.SplashScreenDrawable"
        android:resource="@drawable/launch_background"
    />
```
**android/app/src/main/AndroidManifest.xml**
```xml
<application
        ...
        android:icon="@mipmap/launcher_icon"
        ...>
        <activity
            android:name=".MainActivity"
            ...>
            <!-- Displays an Android View that continues showing the launch screen
                 Drawable until Flutter paints its first frame, then this splash
                 screen fades out. A splash screen is useful to avoid any visual
                 gap between the end of Android's launch screen and the painting of
                 Flutter's first frame. -->
            <meta-data
              android:name="io.flutter.embedding.android.SplashScreenDrawable"
              android:resource="@drawable/launch_background"
              />
    </application>
```

코드를 추가하고 앱을 켜봤더니 성공적으로 앱 아이콘이 잠시 나오고 내가 설정한 스플래시 스크린으로 나오게 되었습니다.

**참조:**
<https://dev-yakuza.posstree.com/ko/flutter/splash-screen/>
{: .notice--info}