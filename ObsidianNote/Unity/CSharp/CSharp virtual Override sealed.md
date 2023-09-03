#language/CSharp  #virtual #override 
``` CSharp
public virtual double Area()
{
    return x * y;
}
```
virtual을 사용하면 override를 해서 재정의 할수있다
[microsoft 사이트](https://learn.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/virtual)
재정의를 할경우
상속을한 부모클래스로 접근을 해도 재정의된 함수가 실행된다.


또한 sealed 를 사용하면 오버라이드 하는것을 막을수있다
virtual과 똑같이 사용한다.
