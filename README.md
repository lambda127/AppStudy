# AppStudy 저장소
## 0. Applicaton Study using Java language
- 유튜브 '홍드로이드'의 강의
- 링크 : <https://youtube.com/playlist?list=PLC51MBz7PMyyyR2l4gGBMFMMUfYmBkZxm&si=oiQYsLkzhqrTb5up>
- Java를 이용한 App 프로그래밍에서 화면 구성 등의 정적인 요소는 activity_main.xml 등의 .xml파일에서 xml로 구성하며 기능 등의 동적인 요소는 MainActivity.java 등의 .java파일에서 java를 이용하여 구성한다.


  
## 1. TextView

<img src="https://github.com/lambda127/AppStudy/assets/95059147/bc77424b-2f11-4ead-827e-e3e98ac86fd7" width="25%" height="25%" title="TextView"></img>


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

https://github.com/lambda127/AppStudy/assets/95059147/0d6720a5-4478-401c-b8ee-153531057f18


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


## 3. Intent(화면전환)

https://github.com/lambda127/AppStudy/assets/95059147/98ad3439-3304-40b2-b6c4-ad2809fe1681


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


https://github.com/lambda127/AppStudy/assets/95059147/e3b176a7-b4c3-4688-b9da-a317d6c1287c



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

<img src="https://github.com/lambda127/AppStudy/assets/95059147/dcabd32d-6945-483d-bfab-6684062014a1" width="20%" height="20%" title="ListView"></img>


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
- 5년 전 영상이라 그런지 지금 템플릿이랑 너무 달라서 설명을 어떻게 적어야 할지 모르곘다.


## 8. Shared Prefernece


https://github.com/user-attachments/assets/64bb445e-e6d0-44c7-915d-ba845434e67f


```xml
<!--activity_sub.xml-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/et_save"
        android:layout_width="100dp"
        android:layout_height="wrap_content"/>

</LinearLayout>

```


```java
//MainActivity.java

public class MainActivity extends AppCompatActivity {

    EditText et_save;
    String str = "file";

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

        et_save = (EditText)findViewById(R.id.et_save);

        SharedPreferences sharedPreferences = getSharedPreferences(str, 0); // 
        String value = sharedPreferences.getString("save", ""); //
        et_save.setText(value); //

    }

    @Override
    protected void onDestroy() { //앱이 종료되었을 때
        super.onDestroy();

        SharedPreferences sharedPreferences = getSharedPreferences(str, 0);
        SharedPreferences.Editor editor = sharedPreferences.edit(); // Shared Preference를 저장하기 위한 도구 = Editor 선언
        String value = et_save.getText().toString();

        editor.putString("save", value); // Shared Preference에 데이터 저장하는 것 "save"는 저장되는 데이터의 이름, value는 저장되는 데이터
        editor.commit(); // 저장

    }
}

```

- Shared Preference는 임시 저장으로 앱 삭제 시 함께 삭제된다.

## 9. Web View


https://github.com/user-attachments/assets/2e8ce523-45bc-4685-bcf4-7df94efab812


```xml
<!--activity_sub.xml-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        />

</LinearLayout>

```

- WebView 위젯 사용, 아이디를 통해 기능 사용


```java
//MainActivity.java

public class MainActivity extends AppCompatActivity {

    private WebView webView;
    private String url = "https://github.com/lambda127";

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

        webView = (WebView) findViewById(R.id.webview);
        webView.getSettings().setJavaScriptEnabled(true); // JavaScript 허용
        webView.loadUrl(url); //url 불러오기
        webView.setWebChromeClient(new WebChromeClient()); // Chrome Client 설정
        webView.setWebViewClient(new WebViewClient()); // 기본 WebClient 설정


    }

    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) { //뒤로가기 키를 눌렀을 때

        if((keyCode==KeyEvent.KEYCODE_BACK) && webView.canGoBack()){ // 뒤로가기 키를 누르고 WebView에서 뒤로갈 수 있을 때
            webView.goBack(); // 뒤로가기
            return true;
        }

        return super.onKeyDown(keyCode, event);
    }
}

```


