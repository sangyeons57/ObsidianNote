#language/CSharp  #unity/attributes 
```csharp
[SerializeField]

[system.Serizaliable]
```
는 priavate에서도 inspector가 값을 조정할수있게 해주는 거다
Serialize는 직력화 작업인데
추상적인 데이터를 전송가능하고 저장가능한 형태로 바꾸는것을 의미한다.
이와 반대로 역직렬화 라는 게념이있다.

system.Serizable은 클레스 내부 요소도 인스펙터에서 보일수있게 한다.
