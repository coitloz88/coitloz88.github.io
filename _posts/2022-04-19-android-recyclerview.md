---
title: Android | RecyclerView
date: 2022-04-15 14:35:00 +0900
categories: [Android]
tags: [android, recyclerview]     # TAG names should always be lowercase
---

# RecyclerView 개념

RecyclerView를 사용하면 대량의 데이터 세트를 효율적으로 표시할 수 있다. RecyclerView는 개별 요소를 *재활용*하여, 항목이 스크롤되어 화면에서 벗어나더라도 RecyclerView는 뷰를 제거하지 않는다. 대신 스크롤된 새 항목의 뷰를 재사용한다.  
![ListView vs. RecyclerView](https://velog.velcdn.com/images%2Fhoyaho%2Fpost%2F9bbc5c00-4d42-4732-b26c-7a839380cd85%2Fimage.png){:. center}

---

# RecyclerView 활용

## 1. 레이아웃 영역 준비

### 1-1. Main Activity에 RecyclerView 추가
`Main Activity`{:. filepath } 등의 자신이 사용할 Activity에 RecyclerView를 추가해준다.

### 1-2. RecyclerView 레이아웃 설정
데이터가 어떻게 들어갈 것인지, 그 틀을 정해주어야 한다. 

## 2. 모델 생성
1-2.에서 생성한 레이아웃을 실제 모델(코드)로 구현한다.

## 3. Adapter 생성
Adapter에서는 작성한 RecyclerView 아이템 레이아웃과 데이터를 실제로 연결하는 역할을 수행한다. 
`RecyclerView.Adapter`를 상속받아 새로운 Adapter를 만들 때, Override가 필요한 메서드는 다음과 같다.

## 4. RecyclerView에 Adapter와 LayoutManager 지정
실제 RecyclerView에 연결해주자.

| 메서드 | 설명 | 
|:----------|:----------|  
| `onCreateViewHolder(ViewGroup parent, int viewType)` | viewType 형태의 아이템 뷰를 위한 ViewHolder 객체 생성 |
| `onBindViewHolder(ViewHolder holder, int position)` | position에 해당하는 데이터를 ViewHolder의 아이템뷰에 표시 |
| `getItemCount()` | 전체 아이템 개수를 리턴 |


---
# Image 출처
* <https://velog.velcdn.com/images%2Fhoyaho%2Fpost%2F9bbc5c00-4d42-4732-b26c-7a839380cd85%2Fimage.png>


# References
* <https://developer.android.com/guide/topics/ui/layout/recyclerview?hl=ko>
* <https://developer.android.com/codelabs/android-training-create-recycler-view?index=..%2F..%2Fandroid-training&hl=ko#0>
* <https://recipes4dev.tistory.com/154>
* <https://velog.io/@hoyaho/RecyclerView>