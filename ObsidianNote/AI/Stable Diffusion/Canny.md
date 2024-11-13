
canny는 이미지의 외관선을 따는 알고리즘으로 
컴퓨터 이미치 처리에서 유명하게 쓰이고있다.

canny는 2개의 파라미터를사용하는데

#### lower threshold
해당 값은 Hysteresis 를 위한 값으로
Hystresis는 외곽선의 강함과 약함을 이미지에서 식별하는데 사용한다.
lower threshold와 upper threshold사이에 있는 값들은 약한 모서리로 판단한다.
lower threshold는 약한 모서리로 인식되는 최소값을 결정한다.

헤당값을 높일수록 만은 모서리를 탐지할 가능성이 높아지지만 노이즈 또한 많아진다.

#### upper threshold
upper threshold는 그리디언트의 최대값을 결정한다.
upper threshold보다 큰 값은 강한 모서리로 판단한다.

해당 값을 높일수록 정말 강한 모서리만 탐색할수있짐나 숨겨진 찾지못한 중요한 세부사항을 놓칠수있다.


### 작동방식
1. 가우시안 필터로 이미지를 둥글게 만들어 gradiant로 부드러운 그래프로 쓸수있게 한다.
2. Sabel계산을 통해 각 픽셀의 그래디언트 크기와 방향을 결정한다.
3. 모서리를 탐지하기 위해 "non-maximum suppression" 를 시행한다.
4. lower threshold와 upper threhold의 값을 이용해서  hystresis 를 적용해 강한 모서리와 약한 모서리를 그린다.
5. 