```xml
<!--AndroidManifest.xml-->

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.INTERNET"/>  <!--인터넷 사용 허용-->

    <application
    android:usesCleartextTraffic="true" <!--안드로이드 9 버전이상부터 필요-->
    ...
    </application>

</manifest>

```
- 인터넷 사용 권한 허용 필요


## 10. Custom Navigation Menu

https://github.com/user-attachments/assets/cc27fbea-3a32-4341-b548-826d7df4f959


```xml
<!--activity_main.xml-->

<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"    <!--프로젝트 생성시엔 main임, 이를 변경할 경우에는 MainActivity.java의 이 layout과 이어지는 부분의 id 또한 변경되어야 함-->
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center">

        <Button
            android:id="@+id/btn_open"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="메뉴 열기" />

    </LinearLayout>
    <include layout="@layout/activity_drawer"/>  <!--drawer를 꾸미는 xml 파일의 layout을 이 파일의 layout에 포함시키는 것-->

</androidx.drawerlayout.widget.DrawerLayout>

```

```java
//MainActivity.java

public class MainActivity extends AppCompatActivity {

    private DrawerLayout drawerLayout; //drawer layout은 drawer가 표시 되기 이전의 화면이자 나올 화면
    private View drawerView; //drawer가 표시될 View

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.drawer_layout), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        drawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);
        drawerView = (View) findViewById(R.id.drawer);

        Button btn_open = (Button) findViewById(R.id.btn_open);
        btn_open.setOnClickListener(new View.OnClickListener() { //btn_open이라는 id를 가지는 버튼을 눌렀을 떄
            @Override
            public void onClick(View v) {
                drawerLayout.openDrawer(drawerView); //drawer 열기
            }
        });

        Button btn_close = (Button) findViewById(R.id.btn_close);
        btn_close.setOnClickListener(new View.OnClickListener() { //btn_close이라는 id를 가지는 버튼을 눌렀을 떄
            @Override
            public void onClick(View v) {
                drawerLayout.closeDrawers(); //drawer 닫기
            }
        });

        drawerLayout.setDrawerListener(listener); //drawer 불러오기에 대한 설정 세팅, listener에 그런 설정이 저장된다.
        drawerView.setOnTouchListener(new View.OnTouchListener() { //drawer 터치에 대한 반응을 가능하게 한다
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                return true;
            }
        });

    }

    DrawerLayout.DrawerListener listener = new DrawerLayout.DrawerListener() { // 메뉴(drawer)를 불러왔을 때 기능하는 것, 특별히 커스텀할 수 있다
        @Override
        public void onDrawerSlide(@NonNull View drawerView, float slideOffset) {

        }

        @Override
        public void onDrawerOpened(@NonNull View drawerView) {

        }

        @Override
        public void onDrawerClosed(@NonNull View drawerView) {

        }

        @Override
        public void onDrawerStateChanged(int newState) {

        }
    };
}

```


```xml
<!--activity_drawer-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="240dp"
    android:layout_height="match_parent"
    android:layout_gravity="start"
    android:background="#A67284ED"  <!--darwer 등의 widget/View의 가장 배경 색 설정-->
    android:id="@+id/drawer"
    android:orientation="vertical">

    <Button
        android:id="@+id/btn_close"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"  <!--margin : widget 내용 밖에 여백 설정-->
        android:text="메뉴 닫기"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="component"
        android:gravity="center"
        android:layout_margin="5dp"
        android:background="#6479EB"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="5dp">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:text="menu"/>

    </LinearLayout>
</LinearLayout>


```


## 11. Camera
- 촬영 버튼 클릭 -> 카메라 앱 -> 촬영 -> 앱 화면에 촬영한 사진 띄우기

```xml
<!--activity_drawer-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

   <LinearLayout
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:layout_weight="1">

       <ImageView
           android:id="@+id/img"
           android:layout_width="match_parent"
           android:layout_height="match_parent"/>

   </LinearLayout>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:gravity="center">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/btn"
            android:text="촬영"/>

    </LinearLayout>


</LinearLayout>

```


