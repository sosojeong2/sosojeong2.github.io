---
layout: post
title: "[Android-Kotlin]앱 시작하기"
date: 2021-03-25 17:15:23 +0900
category: Android-Kotlin
---

# [Android-Kotlin]앱 시작하기

코틀린이란? 

1. JetBrains에서 공개한 프로그래밍 언어
2. JVM(Java Vertual Machine) 기반 언어
3. JAVA와 함께 이용이 가능
4. JVM에선 바이트코드가 기본적으로 사용되고, 기계어로도 컴파일이 가능하다.
5. 구글에서 채택한 공식언어
6. 코딩 시 정말 간결하다
7. 널포인터 익셉션으로부터 고통을 줄여준다.
8. 세미콜론 안써도된다.
9. 최근 안드로이드 개발 회사에선 선택이 아니라 필수다. 

목표 : hello world 버튼 클릭 시 토스트 메세지를 출력하는 앱 만들기

액티비티 만들기 : 그림에서 도화지 역할 , 버튼을 배치할 수 있는 액티비티

패키지 만들기 : ( 오른쪽 버튼 → New → Package  )

액티비티를 모아두는 폴더를 만듭니다. 

액티비티 파일 만들기 : 코틀린에선 .kt로 끝나는 확장자를 사용해서 코틀린 파일임을 나타내고 있다.  ( 오른쪽 버튼 → New → Kotiln File/Class )  

```kotlin
package com.example.myapplication.activity

import android.app.Activity
import android.os.Bundle

class HelloActivity : Activity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        //내용 작성하기
    }
}
```

레이아웃 만들기 : res폴더 안에 layout 폴더를 만들어 준다.

( 오른쪽 버튼 → New → Directory ) 

레이아웃으로 사용할 xml 파일을 만들어 줍니다.  

```kotlin
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent" android:layout_height="match_parent">

    <Button
        android:id="@+id/btnHello"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="HelloWorld"
        android:textAllCaps="false"
				android:layout_centerInParent="true" />
</RelativeLayout>
```

액티비티와 레이아웃을 연결한다. 

build.gradle 파일에 apply plugin: 'kotlin-android-extensions 을 추가해주면 편하게 개발이 가능하다. 

토스트 메세지를 띄워본다.ㅎㅎ

```kotlin
package com.example.myapplication.activity

import android.app.Activity
import android.os.Bundle
import android.widget.Toast
import com.example.myapplication.R
import kotlinx.android.synthetic.main.activity_hello.*

class HelloActivity: Activity(){
    override fun onCreate(savedInstanceState: Bundle?){
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_hello)

        btnHello.setOnClickListener(){
            Toast.makeText(this, "토스트 메시지", Toast.LENGTH_SHORT).show()
        }
    }
}
```

에뮬레이터 실행 

'Error running app : Default Activity not found'가 뜹니다

Android Manifest 

여기에 application 정보만 있고 Activity에 관한 정보가 없다.

또, 처음 시작하는 액티비티라고 알려줘야한다. 

(MAIN action과 LAUNCHER category를 추가합니다.)

- action.MAIN 시작 액티비티 지정
- category.LAUNCHER 애플리케이션에서 가장 먼저 실행되는 액티비티

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapplication">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication">

        <activity android:name=".activity.HelloActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```