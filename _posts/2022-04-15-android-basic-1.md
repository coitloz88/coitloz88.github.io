---
title: [Android 기초] 1.1 시작하기
date: 2022-04-15 14:35:00 +0900
categories: [Android, Fundamentals]
tags: [android, codelab]     # TAG names should always be lowercase
---

Android에 관해서는 동아리에서 어느정도 공부했었지만, 체계적으로 기초부터 공부한 게 아니라 중구난방으로 배운 감이 없잖아 있어서 Google에서 제공하는 [Codelab](https://developer.android.com/courses/fundamentals-training/toc-v2?hl=ko)을 보면서 다시 공부해보기로 했다.  

사용할 언어는 Java. 요즘 안드로이드는 Kotlin으로 거의 넘어간 것 같긴 하지만, Java가 익숙한 편이라 우선 Java로 공부하면서 여러 기능을 익혀보고 Kotlin으로 넘어가보려고 한다.

---
# Summary
* 프로젝트에 새로운 Library를 추가하거나 Library 버전을 바꾸기 위해서는 `build.grandle (Module:app)` 파일을 수정한다.
* 앱을 위한 모든 코드와 리소스는 app{: .filepath }과 res{: .filepath } 폴더에 위치한다. java{: .filepath } 폴더는 Java 소스 코드 형태의 Activities, Tests, 그리고 다른 Components들을 포함한다. res{: .filepath } 폴더는 Layouts, Strings, Images와 같은 리소스를 포함한다.
* 안드로이드 앱에 Features, Components나 Permissions을 추가하기 위해서는 AndroidManifest.xml{: .filepath }을 수정해줘야 한다. 여러 Activities와 같은 앱을 위한 모든 Componenets는 이 XML 파일 안에서 선언되어 있어야 한다.

# [Log](https://developer.android.com/reference/android/util/Log.html?hl=ko)
앱에 `Log` statement를 추가하여 **Logcat** 창에서 메시지를 표시할 수 있다. `Log` 메시지는 Values, Execution paths, Exceptions을 확인하기 위한 좋은 디버깅 도구이다.  

다음과 같이 `Log`를 작성할 수 있다.
```java
Log.d("MainActivity", "Sample Log Message");
```
* `Log`: Logcat 창에 로그 메시지를 보내기 위한 클래스
* `d`: 로그 메시지를 필터링하기 위한 디버그 수준 설정
    - `e`: Error
    - `w`: Warn
    - `i`: Info
* `"MainActivity"`: Logcat 창에서 메시지를 필터링하기 위한 첫번째 인자인 TAG
    - 일반적으로 메시지가 발생한 `Activity` 이름으로 설정해주어 디버깅이 편리하도록 한다.
    - `private static final String LOG_TAG = MainActivity.class.getSimpleName();`
* `"Sample Log Message"`: 실제 메시지에 해당하는 두번째 인자
---

# References
* <https://developer.android.com/courses/fundamentals-training/toc-v2?hl=ko>