```java
//MainActivity.java

public class MainActivity extends AppCompatActivity {

    private static final int REQUEST_IMAGE_CAPTURE = 672;
    private String imageFilePath;
    private Uri photoUri;


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


        //권한 체크
        TedPermission.with(getApplicationContext())
                .setPermissionListener(permissionListener)
                .setRationaleMessage("카메라 권한이 필요합니다.")
                .setDeniedMessage("거부하셨습니다.")
                .setPermissions(Manifest.permission.CAMERA)
                .check();



        findViewById(R.id.btn).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE); // 이미지 촬영 앱을 띄우는 구문
                if(intent.resolveActivity(getPackageManager()) != null){
                    File photoFile = null;
                    try{
                        photoFile = createImageFile();
                    }catch (IOException e){

                    }

                    if(photoFile != null){
                        photoUri = FileProvider.getUriForFile(getApplicationContext(), getPackageName(),photoFile);
                        intent.putExtra(MediaStore.EXTRA_OUTPUT, photoUri); // 화면을 띄움
                        startActivityResult.launch(intent);  //startActivityForResult(intent, REQUEST_IMAGE_CAPTURE);가 Deprecated 되었기 때문에 다른 방식으로 구현
                        // 다음 intent로 화면 이동을 하고 다시 돌아올 때 갔었던 화면에서 데이터를 불러오는 기능을 가짐
                    }
                }
            }
        });
    }

    private File createImageFile() throws IOException{
        String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date()); //시간을 형식에 맞춰서 저장
        String imageFileName = "TEST_" + timeStamp+"_"; //파일이름 형식 설정 TEST_(시간)_
        File storageDir = getExternalFilesDir(Environment.DIRECTORY_PICTURES);
        File image = File.createTempFile(
                imageFileName,
                ".jpg", //압축 형식
                storageDir
        );  // 지정한 형식에 맞게 파일 이름, 파일 압축 방식 지정, 저장 위치 지정 및 저장
        imageFilePath = image.getAbsolutePath();
        return image;

    }

    ActivityResultLauncher<Intent> startActivityResult = registerForActivityResult( //startactivityForResult()를 대체하는 method
            new ActivityResultContracts.StartActivityForResult(),
            new ActivityResultCallback<ActivityResult>() {
                @Override
                public void onActivityResult(ActivityResult result) {

                    if (result.getResultCode() == Activity.RESULT_OK) {
                        Bitmap bitmap = BitmapFactory.decodeFile(imageFilePath);
                        ExifInterface exif = null;

                        try {
                            exif = new ExifInterface(imageFilePath);
                        } catch (IOException e) {
                            e.printStackTrace();
                        }

                        int exifOrientation;
                        int exifDegree;

                        if (exif != null) {
                            exifOrientation = exif.getAttributeInt(ExifInterface.TAG_ORIENTATION, ExifInterface.ORIENTATION_NORMAL);
                            exifDegree = exifOrientationToDegress(exifOrientation); //돌아간 각도 불러오기
                        } else {
                            exifDegree = 0;
                        }

                        ((ImageView) findViewById(R.id.img)).setImageBitmap(rotate(bitmap, exifDegree)); //사진이 돌아간 정도에 따라 사진을 돌린 후 ImageView에 표시

                    }

                }


            });
              /* // 강의에서는 아래의 주석 처리된 구문이었으나 위의 startActivityForResult를 대체하는 methid에 포함되었음
                @Override
                protected void onActivityResult (int requestCode, int resultCode, Intent data) { //찍은 사진을 다른 화면으로 가면서 보내는 구문

                    if (requestCode == REQUEST_IMAGE_CAPTURE && resultCode == RESULT_OK) {
                        Bitmap bitmap = BitmapFactory.decodeFile(imageFilePath);
                        ExifInterface exif = null;

                        try {
                            exif = new ExifInterface(imageFilePath);
                        } catch (IOException e) {
                            e.printStackTrace();
                        }

                        int exifOrientation;
                        int exifDegree;

                        if (exif != null) {
                            exifOrientation = exif.getAttributeInt(ExifInterface.TAG_ORIENTATION, ExifInterface.ORIENTATION_NORMAL);
                            exifDegree = exifOrientationToDegress(exifOrientation);
                        } else {
                            exifDegree = 0;
                        }

                        ((ImageView) findViewById(R.id.img)).setImageBitmap(rotate(bitmap, exifDegree));

                    }

                }
     */

    private int exifOrientationToDegress(int exifOrientation) {// 촬영한 이미지의 돌아간 정도를 반환
        if (exifOrientation == ExifInterface.ORIENTATION_ROTATE_90){
            return 90;
        }else if (exifOrientation == ExifInterface.ORIENTATION_ROTATE_180){
            return 180;
        }else if (exifOrientation == ExifInterface.ORIENTATION_ROTATE_270){
            return 270;
        }
        return 0;
    }

    private Bitmap rotate(Bitmap bitmap, float degree) { //촬영 시 돌아간 정도를 받아 돌려서 반환
        Matrix matrix = new Matrix();
        matrix.postRotate(degree);
        return Bitmap.createBitmap(bitmap, 0,0, bitmap.getWidth(), bitmap.getHeight(), matrix, true);
    }




    PermissionListener permissionListener = new PermissionListener(){ // 권한 허용 및 거부에 따른 토스트 메시지
        @Override
        public void onPermissionGranted(){
            Toast.makeText(getApplicationContext(), "권한이 허용됨", Toast.LENGTH_SHORT).show();
        }

        @Override
        public void onPermissionDenied(List<String> deniedPermissions) {
            Toast.makeText(getApplicationContext(), "권한이 거부됨", Toast.LENGTH_SHORT).show();
        }
    };

}

```

