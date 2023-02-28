#cpp  #const #포인터 

### const값을 pointer로 변환
```cpp
const int cInt = 10;
int* pInt = (int*)&cInt;
*pInt = 20;
cout << cInt << endl;
```
cInt는 const여서 문법적으로 변환 할수없지만.
포인터를 이용해서 매모리에서 직접 변환할수있다
하지만 print해보면 변환이 안되어있는데 cpu에서 10으로 생각하기때문이다
따라서 volatile을 사용해서 매모를 직접 가져오게해야한다.
```cpp
volatile const int cInt = 10;
int* pInt = (int*)&cInt;
*pInt = 20;
cout << cInt << endl;
```

### pointer const
바뀌지 못하는 포인터

포인터가 가르키는 값을 상수화 => 가르키는 값을 못바꾸게함
```Cpp
int a = 0;
const int* pConstInt = &a;

*pConstInt = 100; // 에러가난다.

```

포인터를 상수화 => 가르키는는 메모리위치를 수정을 못하게함
```Cpp
int a = 0;
int* const pIntConst = &a;

*pIntConst = 100; //포인터가 상수화 된거여서 값을 바꿀수있다.
```

둘다 상수화
```Cpp
int a = 0;
const int* const pIntConst = &a;

*pIntConst = 100; //값도바꿀수없고 포인터도 바꿀수없다.
```