# AppStudy 저장소
## 0. Java App programming
- Java를 이용한 App 프로그래밍에서 화면 구성 등의 정적인 요소는 activity_main.xml 등의 .xml파일에서 xml로 구성하며 기능 등의 동적인 요소는 MainActivity.java 등의 .java파일에서 java를 이용하여 구성한다.

  
## 1. TextView
```xml
<!--activity_main.xml-->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <TextView
        android:textAlignment="center"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="자바가 좀더 복잡한 느낌은 있네"/>

    <TextView
        android:textAlignment="center"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="흠..."/>

</LinearLayout>

```
- MainActivitty.java는 건드리지 않았다.
- 레이아웃 속성중 android:orientation에 대해서 = "horizontal" 인지 "vertical"인지에 따라 수평 배열, 수직 배열 조작가능(기본값은 horizontal)
- TextView의 속성 중 android:layout_width, android:layout_height에 대해서 "match_parent"와 "wrap_content" 두 가지로 설정 가능하며 이는 속해있는 레이아웃에 크기를 맞출건지(match_parent) 구성요소의 크기에 맞출건지("wrap_content")에 따라 설정하면 된다. 물론, 특정 크기를 입력하여 크기를 입력할 수 있다.


## 2. EditText & Button
```xml
<!--activity_main.xml-->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/et"
        android:layout_width="300dp"
        android:layout_height="wrap_content"
        android:hint="아이디를 입력하세요..."/>

    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="버튼"/>

</LinearLayout>
```
- android:id="@+id/{아이디}"를 통하여 .java를 통한 동적 기능 구현을 위하여 연결점을 설정할 수 있다.
- EditText는 텍스트를 입력하는 위젯이다. 속성 중 android:hint를 이용하여 EditText에 무엇을 입력해야 하는지 등을 보여줄 수 있다.


```java
public class MainActivity extends AppCompatActivity {

    EditText et;
    Button btn;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        et = findViewById(R.id.et);//activity_main.xml의 "et"라는 id를 가지는 EditText와 연결
        btn = findViewById(R.id.btn); // activity_main.xml의 "btn"이라는 id를 가지는 Button과 연결

        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                et.setText("자바 앱 스터디 EditText & Button");
            }
        });


    }
}
```
- 특정 위젯은 하나의 클래스로 이를 구현하고 기능을 구성하기 위해서는 객체를 만들어야한다. 위의 예제에서는 "EditText et; Button btn;"의 부분이다.
- 이러한 객체를 구현하고자하는 위젯의 id와 findViewById(R.id.{아이디})로 연결한다.
- 이때, EditText의 경우 .setText를 이용하여 입력된 Text를 설정할 수 있으며 Button은 .setOnClickListener(new View.OnClickListener(){기능})로 버튼이 눌렸을 때 작동할 기능을 만들 수 있다.


## 3. intent(화면전환)
```xml
<!--activity_main.xml-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:gravity="center">


    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="화면전환"/>

</LinearLayout>

```
- 이동 전 이동을 위한 버튼이 있는 페이지


```xml
<!--activity_sub.xml-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal"
    android:gravity="center"
    tools:context=".SubActivity">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textAlignment="center"
        android:textSize="30sp"
        android:text="이동 성공"/>
</LinearLayout>

```
- "화면전환" 버튼을 눌러 페이지 이동을 할 페이지로 "이동 성공" TextView를 띄워 두었다.


```java
//MainActivity.java
public class MainActivity extends AppCompatActivity {

    private Button btn;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        btn = findViewById(R.id.btn);
        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(MainActivity.this, SubActivity.class);
                startActivity(intent);//액티비티 이동

            }
        });
    }
}

```
- 버튼을 눌렀을 때, ___Intent___ ___intent___ ____=___ ___new___ ___Intent({현재___ ___class}.this,___ ___{이동할___ ___class}.class);___ 를 이용하여 페이지 이동 출발 페이지와 도착 페이지를 설정하며, ___startActivity(intent);___ 를 이용하여 페이지를 이동한다.

```java
//SubActivty.java
public class SubActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_sub);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });
    }
}

```
- 텍스트만 띄워 두어서 기능이 없다.



