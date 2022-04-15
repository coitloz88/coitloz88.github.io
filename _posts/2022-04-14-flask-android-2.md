---
title: Android에서 Flask 서버 접속하기 2 - SwipeRefreshLayout 사용
date: 2022-04-14 16:55:00 +0900
categories: [Computer, Android]
tags: [android, flask, volley, json]     # TAG names should always be lowercase
---

[이전 글](https://coitloz88.github.io/posts/flask-android-1/)에서 Flask 로컬 서버에서 랜덤한 숫자를 발생시키고 해당 숫자를 안드로이드 에뮬레이터에서 받아오는 예제를 진행해보았다.  

<br>

이번에 추가한 내용은 다음과 같다.
* .json Format의 지정해둔 key 값을 통해 랜덤한 숫자 value만을 받아온다.
* 이를 안드로이드 앱에서 `Random Number: X` 형태로 출력한다.
* ConstraintLayout 대신 SwipeRefreshLayout을 사용하여 당겨서 새로고침 기능을 추가했다. 이를 통해 앱 종료 후 재실행이 아닌 단순 새로고침으로 난수가 계속해서 바뀌는 것을 확인할 수 있다.

코드는 [이 쪽](https://github.com/coitloz88/Flask-Android-link/tree/main/AndroidApp).  


---

# Flask(Jupyter notebook, python) 서버 part
jupyter notebook에서 실행했다. 이전과 크게 달라진 점은 없으며, value를 string으로 변환하지 않고 랜덤하게 발생한 int값을 그대로 넘긴다.
```python
import flask
import random

app = flask.Flask(__name__)

@app.route('/')
def make_randomNumber():
    ranStr = random.randrange(1, 11)
    return flask.jsonify({'tk':ranStr}), 200
    
if __name__ == '__main__':
    app.run(host = '0.0.0.0')
```

로컬 서버를 구동하고 있는 상태에서 안드로이드 앱을 실행하자. 자꾸 까먹는다.  

<br>

## TODO  
AWS를 이용하여 실제 퍼블릭 서버 배포

<br>

---

# Android(Android studio, java) 어플리케이션 part

## `onResponse()`에서  JSON Object를 받아와 파싱하기 [(참고)](https://infos.tistory.com/4004)

추가된 코드는 다음과 같다.
```java
import org.json.JSONException;
import org.json.JSONObject;

...

public void onResponse(String response) {
    try {
        JSONObject jsonObject = new JSONObject(response);
        ((TextView) findViewById(R.id.tv_test_message)).setText("Random Number: " + jsonObject.getInt("tk"));
        //python 코드에서 json의 key값을 'tk'로 설정해줬기 때문에 해당 key로 난수 value를 받아온다.
    } catch (JSONException e) {
        Toast.makeText(getApplicationContext(), "Err ErrorListener:" + e, Toast.LENGTH_SHORT).show();
    }
}

...
```

<br>

## SwipeRefreshLayout 사용하기[(참고)](https://stickode.tistory.com/20)

### build.grandle(:app)에 dependency 추가
해당 파일의 `dependencies{ ... }` 부분에 다음 문구를 추가한 뒤 `Sync now`{: .filepath }로 다운로드 받는다.
```
implementation "androidx.swiperefreshlayout:swiperefreshlayout:1.1.0"
```

### `Activity_main.xml` 수정하기
우선 `activity_main.xml`을 수정해줘야한다.  
Code를 직접 수정해준다. 본래 `<androidx.constraintlayout.widget ... >`{: .filepath }으로 되어있을텐데, 이 부분을 포함하여 Textview의 설정도 ConstraintLayout에서 SwipeRefreshLayout으로 바뀌므로 일부 바꿔준다.  

본래는 새로고침할 View 영역을 SwipeRefreshLayout으로 감싸줘야 하는데, 내 경우는 textview 하나 있는 거 감싸주는 거라 통째로 고쳐주었다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.swiperefreshlayout.widget.SwipeRefreshLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/swipeLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/tv_test_message"
        android:gravity="center"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Empty Message"
        android:textSize="20sp">
    </TextView>

</androidx.swiperefreshlayout.widget.SwipeRefreshLayout>
```

### `MainActivity.java` 수정하기
1. MainActivity 클래스가 SwipeRefreshLayout.OnRefreshListener 인터페이스를 implement하게 수정

2. SwipeRefreshLayout을 객체로 생성

3. `onCreate( ... )` 메서드에서 SwipeRefreshLayout에 `setOnRefreshListener()` 메서드를 통해 리스너를 달아준다.  

4. `onRefresh()` 메서드에서 새로고침 시 변경되는 사항을 작성한다. 해당 메서드에 작성한 내용은 사용자가 화면을 위에서 아래로 당기면 호출된다. 

> SwipeRefreshLayout을 사용해서 새로고침을 구현하는 경우, `setRefreshing()` 메서드로 화면을 update하고 새로고침이 끝났다면 반드시 `setRefreshing()` 메서드 인자에 `false`{: .filepath }를 세팅해주어야 한다. (Boolean 값으로 상단 원형 ProgressBar의 Visibility를 설정함)
{: .prompt-tip }

```java
package com.example.flaskservertest;

import androidx.appcompat.app.AppCompatActivity;
import androidx.swiperefreshlayout.widget.SwipeRefreshLayout;

import android.os.Bundle;

public class MainActivity extends AppCompatActivity implements SwipeRefreshLayout.OnRefreshListener {

    SwipeRefreshLayout srl;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        srl = findViewById(R.id.swipeLayout);
        srl.setOnRefreshListener(this);

    }

    @Override
    public void onRefresh() {
        updateLayoutView();
        srl.setRefreshing(false);
    }

    public void updateLayoutView(){
        //새로고침 시 실행할 내용 작성
        ...
    }
}
```

<br>

## TODO
실제 디바이스에 설치

<br>

---

# References
* <https://developer.android.com/training/swipe/add-swipe-interface?hl=ko#AddRefreshAction>
* <https://infos.tistory.com/4004> 
* <https://stickode.tistory.com/20>
* <https://kobbi-reply.tistory.com/28>