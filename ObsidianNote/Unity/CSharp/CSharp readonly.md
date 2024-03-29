#language/CSharp  #Programing/static #readonly

**Q:** 이건 c#코드인데 왜 static으로 표시할수없다는 거지? public static const int PLAYER1 = 1; public static const int PLAYER2 = 2;

**AL** `const` 키워드는 값을 상수화할 때 사용하며, 정적(static)으로 표시할 수 없습니다. 만약 정적으로 표시하고 싶다면, 다음과 같이 수정하시면 됩니다

`readonly` 키워드는 C# 언어에서 사용되는 접근 제한자입니다. `readonly` 키워드를 사용하면 해당 필드는 객체가 생성된 이후에도 값이 변경되지 않는 것을 보장할 수 있습니다.

예를 들어, 다음 코드처럼 `readonly` 키워드를 사용할 수 있습니다.

---
#### 즉 const대신 사용할수잇다

---
**Q:** 그러면 const 와 readonly의 차이점은 뭐야

**A:** `const`와 `readonly` 는 C#에서 변수의 값을 상수처럼 정의하는 키워드이지만, 사용 방법과 기능적인 차이점이 있습니다.

-   `const`: 컴파일 타임에 결정되는 상수 값을 정의할 때 사용합니다. `const`로 정의된 변수의 값은 프로그램 실행 중에 변경할 수 없습니다. 값이 컴파일 타임에 결정되므로, 속도가 빠릅니다.
    
-   `readonly`: 인스턴스화 후에 결정되는 상수 값을 정의할 때 사용합니다. `readonly`로 정의된 변수의 값은 프로그램 실행 중에는 변경할 수 없지만, 생성자에서 값을 초기화 할 수 있습니다.
    

**따라서, 값이 컴파일 타임에 결정되는 경우에는 `const`를, 값이 인스턴스화 후에 결정되는 경우에는 `readonly`를 사용하
는 것이 적절합니다.**

---

**Q:** 
그러면 왜 const는 static과 같이 못쓰는거야?

**A:** 
`const` 키워드는 값을 상수화 하는데 사용되는 제약사항이 있습니다. `const` 키워드는 컴파일 시점에 값이 결정되어야 하기 때문에, 타입의 값이 동적으로 결정되는 경우에는 사용할 수 없습니다. 또한, `const` 키워드는 클래스 내부에서만 사용할 수 있고, `static`이 아닌 일반적인 클래스 멤버로서 사용되어야 합니다.

반면에, `readonly` 키워드는 컴파일 시점에 값이 결정되어야 하는 것이 아니라, 생성자 또는 클래스 멤버의 초기화 시점에 값이 결정되어야 합니다. 이 키워드는 클래스 외부에서도 사용할 수 있으며, `static` 키워드와 같이 사용할 수도 있습니다.

**즉 const는 클래스의 맴버로 사용되여야만 한다는 제약사항이 있다.**
