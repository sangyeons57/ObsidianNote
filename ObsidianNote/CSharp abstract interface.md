#CSharp #abstract #interface
#### abstact
```csharp
abstract class Animal 
{
  public abstract void animalSound();
  public void sleep() 
  {
    Console.WriteLine("Zzz");
  }
}
// Derived class (inherit from Animal)
class Pig : Animal
{
  public override void animalSound()
  {
    // The body of animalSound() is provided here
    Console.WriteLine("The pig says: wee wee");
  }
}

class Program
{
  static void Main(string[] args)
  {
    Pig myPig = new Pig(); // Create a Pig object
    myPig.animalSound();  // Call the abstract method
    myPig.sleep();  // Call the regular method
  }
}
```
abstract는 자체적으로 인스턴스가 될수 없고
다른 클레스에서 상속 되어야한다

interface도 비슷한건데 차이점을 보면

#### 접근지정자
Interface - 접근지정자를 가질수 없고 기본적으로 public 이다
Abstract - 접근지정자를 가질수 있다.
#### 구현
Interface - 구현이 아닌 서명(정의)만 가질수 있다.
Abstract - 구현을 제공할수 있다.
#### 속도
Interface - 인터페이스가 느림
Abstract - 추상클레스가 빠름
#### 인스턴스화
둘다 - 못함
#### 필드
Interface - 필드를 가지지 못함
Abstract - 필드와 상수를 정의할수 있다.
#### 메소드
Interface - 추상메소드만 있음
Abstract - 비추상매소드 도 있을 수 있음
