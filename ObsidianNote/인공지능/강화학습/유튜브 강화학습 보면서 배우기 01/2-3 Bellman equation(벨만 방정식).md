[유튜브](https://youtu.be/gA-6J-nl4c4?si=c-azAzUKK5Za6G-7)
벨만방정식이란
상태 가치 값을 가지고 다음 상태값을
액션 가치 값을 가지고 다음 액션의 가치값을 구하는 것을 의미한다.

상태 가치값 = (현제상태 (큐) \*모든 액션의 가치) 평균

벨만 방정식 준네 어렵다

간단하게하자면
P (x,y) = P(x|y) * P(y)
이고
P (x,y | z) = P (X | y, z) * P(y | z)
라는 걸 이용해서 
State value function과 Action value fuction을 푸는 것이다.

전 시간에서
스테이트 에서 액션이 발생하면 다음 스테이트가 된다고 했던것도 이용한다
즉,
(St, at) => S(t+1)
이렇게 St,at 에 정보가 S(t+1) 에 존제하니 필요가 없어저 생략해도 되는거다
즉,
P(St+2, at+2 | St, at, St+1, at+1 )
이렇게 있으면
P(St+2, at+2 | St+1, at+1)

이렇게도 표현 할 수 있다는걸 알고

State value functio n과 Action value function을 계산 하기 쉬운 형태로 바꾸는거다.