```kotlin
//build.gradle.kts (:app)

...

dependencies {
     ...

    implementation("io.github.ParkSangGwon:tedpermission:2.3.0") // tedpermission 추가, tedpermission : 권한 허용에 대한 메시지를 더 쉽게 꾸밀 수 있게 함
    
    ...
}


```


```xml
<!--AndroidManifest.xml-->

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-feature
        android:name="android.hardware.camera"
        android:required="false" />

    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>

    <application
        ...  >
        <activity
            ...
        </activity>

        <provider
            android:authorities="com.example.myapplication"
            android:name="androidx.core.content.FileProvider"
            android:exported="false"
            android:grantUriPermissions="true">

            <meta-data
                android:name="androidx.core.content.FileProvider"
                android:resource="@xml/file_paths" />

        </provider>

    </application>

</manifest>

```

```xml
<!--res/xml/file_paths.xml : 추가로 생성한 파일, AndroidManifest에서 사용-->

<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <external-path
        name = "my_image"
        path = "Android/data/com.example.myapplication/files/Pictures"/>
</paths>

```

- 앱 실행은 됨. 다만 권한 허용에 대한 팝업이나 토스트가 실행시 마다 뜸, 또한 카메라 앱으로 이동이 되지 않음.
- 버전에 따른 변화는 https://kne-coding.tistory.com/163 를 참고하여 수정하였음


## 12. RecyclerView

https://github.com/user-attachments/assets/879a41e0-3203-4845-b5dd-b7f8327845da


- Item을 계속 생성할 수 있는 lsit View의 일종
- 버전이 달라서 생긴 변화는 https://kne-coding.tistory.com/170 를 참고하여 수정하였음

```xml
<!--activity_main.xml-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/rv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbarFadeDuration="0"
        android:scrollbarSize="5dp"
        android:scrollbarThumbVertical="@android:color/darker_gray"
        android:scrollbars="vertical"
        android:layout_weight="1">


    </androidx.recyclerview.widget.RecyclerView>


    <Button
        android:id="@+id/btn_add"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="추가"/>

</LinearLayout>

```

```xml
<!--item_list.xml : RecyclerView의 리스트에 들어갈 아이템의 형식.-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <ImageView
            android:id="@+id/iv_profile"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@mipmap/ic_launcher"/>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical"
            android:gravity="center">

            <TextView
                android:id="@+id/tv_name"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_margin="5dp"
                android:text="이름입니다."/>

            <TextView
                android:id="@+id/tv_content"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_margin="5dp"
                android:text="내용입니다."/>

        </LinearLayout>

    </LinearLayout>

</LinearLayout>

```

