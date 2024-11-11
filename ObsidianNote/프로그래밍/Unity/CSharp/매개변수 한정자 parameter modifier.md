#language/CSharp 

### ref
값을 참조 할수있게 하는 한정자이다
기본적으로는
```CSharp

void change(ref int num)
{
	num = 10;
}

int a = 1;
change(ref a);
//a = 10 이된다
```
이런 방식으로 사용한다

```CSharp
void change(int[] num)
{
	num[0] = 10;
	//num = [10,2,3];

	num = new int[] {4,5,6,7}
	//num = {4,5,6,7};
}

int[] a = new int[]{1,2,3};
change(a);
//a = [10, 2, 3];
```
참조를 벨류로 보내면 이렇게 새로운 new를 해서 값을 보내면 
해당 변환은 change함수 안에서만 적용되고
밖에서는 참조를 통해 변환시킨 `num[0] = 10`  만 적용된다

하지만 참조하는 값을 참초형태로 보내면
```CSharp
void change(ref int[] num)
{
	num[0] = 10;
	//num = [10,2,3];

	num = new int[] {4,5,6,7}
	//num = {4,5,6,7};
}

int[] a = new int[]{1,2,3};
change(ref a);
//num = {4,5,6,7};
```
이렇게 new로 값을 바꿔도 밖에있는 a 에 적용이 된다


또한 참조를 리턴 할수도있는데
이때 함수안에서 선언된 값의 참조를 리턴하면
함수가 종료할때 값이 사라지므로 비어있는 값을 참조하게된다 즉 NullReference

따라서 외부에있는 값을 argument로 받아서 그값의 참조를 반환하는 건된다


## out
out 은  ref와 기능적으로 완전히 동일하다
하지만 out 은 무조건 함수안에서 1번이상 값이 할당되어야한다.
반로 ref는 한번 할당된후 참조에 사용할수있다.


## in
in 은 ref와 기능적으로 완전히 동일하지만
in 은 값을 바꿀수없다 즉 readonly이다
하지만 참조는 할수있다
```CSharp
num = new int[] {1,2,3}; // 이건 안되다

num[0] = 1; //이건 된다
```


