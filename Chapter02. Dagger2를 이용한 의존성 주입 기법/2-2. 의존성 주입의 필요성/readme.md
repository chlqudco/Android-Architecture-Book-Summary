- 의존성 주입의 필요성에 대해 알아보며 의존성 주입에 대한 이해도를 높여보자

- 변경의 전이
	- 컴퓨터와 CPU 예시를 다시 생각해보자
	- 다른 타입의 CPU를 원한다고 가정
		- 그러면 기존 CPU 클래스 명과 CPU 클래스를 의존하던 컴퓨터 클래스도 같이 변경해야 한다
		- 결과적으로 하나의 클래스를 변경함으로써 다른 의존 관계까지 변경 사항이 전이된다
	- 이를 해결하기 위해 컴퓨터가 의존하는 CPU를 interface로 만들어보자
	- 그럼에도 아직 변경의 귀찮음이 남아있다

```
public interface CPU {}

public class A_CPU implements CPU {}

public class Computer{
	private CPU cpu;

	public Computer(){
		cpu = new A_CPU();
		//cpu = new I_CPU();
	}
}

```

- 제어의 역전 (IoC, Inversion of Control)
	- 제어의 역전은 어떠한 일을 수행하도록 만들어진 프레임워크에 제어권을 위임함으로써 관심사를 분리하는 것
	- 제어의 역전을 통해 앞의 문제를 해결할 수 있다.

```

public class Computer{
	private CPU cpu;

	public Computer(){}

	public Computer(Cpu cpu){
		this.cpu = cpu;
	}

	public void setCPU(CPU cpu){
		this.cpu = cpu;
	}
}

cpu는 메인함수에서 할당

```

- Computer 클래스의 생성자에서 CPU 객체를 만들지 않고 외부로부터 CPU 객체를 생성한 뒤 클래스에게 객체를 제공한다.
	- CPU 객체의 생성 및 관리를 외부에 위임했다.
		- 이를 제어의 역전이라고 한다
	- 제어의 역전을 통해 결합도를 약하게 만들었다
	- 컴퓨터 클래스는 이제 CPU를 뭐로 바꾸던 내부 필드나 메서드 매개변수를 변경하지 않아도 된다.

- 의존성 주입의 장점
	- 인터페이스 기반의 프로그래밍을 하여 코드를 유연하게 한다.
	- 주입하는 코드만 따로 변경하기 쉬워 리팩토링이 수월하다
	- 단위테스트를 하기가 더욱 쉬워진다.(가장 큰 장점)
	- 클래스 간의 결합도를 느슨하게 한다.
	- 여러 개발자가 서로 사용하는 클래스를 독립적으로 개발할 수 있다.

- 의존성 주입의 단점
	- 간단한 프로그램을 만들 때는 번거롭다.
	- 동작과 구성을 분리해서 코드를 추적하기 어렵게 하고 가독성을 떨어뜨릴 수 있다.
		- 따라서 개발자는 더 많은 파일을 참조해야 한다.
	- Dagger2 는 빌드에 시간이 조금 더 소요된다.

- 짧은 기간 개발하고 더는 유지보수 하지 않는 간단한 프로그램은 의존성 주입을 굳이 추천하지 않음
	- 의존성 주입을 하기 위해 인터페이스를 설계하고 프레임워크 설정이 생산성을 떨어뜨리기 때문.
	- 그러나 일반적인 앱은 생산성을 향상한다.
