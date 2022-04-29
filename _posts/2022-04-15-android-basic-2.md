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

<br>

---

<br>

# UI 구성 요소의 Attributes 변경
**Attributes** 탭에서는 UI 구성 요소의 모든 XML Attributes에 접근할 수 있다. 자세한 내용은 [이 곳](https://developer.android.com/reference/android/view/View.html?hl=ko)에서 확인 가능하다.

## 레이아웃 Width와 Height 설정
`layout_width`와 `layout_height` Attributes는 View 창에서 보이는 넓이와 높이 크기를 변경할 수 있게 해준다. 이러한 Attributes로 `ConstraintLayout`의 다음 세 가지 값 중 하나를 선택할 수 있다.  

| 이름 | 설명 | 표시 | 
|:----------|:----------|:----------:|  
| `match_constraint` | Parent의 높이나 너비의 가능한 영역 전부를 채운다. | ![match_constraint](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbp5diK%2FbtqzNCTbipc%2FogSMZSDEPSq3eQoSiP9TI0%2Fimg.png){: .center} | 
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

<br>

---

<br>

# ConstraintLayout

## ViewGroup

## Layout Variants 생성

좀 더 쉽게 Horizontal(*landscape*)이나 Vertical(*portrait*) 방향에 대한 Layout Variants를 만드는 방법에 관해 알아보자.

### Horizontal 방향 레이아웃 미리보기
1. Android Studio 프로젝트를 열자.
2. **activity_main.xml** 레이아웃 파일을 열고, **Design** 탭을 선택하자.
3. **Orientation in Editor** 버튼 ![Orientation in Editor](https://developer.android.com/codelabs/android-training-layout-editor-part-b/img/2880bbe9bf5ed4bd.png?hl=ko) 클릭
4. 드롭다운 메뉴의 **Switch to Landscape**를 고르면 레이아웃이 Horizontal 방향으로 보여진다. 다시 세로로 돌리고 싶다면 **Switch to Portrait**을 골라주자.

### Horizontal 방향 Layout Variant 만들기
1. **Orientation in Editor** 버튼 ![Orientation in Editor](https://developer.android.com/codelabs/android-training-layout-editor-part-b/img/2880bbe9bf5ed4bd.png?hl=ko)을 클릭하자.
2. **Create Landscape Variation**를 선택하자.

그러면 이제 **land/activity_main.xml** 탭이 Horizontal 방향을 위한 레이아웃을 보여주며 새로운 에디터에서 열릴 것이다. 이제 본래의 Vertical 방향을 건드리지 않고 Horizontal 방향만을 수정할 수 있다.

3. **Project > Android** 탭의 **res > layout** 디렉토리를 보면, 안드로이드 스튜디오가 자동으로 `activity_main.xml (land)`를 생성한 것을 볼 수 있다.

### Horizontal 방향 레이아웃 수정
Vertical 방향 레이아웃을 수정할 때처럼 동일한 방법으로 수정이 가능하다.

<br>

---

<br>

# 레이아웃을 LinearLayout으로 변경

`LinearLayout`이란 레이아웃의 View들을 Horizontal 혹은 Vertical 행으로 정렬하는 View Group을 말한다. 보통 다른 View 그룹 간에 UI 구성 요소를 가로 혹은 세로로 정렬하기 위해 자주 사용된다.  

블록(View)을 차곡차곡 쌓아올리는 것과도 같은 레이아웃인데, 블록을 쌓을 때 블록과 블록 사이가 비어있을 수 없는 것처럼 각 View는 연속적이다. 따라서 객체를 만들면 겹치지 않고 수평 또는 수직으로 나열된다.  

## Required Attributes
1. `layout_width`
2. `layout_height`

    - `match_parent`: 그것의 Parent의 너비나 높이를 가득 채우도록 View를 확장한다.
    - `wrap_content`: View 크기를 축소해서, View가 내용을 둘러쌀 수 있을 정도로마나 커진다. 내용이 없으면 View가 보이지 않게 된다.
    - Fixed number of dp: 장치 화면 밀도에 맞게 조정된 고정된 사이즈를 지정한다.

3. `orientation`
```
android:orientation = "~~"
```
    - horizontal: View가 왼쪽에서 오른쪽으로 정렬된다. 디폴트 값.
    - vertical: View가 위에서 아래로 정렬된다.


> LinearLayout에서는 View의 가중치(weight)를 설정할 수 있는데, 이 때 가중치란 부모 컨테이너의 남은 영역을 얼마나 차지할 것인가를 결정하는 비율 값이다. 디바이스 별로 화면 크기가 달라도 알맞은 비율을 유지할 수 있다.
{: .prompt-tip }


<!--
# Button을 위한 onClick Attribute와 Handler 추가하기
*click handler*는 클릭가능한 UI 구성 요소를 이용자가 클릭하거나 탭했을 때 호출되는 메서드다. 안드로이드 스튜디오에서는 **Degisn** 탭의 **Attributes**를 선택하여 `onClick` 필드 내의 메서드 이름을 특정할 수 있다. 혹은 XML 에디터에서 `android:onClick` 특성을 추가하는 방법으로도 가능하다.

## Toast Button handler
```java
Toast toast = Toast.makeText(this, R.string.toast_message, Toast.LENGTH_SHORT);
```
-->

<br>

---

<br>

# 레이아웃을 RelativeLayout으로 변경
`RelativeLayout`이란 그룹 내에서 각 View가 다른 View들과 상대적으로 배치되고 정렬되는 View Group을 말한다.  

LinearLayout에 비해 View의 배치가 보다 자유로운데, View들의 상대적인 위치로 배치할 수 있다는 것은 곧 부모 레이아웃에서의 상대적인 위치일 수도 있고, 특정 View에 대한 상대적인 위치일 수도 있다. 즉, 객체를 만들면 객체 혼자 움직이지 않고 항상 `id(객체 이름)`를 선정한 다른 객체를 기준으로 위치와 방향을 조정한다.


## 정렬

### 부모 레이아웃에서의 상대적인 위치 지정
1. `android:layout_alignParent(Top / Bottom / Left / Right) = ~~`: 빈 칸(`~~`)에 `true`를 넣어 View를 부모 레이아웃의 한 변에 정렬하여 배치할 수 있다.
2. `android:layout_center(Horizontal / Vertical / Inparent) = ~~`: 빈 칸에 `true`를 넣어 View를 부모 레이아웃의 수평 중앙 / 수직 중앙 / 전체 중앙으로 배치할 수 있다.

### 특정 View에 대한 상대적인 위치 지정
1. `android:layout_(above / below / toLeftOf / toRightOf) = @id/~~`: 빈 칸에 `id`를 입력하여, View를 그 `id`에 해당하는 View의 위 / 아래 / 왼쪽 / 오른쪽에 배치할 수 있다.
2. `android:layout_align(Top / Bottom / Left / Right) = @id/~~` 빈 칸에 `id`를 입력하여, View를 그 `id`에 해당하는 View의 한 변에 정렬하여 배치할 수 있다.

---

# Image Source
* <https://developer.android.com/codelabs/android-training-layout-editor-part-a/img/82bedbd4dd5bef39.png?hl=ko>
* <https://developer.android.com/codelabs/android-training-layout-editor-part-a/img/df8aacf0d83e0af7.png?hl=ko>
* <https://beomseok95.tistory.com/305#View_%ED%81%AC%EA%B8%B0_-_android:layout_width___layout_height_%EC%86%8D%EC%84%B1_%EC%82%AC%EC%9A%A9>
* <https://developer.android.com/codelabs/android-training-layout-editor-part-b/img/2880bbe9bf5ed4bd.png?hl=ko>

# References
* <https://developer.android.com/courses/fundamentals-training/toc-v2?hl=ko>
* <https://newgenerationkorea.wordpress.com/2015/07/18/layout-%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0-linearlayout%EA%B3%BC-relativelayout/>
* <https://m.blog.naver.com/789_skymert/221973230139>