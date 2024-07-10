# AppStudy 저장소
## 0. Applicaton Study using Java language
- 유튜브 '홍드로이드'의 강의를 들으며 공부하였다. <https://youtube.com/playlist?list=PLC51MBz7PMyyyR2l4gGBMFMMUfYmBkZxm&si=oiQYsLkzhqrTb5up>
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
        android:id="@+id/tv_sub"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textAlignment="center"
        android:textSize="30sp"
        android:text="이동 성공"/>
</LinearLayout>

```
- "화면전환" 버튼을 눌러 페이지 이동을 할 페이지로 "이동 성공" TextView를 띄워 두었다. 이동 과정에서 문자열 데이터가 전송되어 해당 문자열이 띄워질 예정이다.


```java
//MainActivity.java
public class MainActivity extends AppCompatActivity {

    private Button btn;
    private EditText et;
    String str;

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

        et = findViewById(R.id.et);

        btn = findViewById(R.id.btn);
        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                str = et.getText().toString();

                Intent intent = new Intent(MainActivity.this, SubActivity.class);
                intent.putExtra("str", str);
                startActivity(intent);//액티비티 이동

            }
        });
    }
}

```
- 버튼을 눌렀을 때, Inten intent = new Intent({현재 class}.this, {이동할 class}.class);를 이용하여 페이지 이동 출발 페이지와 도착 페이지를 설정하며, startActivity(intent);를 이용하여 페이지를 이동한다.
- str = et.getText().toString();으로 EditText에 입력된 문자열을 가져와 String으로 변화하여 str에 저장한다. intent.putExtra("{정보 이름}", str);를 통해 페이지 이동 시 보낼 정보에 포함 시킨다.

```java
//SubActivty.java
public class SubActivity extends AppCompatActivity {

    private TextView tv_sub;

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

        tv_sub = findViewById(R.id.tv_sub);

        Intent intent = getIntent();
        String str = intent.getStringExtra("str");


        tv_sub.setText(str);
    }
}

```
- MainActivity에서 보낸 문자열 데이터를 받아 TextView에 띄운다. Intent intent = getIntent();를 통해 페이지 이동 시에 함께 전달된 정보를 가져온다. 또한 String str = intent.getStringExtra("{정보 이름}");를 통해 변수에 저장, tv_sub.setText(str);로 페이지의 TextView에 가져온 문자열을 띄운다.


## 4. ImageView & Toast
```xml
<!--activity_sub.xml-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:gravity="center"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <ImageView
        android:id="@+id/image"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:src="@mipmap/ic_launcher"/>


</LinearLayout>

```
- android:src="@{경로}"를 통해 ImageView에 띄울 이미지를 지정한다.

```java
//MainActivity.java

public class MainActivity extends AppCompatActivity {

    private ImageView image;

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

        image = (ImageView)findViewById(R.id.image);

        image.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(getApplicationContext(), "ImageView And Toast", Toast.LENGTH_SHORT).show();
            }
        });
    }
}

```
- ImageView를 클릭헀을 때 Toast.makeText(getApplicationContext(), "{띄울 메시지}", Toast.LENGTH_SHORT).show();를 통해 토스트 메시지를 띄운다. Toast.LENGTH_SHORT, Toast.LENGTH_Long을 통해 토스트 메시지를 띄우는 지속시간을 정할 수 있다.

## 5. 패키지구조 & 역할
```xml
<!--AndroidManifest.xml-->

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.ImageViewToast"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>

```
- manifests
  -  android:label="@string/app_name" : app의 이름을 지정하는 부분이다. app의 이름을 지정해둔 파일을 연결하였다.
  - android:icon="@mipmap/ic_launcher", android:roundIcon="@mipmap/ic_launcher_round" : app 아이콘, 그냥 아이콘과 라운드 처리된 아이콘이다. 아이콘이 저장된 위치를 연결해 두었다.
  - android:theme="@style/Theme.ImageViewToast" : app의 색깔을 선언해둔 파일을 연결하였다.
  - <activity></activity> 부분은 Activity를 실행하기 위한 선언부이다.

- res
  - drawable : 주로 이미지 파일 저장
  - latout : 대부분의 layout 파일들 관리
  - mipmap : 아이콘 파일 저장
  - values 
    - colros.xml : 사용할 색깔 선언 해 둔 파일
    - strings.xml : 많이 사용하거나 그런 문자열 을 선언 해 둔 파일
    - styles.xml : 테마를 모아둔 파일(영상에는 있지만 현재 버전에는 없으며 themes라는 폴더로 바뀌었음)

## 6. ListView
```xml
<!--activity_sub.xml-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <ListView
        android:id="@+id/list"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

</LinearLayout>

```
- ListView의 아이템은 xml에서 선언 안함

```java
//MainActivity.java

public class MainActivity extends AppCompatActivity {

    private ListView list;

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

        list = (ListView) findViewById(R.id.list);

        List<String> data = new ArrayList<>();

        ArrayAdapter<String> adapter = new ArrayAdapter<>(this, android.R.layout.simple_list_item_1, data);
        list.setAdapter(adapter);

        data.add("이거");
        data.add("재밌다.");
        data.add("행복해");
        data.add("새로운 거 신나");
        data.add("좀 귀찮긴 해도");

        adapter.notifyDataSetChanged();

    }
}

```
- ArrayAdapter<String> adapter = new ArrayAdapter<>({class}, {ListView 형식}, {list});로 "data" list에 연결을 하기위한 Adapter를 선언한다.
- list.setAdapter(adapter);로 ListView와 "data" lsit를 연결한다.
- {list}.add("")를 이용하여 {list}에 데이터를 추가한다.
- {Adapter}.notifyDataSetChanged();로 변경된 데이터를 저장한다.


## 7. Navigation Menu

