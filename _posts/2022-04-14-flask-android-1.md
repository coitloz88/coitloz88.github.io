---
title: Android에서 Flask 서버 접속하기 1 - Emulator
date: 2022-04-14 14:30:00 +0900
categories: [Android, etc]
tags: [computer, android, flask, volley]     # TAG names should always be lowercase
---

Flask 로컬 서버에서 랜덤한 숫자를 발생시키고 해당 숫자를 안드로이드 에뮬레이터에서 받아오는 예제를 진행해보았다.  

최종 목표는 퍼블릭 서버에서, 실제 디바이스에 설치된 앱이 값을 받아오는 것.  

참고한 코드는 [여기](https://infos.tistory.com/4004).  

Android에서 volley.Response.Listener를 사용하여 응답이 왔을 때 이를 받아 처리를 등록했다.  

---

# [Volley](https://developer.android.com/training/volley?hl=ko)란?
Volley는 Android 앱의 네트워킹을 더 쉽고 빠르게 하는 HTTP 라이브러리이다.  
아직 나는 네트워크나 각종 웹 프레임워크에 관한 지식이 부족한 편인데, 공식 문서 및 각종 구글링을 통해 알게 된 내용을 정리해놓으려고 한다.

## Volley로 간단한 요청 보내기
[공식 문서](https://developer.android.com/training/volley/simple?hl=ko#java)에서는 `RequestQueue`를 만들고 `Request` 객체를 전달하여 Volley를 사용한다.  
`Volley.newRequestQueue` 메서드를 사용하여 `RequestQueue`를 편리하게 설정하고 요청을 전송하는 방법에 관해 코드 레벨로 간단하게 알아보자.

### (1) 인터넷 권한 추가
앱이 네트워크에 연결하기 위해 `AndroidManifest.xml`에 `android.permission.INTERNET` 권한을 추가해야 한다.  

### (2) newRequestQueue 사용
```java
    final TextView textView = (TextView) findViewById(R.id.text);
    // ...

    // RequestQueue를 초기화한다.
    RequestQueue queue = Volley.newRequestQueue(this);
    String url ="http://www.google.com";

    // 제공된 URL로부터 string response를 요청한다.
    StringRequest stringRequest = new StringRequest(Request.Method.GET, url,
                new Response.Listener<String>() {
        @Override
        public void onResponse(String response) {
            // Display the first 500 characters of the response string.
            textView.setText("Response is: "+ response.substring(0,500));
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            textView.setText("That didn't work!");
        }
    });

    // RequestQueue에 요청을 추가한다.
    queue.add(stringRequest);
```

---

# 실제 구현
코드는 [이 쪽](https://github.com/coitloz88/Flask-Android-link/tree/main/AndroidApp).  
다만 나는 [여기](https://infos.tistory.com/4004)의 코드를 참고하여 작성했기 때문에, RequestQueue는 이용하지 않았다.  

Flask 서버에서 발생시킨 난수를 key-value 형태의 .json 파일 형식으로 웹에 띄우면, 그 값을 Android Emulator로 가져와 화면의 textview에서 보여준다.  

---

## Flask(Jupyter notebook, python) 서버 part
jupyter notebook에서 실행했다.
```python
import flask
import random

app = flask.Flask(__name__)

@app.route('/')
def make_randomNumber():
    ranStr = "random number: " + str(random.randrange(1, 11)) # 1에서 10 사이의 랜덤한 숫자를 발생 시킨다
    return flask.jsonify({'tk':ranStr}), 200 ## json 형태로 넘김
    
if __name__ == '__main__':
    app.run(host = '0.0.0.0')
```

서버를 구동하고 있는 상태에서 안드로이드 앱을 실행하자.

---

## Android(Android studio, java) 어플리케이션 part

### (1-1) 권한 설정
`AndroidManifest.xml` 파일을 열어보자.  
그러면 보통 다음과 같이 적혀있다.

```xml
<? ... >
<manifest ... >
    <!-- 여기에 추가 권한을 작성한다. -->
    <application ... >
        <activity ... >
            ...
        </activity>
    </application>
</manifest>
```  
이제 <application> 위에 `<uses-permission android:name="android.permission.INTERNET"/>`을 작성해주자.

![AndroidManifest.xml 설정](/assets/img/posts-images/flask-android/capture-2022-04-14-134539.png){: width="70%"}{: .center}

### (1-2) dependency 추가
나는 자동으로 됐는데 혹시나 추가가 잘 되었는지 확인해보자.  
**build.grandle(:app)** 파일의 `dependencies` 부분에 아래의 내용이 잘 추가되었는지 확인해본다.
```
implementation 'com.android.volley:volley:1.2.1'
```

### (2) activity_main.xml에서, flask 서버로부터 내용을 받아와 해당 내용을 보여줄 textview를 하나 만들어 원하는 대로 이름을 설정한다. 나의 경우는 `tv_test_message`로 설정했다.

```xml
<TextView
    android:id="@+id/tv_test_message"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Empty Message"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toTopOf="parent" />
```

### (3) MainActivity.java에서 volley를 사용한 코드를 작성한다.

여기서 문제가 발생했었는데, Android Emulator에서 컴퓨터 로컬호스트에 어떤 URL을 통해 접속하는지 알 수 없었다. 이 때문에 여러 에러와 마주쳐야 했다... 우선 Flask 로컬 서버는 일단 컴퓨터에서 `127.0.0.1:5000`로 돌아간다는 점을 알아두고 가자.  

일단 구글에 '안드로이드 에뮬레이터 로컬 서버 접속'이라고 검색해보면 에뮬레이터에서 ipv4 주소(192.XXX...)로 접속해야 한다, 혹은 로컬 서버 주소(127.XXX...)를 입력해야 한다 이것저것 많이 나왔는데 일단 이 경우는 무조건 아래의 에러를 마주쳐야 했다.  

```
NetworkDispatcher.processRequest: Unhandled exception java.lang.RuntimeException: Bad URL ...
```

위의 오류는 [여기](https://developer.android.com/studio/run/emulator-networking.html)를 참고해서 해결했다.  
에뮬레이터의 각 인스턴스는 가상 라우터/방화벽 서비스 뒤에서 실행되어 개발 머신 네트워크 인터페이스 및 설정과 인터넷에서 격리된다. 에뮬레이션된 기기는 네트워크에서 개발 머신이나 다른 에뮬레이터 인스턴스를 볼 수 없다. 대신 이더넷을 통해 라우터/방화벽에 연결되어 있는 것만 볼 수 있다.  
각 인스턴스의 가상 라우터는 10.0.2/24 네트워크 주소 공간을 관리한다. 라우터가 관리하는 주소는 모두 10.0.2.*xx* 형식이다.

| 네트워크 주소 | 설명 |
|:----------|:----------:|
| 10.0.2.1 | 라우터/게이트웨이 주소 |
| 10.0.2.2 | 호스트 루프백 인터페이스의 특수 별칭(개발 머신의 127.0.0.1) |
| 10.0.2.3 | 첫 번째 DNS 서버 |
| 10.0.2.4/10.0.2.5/10.0.2.6 | 두 번째, 세 번째, 네 번째 DNS 서버(선택사항, 있는 경우) |
| 10.0.2.15 | 에뮬레이션된 기기 네트워크/이더넷 인터페이스 |
| 127.0.0.1 | 에뮬레이션된 기기 루프백 인터페이스 |  

즉, 에뮬레이터에서 로컬 Flask 서버에 접속하기 위해서는 URL에 `10.0.2.2:XXXX`를 입력해야 한다.  

그러나 또다른 에러를 마주쳤다.  

```
code 400, message Bad request version
```  

Jupyter Notebook에서는 다음과 같은 에러를 출력했다.  
![Bad request 400](/assets/img/posts-images/flask-android/capture-2022-04-14-143112.png){: .center}

에뮬레이터 크롬에서 `https://10.0.2.2:5000`를 접속했을 때는 이제 잘 출력돼서 뭐가 문제인건지 또 골치아팠는데 간단하게 해결됐다.  
위의 오류는 `URL`을 `https://10.0.2.2:5000`에서 `http://10.0.2.2:5000`로 변경해주었더니 해결되었다.  

최종 코드는 다음과 같다.

```java
package com.example.flaskservertest;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.TextView;
import android.widget.Toast;

import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Response.Listener<String> rplsn = new Response.Listener<String>() {
            @Override
            public void onResponse(String response) {
                ((TextView) findViewById(R.id.tv_test_message)).setText(response.toString());
            }
        };

        Response.ErrorListener errlsn = new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                Toast.makeText(MainActivity.this, "Err ErrorListener:" + error.toString(), Toast.LENGTH_SHORT).show();
            }
        };

        //String URL = "https://jsonplaceholder.typicode.com/todos/1";
        String URL = "http://10.0.2.2:5000";
        StringRequest req = new StringRequest(Request.Method.GET, URL, rplsn, errlsn);
        RequestQueue rq = Volley.newRequestQueue(MainActivity.this);
        rq.add(req);

    }
}
```

위의 코드는 json 형식으로 파싱하지 않고 그냥 모든 string을 통으로 가져오는 예제이다.  
json 형식으로 파싱하는 예제는 곧 다시 업로드하려고 한다.  

일단은 위의 코드를 실행해서 에뮬레이터에 앱을 빌드하면 정상적으로 random number가 출력됨을 볼 수 있다.  

![Random Number 출력](/assets/img/posts-images/flask-android/capture-2022-04-14-142945.png){: width="50%"}{: .center}

### JSONPlaceholder - typicode
(3)번의 java 코드에서 URL에 주석 처리 된 페이지가 있는데, 무료로 가상 REST API를 제공해주는 사이트이다. 테스트 용도로 사용했다.  
로컬 서버에 접속하기 위해 URL을 `10.0.2. ...`로 설정해주었지만, 만약 실제 디바이스에서 실제 웹사이트에 접속해서 정보를 받아오려는 경우 주석처리 된 것처럼 URL에 원하는 주소를 입력해주면 된다.

## TODO
* 앱을 새로고침해서 Random Number를 출력하기
    - 현재는 종료 후 재접속 시 Random Number가 출력됨
* json 형태를 파싱해서 Number만 받아오기
* [AWS](https://aws.amazon.com/ko/free/) 알아보기
    - Github 학생 인증 시 AWS 이용이 무료라는 소리를 어디서 들은 것 같아서 같이 알아보기


# References
* <https://developer.android.com/training/volley?hl=ko>
* <https://developer.android.com/studio/run/emulator-networking.html>
* <https://infos.tistory.com/4004>
* <https://stackoverflow.com/questions/49389535/problems-with-flask-and-bad-request>