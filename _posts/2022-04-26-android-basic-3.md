---
title: Android 기초 1.3 Text and scrolling views
date: 2022-04-26 15:15:00 +0900
categories: [Android, Fundamentals]
tags: [android, codelab]     # TAG names should always be lowercase
---

> 지난 글: [1.2 대화형 UI](https://coitloz88.github.io/posts/android-basic-2/)

---

# TextView
`TextView` 클래스는 `View`의 서브클래스로, 스크린에 텍스트를 표시한다.  
만약 디바이스 화면보다 더 많은 정보를 표시하려고 하는 경우, 사용자가 가로 혹은 세로로 스크롤해서 정보를 확인할 수 있게 하고 싶을 수도 있는데, 이 경우는 `ScrollView`를 활용할 수 있다.

# ScrollView
`ScrollView` 클래스는 `FrameLayout`의 서브클래스로, 디바이스 화면에 표시가 안되고 있는 UI 구성 요소도 메모리 및 View 계층 구조에 있게 할 수 있다. 텍스트가 이미 메모리 상에 있기 때문에 자유롭게 스크롤링할 수 있다. 그러나 메모리를 많이 사용하기 때문에 앱의 퍼포먼스에 끼치는 영향을 잘 고려해서 사용해야 한다.  
사용자가 추가, 삭제, 편집할 수 있는 많은 양의 아이템 리스트를 표시하기 위해서는 `RecyclerView`를 사용하는 것이 적절하다.

# References
* <https://developer.android.com/courses/fundamentals-training/toc-v2?hl=ko>
