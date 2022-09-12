- 의존성 주입 (DI, Dependency Injection)
	- 하나의 객체에 다른 객체의 의존성을 제공하는 기술

- 의존성
	- 두 클래스간의 관계를 생각해보자 (둘 중 하나가 다른 하나를 필요로 하는 상황)
	- ex) 컴퓨터 클래스와 CPU 클래스가 있는데 CPU 클래스는 당연히 컴퓨터 클래스 안의 멤버여야 함
	- 아래 예시는 다른 CPU로 업그레이드 하고 싶어도 변경할 수가 없다. 
		- 이를 Computer가 CPU1에 의존성을 갖는다고 한다

- 자바 코드 주의
```
public class CPU1 {}

public class Computer{
	private CPU1 cpu;

	public Computer(){
		cpu = new CPU1();
	}

}
```
	
- 주입
	- 주입은 생성자나 메서드 등을 통해 외부로부터 생성된 객체를 전달받는 것을 의미
	- 아래 예제에서 setCPU 메서드를 통해 외부로부터 생성된 객체를 전달받아 멤버 변수에 넣고 있음

```
public class Computer{
	private CPU1 cpu;

	public void setCPU(CPU cpu){
		this.cpu = cpu;
	}

}
```

- 따라서 의존성과 주입의 개념을 합쳐서 정리해보자
	- 의존 관계에 있는 클래스의 객체를 외부로부터 생성하여 주입받는다
