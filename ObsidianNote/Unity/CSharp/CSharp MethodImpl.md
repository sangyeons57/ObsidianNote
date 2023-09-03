#Language/cpp 
[[CSharp lock]] 과 비슷하다
csharp lock 은 오브젝트를 키로 활용해 한 스레드 에서 작동하는 동안
다른 스레드에서는 작동하지 않게 하는 방식이면

이방식은 그냥 동기화 하는 방식이다.

``` CSharp
[MethodImpl(MethodImplOptions.Synchronized)]
public void method() {}
```
이렇게하면 해당 매서드가 동기화 된다

그외에 MethodImplOptions는
[공식 마이크로소프트](https://learn.microsoft.com/ko-kr/dotnet/api/system.runtime.compilerservices.methodimploptions?view=net-6.0)
- AggressiveInlining : 가능하면 인라인.
- NoInlining : 컴파일러가 런터임에 인라인되지 않도록 적용.
- Synchronized : 동기화. 한번에 한 쓰레드에서만.``