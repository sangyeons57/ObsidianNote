#전처리기 #메크로 #define #cpp #조건부_컴파일 #컴파일

[전처리기 관련 내용](https://www.iar.com/kr/knowledge/learn/programming/basics-of-using-the-preprocessor/)

전처리기는 컴파일 되기 직전에 코드를 복사 붙여넣기 해서 만든 다음 
컴파일 하는 기능

### \# include 지시어
지시어를 찾으면 지정된 파일을 열고 해당내용을 넣으면 파일의 내용이 그위치에 작성된것처럼 컴파일 된다.

### \# Macros
\#define NUM 0
위와 같은 방식으로 사용된다
그러면 NUM이 쓰인곳을 다 0으로 컴파일 되기 전에 바꾼다
\#define FUNC(i) 10 + i
이것을 이렇게 함수처럼 사용할수도있다.

들어온 값을 그대로사용하기 때문에 IDE 에서 문제가 있어도 알려주지 않는다
따라서 문제가 생기기 좋고 잘봐야한다.

\#undef NUM
을 사용하면 NUM을 \#define 한것이더이상 작동하지 않는다.

----
---

### 조건부 컴파일
코드가 특정조건에서 컴파일 에서 제외된다
\#ifdef \#ifndef  \#iff \#elif \#else 등이 있다.

특징
값을 입력하는 시점은 컴파일이 모두 완료된 런타임 시점
지역변수나 전역변수를 이용해 비교할수 없다 컴파일이 이미 끝났기 때문
->따라서 컴파일 이전에 조건을 비교해야 히기 때문에 저처리기 단계에서 조건을 비교한다 = 전처리기 를이용해 조건문을 사용해야 한다.

\#if ~ \#endif
일반 if문과 같다
```Cpp
#define TEST 1

void main()
{
#if TEST > 0
 cout << "TEST는 0보다 크다." << endl;
#endif
}
```

\#ifdef  ~ \#endif
ABCD 가 define 으로 정의되어있다면 실행
```Cpp
#define ABCD

void main()
{
#ifdef ABCD
  cout << "ABCD는 정의되어 있음" << endl;
#endif
}
```

\#ifndef ~ \#endif
ABCD 가 define으로 정의되어있지 않으면 실행
```Cpp
#define ABCD

void main()
{
#ifndef ABCD
  cout << "ABCD는 정의되어 있지 않음" << endl;
#endif
}
```

\#elif ~\#else
일반적인 else if와 else이다

---
### Include보호 - guards 
#guard #include보호
조건부 컴파일의 부분
이미 한번 Include된 파일 을 또 Include하면 중보으로 코드가 생성되기 때무에 그걸 막기위해앴는 것이다
```Cpp
#define A - 이부분이 적용됨
#ifndef A - 이미 A가 존재함
#define A - 따라서 이부분 적용안됨
#endif    - 종료
```
\#include 도 같은 방법으로 같은파일이 두번 로드되는 일을 막는다.

---
---
### Error 지시어
#error지시어 
컴파일전 코드를 검사할때 사용한다
```Cpp
#if USE_COLORS && !HAVE_DISPLAY
#error "You cannot use colors unless you have a display"
#endif
```
