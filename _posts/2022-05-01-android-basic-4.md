---
title: Android 기초 2.1 Activities and intents
date: 2022-05-01 15:15:00 +0900
categories: [Android, Fundamentals]
tags: [android, codelab]     # TAG names should always be lowercase
---

> 지난 글: [1.3 Text and scrolling views](https://coitloz88.github.io/posts/android-basic-3/)

---

# Activity

`Activity`란, 앱 이용자가 하나의 집중된 행위를 할 수 있는 단일 화면을 나타낸다. (예: 메일 보내기, 사진 찍기, 지도 보기)  
 
앱은 주로 여러 개의 화면으로 이루어져 있는데, 각각은 느슨하게 연결되어있다. 각 화면은 모두 하나의 Activity다.  

우리가 프로그래밍할 때와 비슷하게, 앱이 launch될 때 가장 먼저 실행되는 Activity는 `Main Activity`다. 처음 한번 실행된 이후로도 물론 다시 불릴 수도 있다.  

새로운 Activity가 시작되면, 이전 Activity는 back stack으로 분류되며 정지된다. stack의 기본 원리인 LIFO에 따라, 현재 Activity에서 작업을 마치고 back button으로 돌아가면 stack에서 activity가 pop되면서 destory되고, 이전 activity가 다시 실행된다.

# Intent

Activity는 *intent*와 함께 시작(활성화)된다. `Intent`란 Activity에서 다른 Activity 혹은 다른 앱 구성 요소로 어떤 액션 요청을 보낼 때 사용할 수 있는 비동기적 메시지이다. Intent를 사용하여 한 Activity에서 다른 Activity를 실행할 때, Activity 간에서 데이터를 넘길 수 있다.

* *Explicit intent*: intent의 target을 알고 있는 경우. 
* *Implicit intent*: target 구성 요소의 이름을 알고 있진 않지만, 수행해야 할 일반적인 액션을 가지고 있는 경우. (나중에 다시 알아볼 예정)

<br>

---

<br>

# Codelabs

1. Main Activity에서 특정 Text를 입력하고, Second Activity에 입력한 Text 데이터를 Intent로 넘겨준다.
2. Second Activity에서는 Main Activity로부터 받은 Text를 표시하고, Main Activity로 돌아갈때 다시 Reply Text 데이터를 Intent로 넘겨준다.

<br>

## 1. Create and launch another Activity: Intent 추가

layout과 Java 파일을 가진 새로운 Activity를 프로젝트에 추가하면, `AndroidManifest.xml`에서 해당 Activity의 `<activity>` 요소를 확인할 수 있다. main activity와 마찬가지로 `AppCompatActivity` 클래스를 상속받는다.  

앱의 각 Activity는 다른 Activity들과 느슨하게 연결되어 있다. 하지만, `AndroidManifest.xml` 파일에서 하나의 Activity를 다른 Activity의 parent activity로 지정가능하다. 이러한 parent-child 관계를 통해 Android에서는 각 Activity의 제목 표시줄에 왼쪽 화살표와 같은 탐색 힌트를 추가할 수 있다.  

### (1) Main Activity에 Button 추가하기

Main Activity의 java 파일에 `launchSecondActivity()`를 정의한다.  
Main Activity의 레이아웃 xml 파일에 버튼을 추가한다. 버튼의 xml 코드를 수정하여 버튼 클릭 시 `launchSecondActivity()`가 실행되도록 설정해준다.

```xml
<Button
        android:id="@+id/button_main"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="16dp"
        android:layout_marginRight="16dp"
        android:text="@string/button_main"
        android:onClick="launchSecondActivity"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintRight_toRightOf="parent" />
```

![Main Activity에 버튼 추가](https://developer.android.com/codelabs/android-training-create-an-activity/img/1ff9e83ac0bb3437.png?hl=ko){: width="43%"}{:. center}

### (2) Main Activity에 Intent 추가하기

`Main Activity`에 명시적인 `Intent`를 추가하고, 새로운 `Intent`를 메서드 인자로 사용하는 `startActivity()`를 호출하기 위해 `launchSecondActivity()`에 다음과 같은 내용을 추가한다.

```java
Intent intent = new Intent(this, SecondActivity.class);
startActivity(intent);
```

<br>

## 2. 서로 다른 Activity 간에 데이터 주고받기: Intent로 데이터 넘기기

서로 다른 Activity 간에 데이터를 주고받기 위해서는 (1)data field, (2)intent *extras*를 사용하는 2가지 방법이 있다. intent data는 작업할 특정 데이터를 나타내는 URI인데, intent를 통해 Activity에 전달하려는 정보가 URI가 아닌 경우, 혹은 전달하려는 정보가 두 개 이상인 경우, 추가 정보를 *extras*에 넣을 수 있다.

### (1) Main Activity에 EditText 추가하기

Second Activity에 intent *extras*로 넘겨줄 문자열 값을 EditText를 통해 입력받는다. 

### (2) Intent extras에 string 추가하기

`Intent` *extras*는 `Bundle`의 key-value 쌍이다. 다른 `Activity`로 정보를 보내기 위해서는 `Intent` extra `Bundle`에 key-value들을 넣어줘야 한다.

1. `Intent` extra의 key를 정의하기 위한 `public` 상수 추가

```java
public static final String EXTRA_MESSAGE = "com.example.android.twoactivities.extra.MESSAGE";
```

2. `EditText`를 연결헐 `private` 변수를 선언하고, 해당 변수를 `onCreate()`에서 `findViewByID()`로 찾아 연결한다.

3. `launchSecondActivity()`의 `Intent` 선언 아래에 `EditText`의 text 데이터를 받아올 수 있도록 코드를 추가한다.

![Main Activity에 EditText 추가](https://developer.android.com/codelabs/android-training-create-an-activity/img/cd5302e2709828b7.png?hl=ko){: width="43%"}{:. center}

4. 문자열을 `Intent` extra에 key-value 형태의 데이터로 추가한다.

```java
intent.putExtra(EXTRA_MESSAGE, message);
```

`MainActivity`의 각 메서드는 다음과 같이 정리된다.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mMessageEditText = findViewById(R.id.editText_main);
}

public void launchSecondActivity(View view) {
        Log.d(LOG_TAG, "Button clicked!");
        Intent intent = new Intent(this, SecondActivity.class);
        String message = mMessageEditText.getText().toString();
        intent.putExtra(EXTRA_MESSAGE, message);
        startActivity(intent);
}
```

### (3) Second Activity에서 메시지를 받을 TextView 추가하기

Main Activity에서 intent extra로 넘어온 문자열 데이터를 받아서 보여주기 위한 TextView를 추가해준다.

![Second Activity에 TextView 추가](https://developer.android.com/codelabs/android-training-create-an-activity/img/d505e47d07ddd850.png?hl=ko){: width="43%"}{:. center}

### (4) Get Extras and Display the message

1. SecondActivity.java의 `onCreate()`에서 Activity를 활성화하는 `Intent`를 받아온다.

```java
Intent intent = getIntent();
```

2. `MainActivity.EXTRA_MESSAGE` static 변수를 key로 사용하여 `Intent` extras로부터 메시지를 포함하는 문자열을 받아온다.

```java
String message = intent.getStringExtra(MainActivity.EXTRA_MESSAGE);
```

3. 레이아웃의 `TextView`를 `findViewByID()`를 사용하여 코드 내 변수와 연결한다.

4. `TextView`의 텍스트를 `Intent` extra로부터 받은 문자열로 설정한다.

```java
textView.setText(message);
```

`SecondActivity`의 메서드는 다음과 같이 정리된다.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_second);
    Intent intent = getIntent();
    String message = intent.getStringExtra(MainActivity.EXTRA_MESSAGE);
    TextView textView = findViewById(R.id.text_message);
    textView.setText(message);
}
```

<!--
TODO:
* Second Activity → Main Activity로 데이터 넘기기
* 데이터가 잘 넘어갔는지 RESULT 확인
* Activity 종료(finish())
-->

<br>

## 3. Main Activity에 Reply를 건네주며 돌아오기
Second Activity로부터 Main Activity로 데이터를 돌려주자(마찬가지로 intent *extras*를 이용한다.)  

### (1) Second Activity Layout에 EditText와 Button을 추가하기

이전 단계에서 했던 것처럼, Reply를 작성할 `EditText`와 전송을 Trigger할 `Button`을 `SecondActivity`의 Layout에 추가해준다.  

새로운 `activity_second.xml`은 다음과 같아질 것이다.

![Second Activity에 Reply 추가](https://developer.android.com/codelabs/android-training-create-an-activity/img/4acbc21712a8e6f7.png?hl=ko){: width="43%"}{:. center}

### (2) Second Activity에 Reponse Intent 추가하기

이전에 했던 것과 마찬가지로 진행하면 된다.

class의 상단에, `Intent` extra의 key를 정의하기 위한 public 변수를 설정한다.  
또한, `EditText`를 연결하기 위한 private 변수도 설정해준다.  

`onCreate()` 메서드에서는 `findViewByID()`로 `EditText`의 reference를 private 변수에 할당해준다.  

`returnReply()` 메서드에서는 `EditText`의 text를 설정하는 코드와, Reply를 위한 새로운 `Intent`를 생성하는 코드를 작성해준다. 이때, 이전에 사용했던 Intent를 사용하면 절대 안된다.  

`EditText`로부터 받은 `reply` 문자열을 새로운 intent의 `Intent` *extra*에 담아준다.

```java
replyIntent.putExtra(EXTRA_REPLY, reply);
```

응답이 성공적이었다는 것을 나타내도록 결과를 `RESULT_OK`로 설정한다. 
> `Activity` 클래스에서 result code를 정의하는데, `RESULT_OK`와 `RESULT_CANCELLED`로 나뉜다.

```java
setResult(RESULT_OK, replyIntent);
```

현재 `Activity`를 닫고 `MainActivity`로 돌아가기 위해 `finish()`를 호출해준다.

이전 단계를 포함하여, `SecondActivity`의 자바 코드는 다음과 같이 정리된다.

```java
public class SecondActivity extends AppCompatActivity {
    public static final String EXTRA_REPLY =
                         "com.example.android.twoactivities.extra.REPLY";
    private EditText mReply;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);
        mReply = findViewById(R.id.editText_second);
        Intent intent = getIntent();
        String message = intent.getStringExtra(MainActivity.EXTRA_MESSAGE);
        TextView textView = findViewById(R.id.text_message);
        textView.setText(message);
    }

    public void returnReply(View view) {
        String reply = mReply.getText().toString();
        Intent replyIntent = new Intent();
        replyIntent.putExtra(EXTRA_REPLY, reply);
        setResult(RESULT_OK, replyIntent);
        finish();
    }
}
```

### (3) Main Activity에 Reply를 표시할 TextView 추가하기

`SecondActivity`로부터 받은 reply를 MainActivity에 표시하기 위해, `activity_main.xml`에 `TextView` 구성 요소를 추가하자. 초기에는 보이지 않도록 설정하지 위해서는 `android:visibility`를 `invisible`로 설정해주면 된다.  

### (4) Main Activity에서 Intent extra Reply 받고 표시하기

다른 `Activity`를 시작하기 위해 명시적인 `Intent`를 사용했을 때, 만약 아무런 데이터도 넘기고 싶지 않다면 그냥 아예 이전 포스팅에서 해왔던 것처럼 `startActivity()`로 새로운 `Activity`를 시작해주면 된다. 그러나 이전에 활성화 되었던 `Activity`로부터 데이터를 받고 싶은 경우, `startActivityForResult()`로 해당 목표를 수행할 수 있다.  

1. **MainActivity**에서, 받고 싶은 특정 타입의 응답을 위한 key를 class 상단에 public 변수로 선언해준다.

```java
public static final int TEXT_REQUEST = 1;
```

2. Reply header와 `TextView`를 연결하기 위한 두 개의 private 변수를 선언한다.

```java
private TextView mReplyHeadTextView;
private TextView mReplyTextView;```
```

3. 앞서 2.에서 선언한 두 private 변수를 `onCreate()`메서드에서 `findViewByID()`로 실제 layout의 reply header와 reply `TextView`에 연결해준다.


이제 MainActivity의 `onCreate()`는 이전 단계를 포함하여 다음과 같이 정리될 것이다.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mMessageEditText = findViewById(R.id.editText_main);
        mReplyHeadTextView = findViewById(R.id.text_header_reply);
        mReplyTextView = findViewById(R.id.text_message_reply);
}
```

4. 이제 `launchSecondActivity()`를 수정해주자. 기존의 `startActivity()`를 `TEXT_REQUEST` key를 포함하는 `startActivityForResult()`로 변경해주자.

```java
startActivityForResult(intent, TEXT_REQUEST);
```

5. 콜백 메서드 `onActivityResult()`를 오버라이드한다.

```java
@Override
public void onActivityResult(int requestCode, 
                             int resultCode, Intent data) {
}
```
세 개의 변수는 데이터를 리턴하는 동작을 수행하기 위한 모든 정보를 포함한다.
        * `requestCode`: `startActivityForResult()`로 `Activity`를 실행했을 때 설정
        * `resultCode`: 실행된 `Activity`에서 설정됨(보통 `RESULT_OK`와 `RESULT_CANCELLED`로 나뉨)
        * `Intent data`: 실행된 `Activity`로부터 리턴된 데이터를 포함

6. `onActivityResult()`에 부모 생성자 `super.onActivityResult()`를 호출하자.

7. `Intent` 결과가 제대로 왔는지 확인하기 위해 `TEXT_REQUEST`를 테스트하는 코드를 추가하자.

```java
if (requestCode == TEXT_REQUEST) {
    if (resultCode == RESULT_OK) { 
    }
}
```

8. if 블럭 안에, `Intent` extra를 받는 코드를 추가해주자.

```java
String reply = data.getStringExtra(SecondActivity.EXTRA_REPLY);
```

9. 그리고 이제 응답을 보여줄 reply header와 `TextView` 보이도록 설정해주자.

```java
mReplyHeadTextView.setVisibility(View.VISIBLE);
mReplyTextView.setText(reply);
mReplyTextView.setVisibility(View.VISIBLE);
```

이제 전체 `onActivityResult()` 메서드는 다음과 같을 것이다.

```java
@Override
public void onActivityResult(int requestCode, 
                             int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == TEXT_REQUEST) {
        if (resultCode == RESULT_OK) {
            String reply = 
                    data.getStringExtra(SecondActivity.EXTRA_REPLY);
            mReplyHeadTextView.setVisibility(View.VISIBLE);
            mReplyTextView.setText(reply);
            mReplyTextView.setVisibility(View.VISIBLE);
        }
    }
}
```

앱을 실행한후 Second Activity에서 응답을 보내면 다음과 같이 Main Activity에서 해당 응답을 받을 수 있다.  

![Main Activity에서 Reply 받기](https://developer.android.com/codelabs/android-training-create-an-activity/img/e1c10fcdbf9a7e75.png?hl=ko){: width="43%"}{:. center}




<br>

---

<br>

# Image Source
* <https://developer.android.com/codelabs/android-training-create-an-activity/img/1ff9e83ac0bb3437.png?hl=ko>
* <https://developer.android.com/codelabs/android-training-create-an-activity/img/cd5302e2709828b7.png?hl=ko>
* <https://developer.android.com/codelabs/android-training-create-an-activity/img/d505e47d07ddd850.png?hl=ko>
* <https://developer.android.com/codelabs/android-training-create-an-activity/img/4acbc21712a8e6f7.png?hl=ko>
* <https://developer.android.com/codelabs/android-training-create-an-activity/img/e1c10fcdbf9a7e75.png?hl=ko>

# References
* <https://developer.android.com/courses/fundamentals-training/toc-v2?hl=ko>
