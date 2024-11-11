#language/java 

@FunctionalInterface를 interface생성위치에 두면 해당 인터페이스는 함수형 인터페이스가된다.
``` java
@FunctionalInterface
interface TestInterface{
	void myMethod(String message);
}
```
이런 형태로 사용할수있는데

이렇게 되면 파라미터에 해당 인터페이스를 집어넣어 람다식을 파라미터에서 사용할수있게된다.
```java
public void testMainMethod(TestInterface inputRamda){
	inputRamda.myMethod("testmessage");
}
```
이렇식으로 일반 파라미터 처럼 사용할수있다.

```java
testMainMethod(i -> System.out.println(i));
```
이런식으로 하면 해당 파라미터가 함수형 인터페이스가 실행될때 람다식을 실행시키게 된다.
해당기능은 java8 부터 지원하고있다.

```java
TestClass::testMethod
testMainMethod(TestClass::testMethod)
```
이러한 방식으로 해당 클레스에 메소드를 참조해서  람다식으로 사용할수도있다.

