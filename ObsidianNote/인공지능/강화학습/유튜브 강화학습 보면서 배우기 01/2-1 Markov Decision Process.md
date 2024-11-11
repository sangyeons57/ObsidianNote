[강화학습 1-3 강의 유튜브](https://youtu.be/DbbcaspZATg?si=xmKcAAR5DnxM6LEJ)

S= Status
a = Action
t = Time
이라고 할때
S1 에서 a1 을 하면 -> S2이된다.
S1,a1 -> S2
S2,a2 -> S3
S3,a3 -> S4

P(a1, S0, a0, S1)은
a1값을 얻으려고 할떄 S0,a0,S1 의 값알고 있으면

S1 은 S0,a0의 결과 임으로
S0,a0값은 필요없다.
따라서 P(a1,S1) 만 하면된다

하지만 P(S2, S0,a0, S1,a1) 이면
S2를 얻위해서는
S0,a0는 필요없지만
S1에서 a1을 해야지 S2가 나오기때문에
P(S2,S1,a1)와 같다고 할수있다.

여기서 말하는 바 는
연결된 한 다리까지만 알면 나머지는 전부 알 수 있다는 것이다.

여기서  P = Policy 정책이다.
어떤 a(action)을 할까 경정하는것이다 P(Policy)이다.

Markov Decision Process
강화학습의 목표는
Goal = maximize Expected Return
예측되는 리턴값의 최대화

수식으로 표현하면
Return Gt = Rt + G* R(t+1) + G^2 * R(t+2) + .........

Maximize Gt
자연어로 표기하면
Gt = 1액션 이후에 받을수있는값 + 입실론 * 2액션이후에 받을수 있는 값 ...
