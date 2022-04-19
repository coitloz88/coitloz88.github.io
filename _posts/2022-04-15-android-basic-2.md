---
title: Android 기초 1.2 대화형 UI
date: 2022-04-15 15:00:00 +0900
categories: [Android, Fundamentals]
tags: [android, codelab]     # TAG names should always be lowercase
---

> 지난 글: [1.1 시작하기](https://coitloz88.github.io/posts/android-basic-1/)

---

# Layout Editor 살펴보기

![Android Studio Layout Editor](https://developer.android.com/codelabs/android-training-layout-editor-part-a/img/76af3429c6df2e78.png?hl=ko)

1. `activity_main.xml`{: .filepath }에서는 실제로 사용자에게 보이는 화면 구성을 확인할 수 있다.
2. **Design** 탭을 사용하여 각종 Elements와 Layout을 조작할 수 있다. **Code** 탭에서는 Layout의 XML 코드를 편집할 수 있다.
3. **Palette** 탭에서는 앱의 레이아웃에 사용할 수 있는 UI 구성 요소를 보여준다.
4. **Component tree**에서는 UI 구성 요소의 계층 구조를 보여준다.
5. 레이아웃 편집기의 Design 화면과 Blueprint 화면에서는  레이아웃의 UI 구성 요소를 보여준다.
6. **Attributes** 탭에서는 UI 구성 요소의 Properties를 설정할 수 있다.

# Layout Editor에 View 구성 요소 추가하기

* Constraint handle
![Constraint handle](https://developer.android.com/codelabs/android-training-layout-editor-part-a/img/82bedbd4dd5bef39.png?hl=ko){: width="10%"}{: .center}  
* Resizing handle
![Resizing handle](https://developer.android.com/codelabs/android-training-layout-editor-part-a/img/df8aacf0d83e0af7.png?hl=ko){: width="10%"}{: .center}  

# UI 구성 요소의 Attributes 변경
**Attributes** 탭에서는 UI 구성 요소의 모든 XML Attributes에 접근할 수 있다. 자세한 내용은 [이 곳](https://developer.android.com/reference/android/view/View.html?hl=ko)에서 확인 가능하다.

## 레이아웃 Width와 Height 설정
`layout_width`와 `layout_height` Attributes는 View 창에서 보이는 넓이와 높이 크기를 변경할 수 있게 해준다. 이러한 Attributes로 `ConstraintLayout`의 다음 세 가지 값 중 하나를 선택할 수 있다.
| 이름 | 설명 | 표시 | 
|:----------:|:----------|:----------:|  
| `match_constraint` | Parent의 높이나 너비의 가능한 영역 전부를 채운다. | ![match_constraint](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbp5diK%2FbtqzNCTbipc%2FogSMZSDEPSq3eQoSiP9TI0%2Fimg.png){: width="10%"}{: .center} | 
| `wrap_content` | View 크기에 맞춘다. | ![wrap_content](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbE6DeU%2FbtqzOedxqEE%2FnlEy1kP4oj7FhNoRlQFG0K%2Fimg.png) |
| `dp` | 디바이스의 스크린 사이즈에 맞는 고정된 사이즈를 지정한다. | ![dp](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbfLwwZ%2FbtqzM9wTGTH%2Fc3kVkikqAMYG3zeTKXmWlk%2Fimg.png) |  

> Hard-coding Strings 경고 문구와 관련하여
    [여기](https://developer.android.com/codelabs/android-training-layout-editor-part-a?index=..%2F..%2Fandroid-training&hl=ko#6)를 참조하여 해당 경고를 없앨 수 있다.
{: .prompt-tip }

# Toast Message 출력
`Toast`는 작은 팝업 창으로 간단한 메시지를 보여주는 기능을 제공한다. 메시지에 필요한 만큼의 공간만을 채운다. `Toast`는 일반적으로 다음과 같은 코드를 통해 그 인스턴스를 생성한다.  
```java
Toast toast = Toast.makeText(this, R.string.toast_message, Toast.LENGTH_SHORT);
```

<!--
# Button을 위한 onClick Attribute와 Handler 추가하기
*click handler*는 클릭가능한 UI 구성 요소를 이용자가 클릭하거나 탭했을 때 호출되는 메서드다. 안드로이드 스튜디오에서는 **Degisn** 탭의 **Attributes**를 선택하여 `onClick` 필드 내의 메서드 이름을 특정할 수 있다. 혹은 XML 에디터에서 `android:onClick` 특성을 추가하는 방법으로도 가능하다.

## Toast Button handler
```java
Toast toast = Toast.makeText(this, R.string.toast_message, Toast.LENGTH_SHORT);
```
-->
---

# Image Source
* <https://developer.android.com/codelabs/android-training-layout-editor-part-a/img/82bedbd4dd5bef39.png?hl=ko>
* <https://developer.android.com/codelabs/android-training-layout-editor-part-a/img/df8aacf0d83e0af7.png?hl=ko>
# <https://beomseok95.tistory.com/305#View_%ED%81%AC%EA%B8%B0_-_android:layout_width___layout_height_%EC%86%8D%EC%84%B1_%EC%82%AC%EC%9A%A9>

# References
* <https://developer.android.com/courses/fundamentals-training/toc-v2?hl=ko>
