#backpropagation #오차역전파 #DNN #AI

오차역전파 backpropagation은
DNN은 학습 방식중 하나이다

오자 역전파 방법이 없었을떄는 거대 신경망의 학습을 할수없었는데
오차 역전파 방법이 생기고 학습을 할수있게 되었다.

오차 역정파를 간단하개 설명하자면
1. ForwarPass 순전파 로 먼저 결과값 을 얻고
2. 해당 결과값과 실제값을 비교(빼기)를 해서 차이값(손실값, loss) 를 얻고
3. 해당 차이값을 ChainRule을 통해서 Backpropagation 역전파 를 해서 각 파라미터를 수정하는 것이다.

간단하게 예를 보자면

X 값과 Y값은 정해저 있을떄
x = 값은 인풋
y = 값은 실제값
$$ 
x = 2
$$
$$ 
y = 3
$$
$$ 
a = 3
$$
$$ 
ax = 6
$$
라는 a의 기울기를 구하는 거라고 예만들었을떄.

실제값과 결과값 사이의 오차는
$$
ax - y = 3
$$
이다

이떄 모든 값들의 오차를 구하는 방식으로
MSE = Mean Squared Error (평균 제곱 오차) 라는 방식이 있는데

지금은 평균을 낼필요없이 요소가 하나 만 있음으로

$$
(ax - y)^2 = 9 = Loss
$$
$$ Loss =  L$$
라고 볼수 있다.

이떄 오차값의 미분값은
$$ \delta Loss = 1 $$
이고
$$ {\delta Loss \over \delta Loss} = 1 $$
이라고 할수 있다.

다음으로 ax - y 값을  chain rule에 의거해 구하려면
$$ { \delta L \over \delta ((ax-y)^2)} $$
를 해야하는데 
$$ 
ax - y = u
$$
$$ 
L = u ^ 2
$$
라고 한다면

Loss에 대한 u의 미분 값은
$$ { \delta L \over \delta (u)} = { \delta (u^2) \over \delta (u)} = 2u = 2(ax -y) = 2 \times 3 = 6 $$
이라고 할수 있다.
다음으로 체인룰에 의거해 ax와 y를 구하면

$$
{ \delta L \over \delta (ax)} = { \delta u \over \delta ax } . { \delta L \over \delta u } = { \delta u \over \delta ax } \times 6 = 1 \times 6 = 6
$$
으로 구할수있고


$$
{ \delta L \over \delta y} = { \delta u \over \delta y } . { \delta L \over \delta u } = { \delta u \over \delta y } \times 6 = -1 \times 6 = -6
$$
$$
$$


$$
{ \delta L \over \delta a} = { \delta (ax) \over \delta a } . { \delta L \over \delta (ax) } = { \delta (ax) \over \delta a } \times 6 
$$

라는 식이 나오는데
먼저  x = 2 , a = 3이라고 정의 했기 때문에
$$
{ \delta (ax) \over \delta a } \times 6 = x \times 6 = 12
$$

$$
{ \delta L \over \delta x} = { \delta (ax) \over \delta x } . { \delta L \over \delta (ax) } = { \delta (ax) \over \delta x } \times 6 = a \times 6 = 18 
$$

으로 구해서 역전파를 마칠수있다
여기에다가 적용률 혹은 학습률을 넣어서 해당 오차만큼 파라미터에 적용하게 된다.