```java
//MainActivity.java

public class MainActivity extends AppCompatActivity {

    private ArrayList<MainData> arrayList ;
    private MainAdapter mainAdapter;


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


        RecyclerView recyclerView = (RecyclerView) findViewById(R.id.rv);
        LinearLayoutManager linearLayoutManager = new LinearLayoutManager(this);
        recyclerView.setLayoutManager(linearLayoutManager);


        arrayList = new ArrayList<>();

        mainAdapter = new MainAdapter(arrayList);
        recyclerView.setAdapter(mainAdapter);


        Button btn_add = (Button) findViewById(R.id.btn_add);
        btn_add.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                MainData mainData = new MainData(R.mipmap.ic_launcher, "이름입니다.", "내용입니다.");
                arrayList.add(mainData);
                mainAdapter.notifyDataSetChanged(); //변경 알림 -> 새로 고침

            }
        });


    }
}

```


```java
//MainData..java : 사용되는 데이터들을 저장해두고 얻어 올 수 있게 하는 Class

public class MainData {

    private int iv_profile;
    private String tv_name;
    private String tv_content;

    public MainData(int iv_profile, String tv_name, String tv_content) {
        this.iv_profile = iv_profile;
        this.tv_name = tv_name;
        this.tv_content = tv_content;
    }

    public int getIv_profile() {
        return iv_profile;
    }

    public void setIv_profile(int iv_profile) {
        this.iv_profile = iv_profile;
    }

    public String getTv_name() {
        return tv_name;
    }

    public void setTv_name(String tv_name) {
        this.tv_name = tv_name;
    }

    public String getTv_content() {
        return tv_content;
    }

    public void setTv_content(String tv_content) {
        this.tv_content = tv_content;
    }
}

```


```java
//MainAdapter.java

public class MainAdapter extends RecyclerView.Adapter<MainAdapter.CustomViewHolder> {

    private ArrayList<MainData> arrayList;


    public MainAdapter(ArrayList<MainData> arrayList) {
        this.arrayList = arrayList;
    }

    //@NonNull
    @androidx.annotation.NonNull
    @Override
    public MainAdapter.CustomViewHolder onCreateViewHolder(/*@NonNull*/@androidx.annotation.NonNull ViewGroup viewGroup, int i) {

        View view = LayoutInflater.from(viewGroup.getContext()).inflate(R.layout.item_list, viewGroup, false);
        CustomViewHolder holder = new CustomViewHolder(view);


        return holder;
    }

    @Override
    public void onBindViewHolder(/*@NonNull*/ @androidx.annotation.NonNull MainAdapter.CustomViewHolder customViewHolder, int position) {

        customViewHolder.iv_profile.setImageResource(arrayList.get(position).getIv_profile());
        customViewHolder.tv_name.setText(arrayList.get(position).getTv_name());
        customViewHolder.tv_content.setText(arrayList.get(position).getTv_content());

        customViewHolder.itemView.setTag(position);
        customViewHolder.itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String curName = customViewHolder.tv_name.getText().toString();
                Toast.makeText(v.getContext(), curName, Toast.LENGTH_SHORT).show();
            }
        });

        customViewHolder.itemView.setOnLongClickListener(new View.OnLongClickListener() {
            @Override
            public boolean onLongClick(View v) {

                remove(customViewHolder.getAdapterPosition());

                return true;
            }
        });

    }

    @Override
    public int getItemCount() {
        return (null != arrayList ? arrayList.size() : 0);
    }

    public void remove(int position){
        try{
            arrayList.remove(position);
            notifyItemRemoved(position);

        }catch (IndexOutOfBoundsException ex){
            ex.printStackTrace();
        }
    }

    public class CustomViewHolder extends RecyclerView.ViewHolder {

        protected ImageView iv_profile;
        protected TextView tv_name;
        protected TextView tv_content;


        public CustomViewHolder(/*@NonNull*/ @androidx.annotation.NonNull View itemView) {
            super(itemView);

            this.iv_profile = (ImageView) itemView.findViewById(R.id.iv_profile);
            this.tv_name = (TextView) itemView.findViewById(R.id.tv_name);
            this.tv_content = (TextView) itemView.findViewById(R.id.tv_content);

        }
    }
}

```


```kotlin
//build.gradle.kts (:app)

...

dependencies {
     ...

    implementation("com.android.support:recyclerview-v7:28.0.0")
    
    ...
}


```




