# Singleton Pattern
> 어떤 클래스의 인스턴스가 하나임을 보장하기 위해 사용하는 패턴


일반적인 Singleton Pattern 구현 방법은 아래와 같다.

<pre>
<code>
public class GeneralSingleton {
	// 자신의 instance를 가지고 있는 변수
	private static GeneralSingleton instance = new GeneralSingleton();

	// new 키워드를 사용하지 못하도록 private 생성자 사용
	private GeneralSingleton() {
	}

	// GeneralSingleton 객체는 getInstance 메소드를 통해 접근 가능
	public static GeneralSingleton getInstance() {
		return instance;
	}
}
</code>
</pre>

위와 같은 방법은 아래와 같은 문제점이 있다.
  + 큰 규모의 프로그램에서 효율적이지않다.
  + 클래스 Load 시점에서 객체가 생성되기 때문에 부담스러울수있다.
  + 인스턴스를 생성하는 과정에서 발생하는 어떠한 에러도 처리할수가 없다.

아래는 위의 Singleton을 조금 개선한 코드이다.
<pre>
<code>
public class StaticBlockSingleton {
	private static StaticBlockSingleton instance;

	private StaticBlockSingleton() {
	}

	// static 블록을 이용한 Instance 생성 과정에서
	// 발생 가능한 에러에 대해 예외 처리
	static {
		try {
			instance = new StaticBlockSingleton();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	public static StaticBlockSingleton getInstance() {
		return instance;
	}
}
</code>
</pre>

이전 GeneralSingleton 클래스보다 객체 생성과정에서 예외를 처리 할 수 있다는 점에서 개선이 되었다.

하지만, 여전히 Instance가 클래스 Load 시점에서 생성되고있다.

아래는 Singleton Instance를 클래스 Load 시점이 아닌 사용시점에서 생성되도록 수정하였다.
<pre>
<code>
public class LazySingleton {
	private static LazySingleton instance;

	private LazySingleton() {
	}

	public static LazySingleton getInstance() {
		// 생성 시점에서 Instance를 생성하도록 처리
		if (instance == null) {
			instance = new LazySingleton();
		}

		// 이미 생성된 Instance가 있다면 반환
		return instance;
	}
}

</code>
</pre>

위 방법은 사용 시점에 Singleton Instance를 생성하므로 메모리 적재 시점에 대한 부담이 줄게된다.

하지만, 위의 클래스는 MultiThread 환경에서 안전하지않다.
다수의 Thread가 동일 시점에 getInstance()를 호출하면 인스턴스가 두번 생길 위험이 존재한다.

아래는 synchronized 키워드를 통해 getInstance()를 동기화하여 MultiThread로부터 보호될 수 있도록 수정하였다.
<pre>
<code>
public class SynchronizedSingleton {
	private static SynchronizedSingleton instance;

	private SynchronizedSingleton() {
	}

	// synchronized 키워드를 통한 동기화
	public static synchronized SynchronizedSingleton getInstance() {
		if (instance == null) {
			instance = new SynchronizedSingleton();
		}

		return instance;
	}
}
</code>
</pre>
위의 코드를 통해 동시 접근으로인한 Instance가 여러번 생길 위험은 사라졌으나, 수많은 Thread들이 getInstance()를 호출하게되면 높은 cost 비용으로인해 프로그램 전반에 성능저하가 발생한다.

아래는 미국 메릴랜드 대학 연구원인 Bill pugh가 위와 같은 Singleton이 가진 문제를 해결하고자 제안한 방법이다.

Initialization on demand holder idiom라는 기법이며, jvm의 class loader 매커니즘과 class의 load 시점을 이용하여 내부 class를 생성시킴으로써, Thread간의 동기화 문제를 해결하였다.

<pre>
<code>
public class InitializationOnDemandHolderIdiom {
	private InitializationOnDemandHolderIdiom() {
	}

	// Inner Class에서 InitializationOnDemandHolderIdiom 객체 생성
	private static class Singleton {
		private static final InitializationOnDemandHolderIdiom instance = new InitializationOnDemandHolderIdiom();
	}

	public static InitializationOnDemandHolderIdiom getInstance() {
		return Singleton.instance;
	}
}
</code>
</pre>

이외에도, Enum Class를 활용하여 Singleton 구현도 가능하다. Enum을 통해 구현된 Singleton은 아래와 같은 장점을 가진다.

+ Instance가 생성될 때, MultiThread로부터 안전하다. (추가된 method는 안전하지 않을 수 있다.)
+ 단 한번의 Instance 생성을 보장한다.
+ 사용이 간편하다.
+ enum value는 자바 프로그램에서 전역 접근이 가능하다.

Enum Class로 구현된 Singleton Class는 아래와 같다.

<pre>
<code>
public enum EnumSingleton {
	INSTANCE;

	public static EnumSingleton getInstance() {
		return EnumSingleton.INSTANCE;

	}
}
</code>
</pre>

이처럼 다양한 방법으로 Singleton Class 구현이 가능하다.
