- 데이터 바인딩
	- 명령형 방식이 아닌 선언적 형식으로 레이아웃의 UI 구성 요소를 앱의 데이터와 결합할 수 있는 라이브러리
	
- 선언형 프로그래밍이란 문제에 대한 답을 정의하기보다 문제를 설명하는 것임
	- 명령형은 어떤 방법에 중점을 두는 반면 선언형은 무엇을 할지에 중점을 둠. 뭔소리?

- 명령형 프로그래밍과 선언형 프로그래밍의 텍스트 변경 예제

```
TextView textView = findViewById(R.id.textView);
textView.setText(viewModel.getUserName());

<TextView
	android:text = "@{viewmodel.userName}"/>

```

- 데이터 바인딩을 사용하면 코드를 작성하지 않고 레이아웃 파일에서 직접 데이터를 변경할 수 있다.
	- 즉, 많은 보일러 플레이트 코드를 줄일 수 있다.
	- 또한 findViewById를 호출할 필요가 없어 앱 성능이 향상되고 메모리 누수 및 NPE를 방지할 수 있다

- 데이터 바인딩 설정
	- 4.0이상 기기에서 지원한다.
	- build.gradle에 dataBinding을 설정해주자

- 바인딩 클래스 생성
	- 데이터 바인딩 라이브러리는 바인딩 클래스를 생성한다.
	- 모든 바인딩 클래스는 ViewDataBinding을 상속한다.
	- xml 레이아웃 파일에서 최상위 레이아웃을 <layout> 태그로 감싸면 자동으로 생성된다.
	- 바인딩 클래스의 이름은 레이아웃 파일의 이름을 기반으로 결정된다
		- ex) activity_main.xml -> ActivityMainBinding
	- 레이아웃에 대한 표현은 ~Binding 클래스로 작성되지만, 실제 비즈니스 로직을 추적 하려면 ~BindingImpl을 참조해야 함

- 바인딩 객체 생성하기
	- 일반적으로 바인딩 클래스의 static 메서드를 이용한다.
		- val binding = ActivityMainBinding.inflate(layoutInflater);
	- 레이아웃을 전개하지 않고 전개 후에 바인딩 한다면 bind() 메소드를 사용한다.
		- 먼소리?
		- val binding = ActivityMainBinding.bind(rootView)
	- 바인딩 클래스 이름을 미리 알지 못하는 경우의 방법도 있다.
		- 미리 모르는 경우가 어디있지?
		- val binding = DataBindingUtil.inflate(layoutInflater, R.layout.activity_main, parent, attachToParent)
		- val binding = DataBindingUtil.bind(rootView)
		- DataBindingUtil에는 setContentView 메서드도 있어서 이걸 써도 된다
		- binding = DataBindingUtil.setContentView(this, R.layout.activity_main)

- 바인딩 클래스 이름 사용자화하기
	- 바인딩 클래스 이름을 내맘대로 바꿀 수 있다.
	- <data> 태그 내에 calss 속성으로 지정하면 된다.
		- <data class = "abcabd">
	- 다른 패키지에 저장하려면 이름 앞에 점을 찍으면 된다
	- 전체 패키지 명을 지정해도 괜찮다.

- ID로 View 참조하기
	- 바인딩 클래스를 사용하면 findViewById를 호출할 필요가 없다.	
	- 바인딩 클래스 내부에서 미리 findViewById를 호출한 결과를 캐싱해두기 때문.
	- 따라서 binding.textView1 처럼 접근할 수 있다.

- 레이아웃에 변수 선언하기
	- 데이터바인딩을 사용하면 레이아웃에 id를 선언하고 뷰에 꼭 접근할 필요가 없어진다
	- 레이아웃에 변수를 선언하고 변수에 값을 대입하는 것으로 뷰의 상태를 변경할 수 있다.
	- layout 태그 안에 data 태그를 사용해야 한다
	- data 태그 안에 선언하고 싶은 변수를 <variable> 태그를 사용하여 선언하면 된다.
	- <variable> 태그는 여러개 선언할 수 있고 name과 type 속성을 갖는다
		- name은 변수의 이름, type은 변수의 자료형
	- 선언한 변수를 텍스트 뷰에 바인딩하려면 @{변수명}을 사용하면 된다.
	- 바인딩 클래스에서는 레이아웃에 선언한 변수명에 set접두어로 붙은 메서드로 값을 대입할 수 있다
	- 클래스 형식도 참조 가능하다.

```

<layout>
	<data>
		<variable
			name="myText"
			type="String"/>
	</data>

	<TextView
		android:text="@{myText}"

</layout>

대입

binding.setMyText("Hello World");

```
 
---

- 바인딩 표현식 알아보기
	- 바인딩 표현식에는 꽤 많은 문법이 존재한다.
	- 여러 연산자와 키워드를 xml 레이아웃에서 사용할 수 있다.
	- ex) android:text = "@{String.valueOf(index + 1)}"
	- ex) android:visibility = "@{ age > 13 ? View.GONE : View.VISIBLE }"

- Null 병합 연산자
	- 왼쪽의 피연산자부자 null 이라면 오른쪽 피연산자부를 선택하도록 하는 기능
	- ex) android:text = "@{user.displayName ?? user.lastName}"

- NPE 회피하기
	- 자동으로 생성된 바인딩 클래스는 자동으로 null을 검사하고 NPE를 회피한다
	- 만약 표현식이 null 이라면 null로 배치되고 int값의 기본값으로 0을 사용한다.

- 안드로이드 리소스 참조하기
	- ex) android:padding = "${large? @dimen/largePadding : @dimen/smallPadding}"

- import 사용하기
	- data 태그 내에 import 태그를 사용하여 참조하고 싶은 클래스를 레이아웃 파일에 간단히 불러올 수 있다.

```

<data>
	<import type="android.view.View"/>
</data>

<TextView
	android:visibility="@{user.isAdult ? View.VISIBLE : View.GONE}"/>

```

- 위 예제에서 View 클래스를 참조했으므로 View.VISIBLE 같은 상수를 참조할 수 있게 되었다.
- 만약 같은 이름을 갖는 클래스 두개 이상을 참조해야 하는 경우 클래스의 이름을 변경하여 참조할 수 있다
	- alias 속성으로 바꿀 수 있다.
	-

- include 사용하기
	- 레이아웃 파일 내에서 다른 레이아웃을 포함하는 경우 include 태그를 사용한다.
	- 좀 어렵네?

```

<data>
	<variable
		name="user"
		type="com.chlqudco.User" />
</data>

<LinearLayout>
	<include layout = "@layout/contact"
		app:user="@{user}"/>

</LinearLayout>
```

---

- 이벤트 처리하기
	
- 중도 포기
