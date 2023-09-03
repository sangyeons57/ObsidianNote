#language/CSharp 

where 는 제너릭에 제약을 걸때 사용한다.

```CSharp
class GenericClass<T>
{

}
```
일반적인 제너릭

제너릭 제약
```CSharp
class GnericClass <T> where T : struct
{

}
```

이렇게 하면 T는 값에 해당 되는것만 들어 올수있다 즉 
int, string 등은 들어 올수있는데
list같은거는 못들어 온다.
``
재약조건 리스트
struct : 널을 허용하지 않는 값형식
class :  참조형식 이여야한다
new() : 매개변수가 없는 public생성자가 있어야한다 다른 조건과 함께 사용되는경우 new()를 마지막에 지정해야합니다
notnull : null이 아닌 형식이여야한다
unmanaged : 비관리 형식이여야한다
\<baes class name> : 지정된 기본 클래스이거나 파생된 클래스 이여야한다.
\<interface name> : 기본 지정된 인터페이스 거나 파셍되 유형이여야한다
U : 인터페이스 또는 일반클래스가 될수 있으면 T는 U 에 상속되어야 한다.


상속과 제너릭 제약을 같이 쓰고 싶을때는 이렇게 해야한다
```csharp
public class Genric<T> : ParentClass Where T : Generic<T> , new()
```
class 선언 : 상속 where T : 제약조건


제너릭은 특정 용도로 사용할때는 제약조건이 있어야 한다
예를 들어
클레스안에서  new T(); 로 생성 하려고 하면 Generic<T>, new() 제약조건이 필요하다

그이유는
Generic<T> 는  T 가 Generic<T> 를 상속 받는 경우
Generic<T>에서 T를 생성했을때 T 가 Generic<T> 기능을 사용하기 위해서고

new()는 T를 생성할수있게 하기 위해서다.