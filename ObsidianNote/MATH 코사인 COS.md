#MATH #cos

코사인 그래프는 주기가 2pi인 그래프이다.
따라서 2pi인 tau 값을 넣어 주게 되면
[그래프](https://www.desmos.com/calculator/xu9k5eeszx) 이렇게 된다
x가 1일 증가할때 마다 1주기가 돈다

이걸로 반복된 문향에 shader를 만들수있다

	#define TAU 6.2831855
	#define PI 3.14159

추가로 cos그래프에 0.5를 곱하고 0.5를 더하면
1부터 0사이에 값으로 만들수있다.

y = cos ( (x  + xOffset) * TAU * 5 ) * 0.5 + 0.5
xOffset = 
xOffset 일반 float로 값이 추가되면 대각선이 된다
cos추가되면 선에 흘들림이 추가된
ex) cos(x * TAU * 흘들림횟수) * 강도 