[유튜브 SGD ](https://youtu.be/gHcu0NKyli4?si=qV9YXkr7OMQlNjU2)
[유튜브 SD vs SGD](https://youtu.be/LwQGhBVoU6I?si=AQTyM_wh9wBS8MX6)
[유튜브 Momentum , RMSProp](https://youtu.be/-oHYAUhq5ao?si=ljN8sCLrMTeKlhAo)

미니 배치란 학습시킬때 학습 데이터를 일정한 비율로 나눠서 학습 시키는걸말한다
이렇게 하는 이유는 더 빠르고 더 안정적이게 학습 시키기 위해서다
기본적인 미니 배치는 각 요소의 오차율은 모두 더해서 평균을 내어서 그값만큼 학습시킨다
이것이 **Gradient Descent (GD)** 이다


**Stochastic Gradient Descent (SGD)** 는 GD가 모든 요소를 구해 합하는 방식이 비효율적이고
부분 오차 최하 에 들어갈수있는 부분을 개선한것이다.
매번 배치에 요소중에서 랜덤한 1개를 뽑아서 그것만 학습 하는거다.
이렇게하면 학습이 불안정 해지지만 빠르게 최하점에 정확하게 갈확률이 높다



Stochastic Mini-Batch 라는 방법도 있는데 
이건 SGD와 SD 의 합의 점으로
미니배치중 일부만 사용하는거다 이건 DQN의 Experience Replay와 비슷해보인다.


Momentum 이란 학습에 가속도를 넣자 라는 개념이다.
즉계속해서 한쪽으로 꾸준히 움직이면 그쪽은 더 크게 학습하자는 개념이다.또한 얕은 지점에서 브레이크 거는것과 같이 갑자기 온변화가 즉시 적용되지않는다 이에대한 장점으로는
local descent를 빠저나갈수있다 이고 단점은 가속도로 인해 최저점에 도달했을때 밀려날수있다이다 결구 다시 최점으로 가긴한다 간단한 코드는 이렇다
```python
# 초기화
velocity = 0
gamma = 0.9 # 모멘텀 계수
learning_rate = 0.01

for i in range(num_iterations):
    # 그래디언트 계산 (여기서는 loss_function과 weights가 이미 정의되었다고 가정)
    gradient = compute_gradient(loss_function, weights)
    
    # 모멘텀 업데이트
    velocity = gamma * velocity + learning_rate * gradient
    
    # 가중치 업데이트
    weights = weights - velocity
```
여기서 보면 velocity가 계속 유지되면서 업데이트 된다.
실제 가중치는 velocity를 이용해서 적용하고 
velocity는 가속도(gamma)개수 * 원레 속도+ 학습률 * 새로은 가속도  이다.



RMSProp 은 가파른방향은 천천히 가고 완만한 방향은 빠르게 가자 이다.
즉 learning Rate를 차이가 그면 줄이고 차이가 적으면 늘리자 이다.
왜 이렇게 하냐면
가장 오류가 낮은 점을 찾아 굴러 떨어질때 너무 오차를 줄이는 방향으로만가면
실제가장낮은점보다 근시안적으로 내려갈수있다. 또한 너무 완만한 면에서는 학습이 빨리되도록 성큼 성큼 움직일수있다.코드 -> 
```python
# 초기화
learning_rate = 0.01
epsilon = 1e-7
rho = 0.9
r = 0

for i in range(num_iterations):
    # 그래디언트 계산
    gradient = compute_gradient(loss_function, weights)
    
    # r 업데이트
    r = rho * r + (1 - rho) * gradient ** 2
    
    # 가중치 업데이트
    weights = weights - learning_rate * gradient / (np.sqrt(r) + epsilon)
```
RMSProp의 목적은
그라디언트 제곱의 누적평균을 유지시킨다
이평균을 제곱근으로 그라디언트를 나눈다.
그러니까 즉 과거의 속도로 현제를 나누는 건데 과거에 속도가 큰경우 학습률이 줄어들고
과거의 속도가 느린경우 학습률이 늘어난다.


평가가 가장좋아보이느 경사하강법 은 바로 =>  ADAM
Adam (Adaptive Moment Estimation)은 딥러닝에서 널리 사용되는 최적화 알고리즘 중 하나입니다. Adam은 신경망의 매개변수를 실시간으로 조정하여 정확도와 속도를 향상시키는 데 도움이 됩니다.

Adam은 각 매개변수의 학습률을 그 매개변수의 과거 그래디언트와 모멘텀에 기반하여 적응시키는 방식으로 작동합니다. 이는 각 네트워크 가중치(매개변수)에 대해 개별적으로 유지되는 학습률을 통해, 학습이 진행됨에 따라 각 매개변수의 학습률을 독립적으로 조정합니다.

Adam 알고리즘은 다음과 같은 장점을 가지고 있습니다:

- 구현하기 쉽습니다.
- 계산 효율성이 높습니다.
- 메모리 요구량이 적습니다.
- 그래디언트의 대각선 재조정에 불변합니다.
- 데이터 및/또는 매개변수가 큰 문제에 잘 맞습니다.
- 비정상 목표에 적합합니다.
- 매우 노이즈가 많거나 희소한 그래디언트에 대한 문제에 적합합니다.
- 하이퍼파라미터가 직관적인 해석을 가지며 일반적으로 조정이 거의 필요하지 않습니다.

따라서, Adam은 그래디언트 하강법의 확장으로 볼 수 있으며, 컴퓨터 비전과 자연어 처리 등의 딥러닝 응용 분야에서 널리 사용되고 있습니다.

간단히말해서 RMSProp + Momentum 이다
``` python
# 초기화
m, v = 0, 0
beta1, beta2 = 0.9, 0.999
learning_rate = 0.001
epsilon = 1e-8

for i in range(num_iterations):
    # 그래디언트 계산
    gradient = compute_gradient(loss_function, weights)
    
    # m과 v 업데이트
    m = beta1 * m + (1 - beta1) * gradient
    v = beta2 * v + (1 - beta2) * gradient ** 2
    
    # 편향 보정
    m_hat = m / (1 - beta1 ** (i + 1))
    v_hat = v / (1 - beta2 ** (i + 1))
    
    # 가중치 업데이트
    weights = weights - learning_rate * m_hat / (np.sqrt(v_hat) + epsilon)

```

보면 
weights = weights - learning_rate * m_hat / (np.sqrt(v_hat) + epsilon)
는 기본적으로 RMSProp를 따른다
=> weights = weights - learning_rate * gradient / (np.sqrt(r) + epsilon)

다른점을 보면 gradiant대신에 => m_hat,  r 대신에 => v_hat 을 쓴걸 볼수있다.

여기서m = 그레디언트의 크기와 방향 v = 그레디언트의 크기

그런데 여기서 m, v는 초기값이 = 0 이여 실제 그레디언트의 평균보다 작을수있어
m_hat과 v_hat을 사용하게된다.
예상)
m = 0, v= 0일때
next m = 0.1 * gradiant
next v = 0.001 * gradiant ** 2
가속도를 구하기위한 값에 초기값이 0 이여서실제 gradiant에 비해 과하게 작아짐

m = (0.9  * m(=0))(=0) + (1 - 0.9) * gradient 
m을 처음 때로 다바꿔봄

m = 0.1 * gradiant 일때
m / (1-beta ** (i + 1)) => m / (1-beta ** (1))

=> (0.1 * gradinat)  / (1 - 0.9 ** 1)
이 가장처음 의 m_hata형태 이다.

(0.1 * gradiant) / 0.1 => gradiant
이렇게 되면 처음에는. 즉, 가속도가 0으로 없을때는 gradiant자체 값을 받게된다.


그럼 마지막으로 여기를 보게되면
weights = weights - learning_rate * m_hat / (np.sqrt(v_hat) + epsilon)

weights 를 업데이트 할때
확습률에 따라 학습 을 하게되는데
학습량은 (방향  * 학습량 인데 가속도가 포함된 ) / (속도(방향x) 인데 가속도 + 0을 방지하기위한 값)
이면서 아직 가속도에 0일때를 고려해 학습 초기에는 gradiant그 자체를 다 받는다.

즉 beta가 클수록 최근 변화를 강하게 받아들인다.
