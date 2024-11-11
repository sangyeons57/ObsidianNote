프로퍼티는 객체지향에서 변수를 기본적을 private으로 해야함에 따라
getter,setter가 필요해 코드가 복잡해짐을 방지하기위해 나온 기능이다.

[한글 자료](https://cosmosproject.tistory.com/545)
```CSharp
class player
{
	private int hp = 100;
	private int mp = 200;

	public int Hp
	{
		set {hp = vlaue;}
		get {return hp;}
	}

	public int Mp
	{
		set {Mp = vlaue;}
		get {return Mp;}
	}

}
```

위처럼 value라는 값을c#에 내부적으로 재공해준다
그럼 이 value를 가지고 getter setter를 간단하게 만드는거다
특징으로는 public으로된 Hp 라는 변수를 추가적으로 만드는거다
사용하는 방식은 일반적인 변수와 같다
데신에 getter와 setter가 만들어져있는 Hp 변수 hp변수값을 참조한나다
```CSharp
Hp = 300; //hp변수의 값이 바뀐다
```

자동 프로퍼티
```CSharp
public string FirstName {get; set;} = "Park";
```
public으로 프로퍼티를 생성하면 자동으로 위의 과정을다해줘
변수는 private이고 get set으로만 접근할수 있게 된다.