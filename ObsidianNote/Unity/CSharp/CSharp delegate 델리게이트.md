#language/CSharp 

델리게이트는 함수(메소드) 를 변수에 담기 위해 만들어진것 이다.
delegate의 뜻은 대리자이다

```CSharp
int num = 1; // 이렇게 int는 정수를 저장한다

void func(int num) { } // 메소드르를 저장하기 위해서는 델리게이트가 필요하다
```

```CSharp
void func (int num)

//방법은 이러하다

//delgate를 만들고 저장하려는 함수의 반환타입과 파라미터 타입이 같아야한다.
//델리게이트 타입을 만든거다
delegate void MyDelegate(int num);

//instance화 한건가?
MyDelegate del;
//그리고 이 del에 함수를 저장한다.
del = func;

//그리고 여기서 실제 함수처럼 실행시킨다
del(3);
```

```CSharp
void funcA (int num)
void funcB (int num)

//이러헥 여러 함수를 합쳐서 저장할수도있다
MyDelegate del;
del = funcA;
del += funcB;

//당연히 반환타입과 파라미터는 같아야하고
// 실행은 추가한 순서대로 된다.
del(3);
```

사용예제
이렇게 상태에 따라 다른 처리를 해야할때
```CSharp
만약 process 안에서 사용하는 데이터만 바뀐다면

if (A)
	process_A();
else if (B)
	process_B();
else if (C)
	process_C();
else 
	process_Default();
```

```CSharp
delegate int MyDel();

MyDel data;
data = A;

process(A);

void process(MyDel dataType)
{
	int data = datatType();
}
```

이렇게 해서 프로퍼티에 함수를 변수처럼 넣을수도 있다.

