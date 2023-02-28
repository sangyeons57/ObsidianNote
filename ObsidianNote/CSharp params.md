#CSharp #params
[관련한글정보](https://developer-talk.tistory.com/174)
params는들어오는 param가 가변적일데 사용하는 키워드이다
```CSharp
public void test(params int[] numbers)
{
	foreach (int number in numbers)
	{
		Debug.log(number);
	}

}
```

우선순위는
오버로딩할때
매개변수가 동일한 매개변수를가 먼저 실행된다

인수가 전달되지않은면 배열의 길이는 0이다
배열의 크기를 정의하지 않아도된다
params는 여러인수중 한개만 가능하다
즉 test (prams int[] numbers, params float[] floats){} 이렇게 여러게 쓰는것이 불가능
그리고 다른 인수랑 쓰일때
가장 params키워드기 있는 인수는 가장 마지막에 위치해야한다.