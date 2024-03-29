#MATH/formula/선형보간

y = (x - a) / (b - a)

선형 보간(linear interpolation)에서 사용되는 수식입니다. 이 수식은 `v` 값을 `a`와 `b` 사이의 범위로 정규화하는 데 사용됩니다.

`v`가 `a`와 같으면 결과는 0이 되고, `v`가 `b`와 같으면 결과는 1이 됩니다. 이 수식은 종종 색상 그라데이션을 만들 때 사용됩니다.

1.  `v-a`는 `v`에서 `a`를 뺀 값입니다. 이 값은 `v`가 `a`와 얼마나 떨어져 있는지 나타냅니다.
2.  `b-a`는 `b`에서 `a`를 뺀 값입니다. 이 값은 `a`와 `b` 사이의 거리를 나타냅니다.
3.  `(v-a)/(b-a)`는 `v-a`를 `b-a`로 나눈 값입니다. 이 값은 `v`가 `a`와 얼마나 떨어져 있는지를 `a`와 `b` 사이의 거리로 나눈 것으로, `v`가 `a`와 `b` 사이에서 어디에 위치하는지 비율로 나타낸 것입니다.

예를 들어, `a=0`, `b=10`, 그리고 `v=5`라면 `(v-a)/(b-a)`는 `(5-0)/(10-0)`이 되어 0.5가 됩니다. 이것은 `v=5`가 0과 10 사이의 중간 지점에 위치함을 의미합니다.

이 수식은 종종 색상 그라데이션을 만들 때 사용됩니다. 예를 들어, 두 색상 사이의 그라데이션을 만들 때, 각 픽셀의 색상은 두 색상 사이의 비율로 결정됩니다. 이 비율은 `(v-a)/(b-a)` 수식을 사용하여 계산할 수 있습니다.
[그래프]( https://www.desmos.com/calculator/mlg5nbyr01 )
