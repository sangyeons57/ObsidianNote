#language/HLSL  #unity/shader  #built-in 

## float: frac(float a);
a 소수 값에 소수 부분만 반환 합니다
1.22 -> 0.22     9999.99 -> 0.99 
이걸로 0부터 1사이 수를 반복하게 할수있다

## float: saturate(float a);
a 소수 값을 0과 1사이로 제한 시킵니다
0보다 작으면 0, 보다크면 1
사용 ) saturate( (x - a) / ( b - a ))
[[MATH 선형보간 수식]] << 설명

## float: smoothstep(float a , float b, float x);
```cg
float t = saturate ((x - a)/(b - a));
return t*t*(3.0 - (2.0*t));
```
smoothstap은 선형보간수시과 한계를 설정하고 추가로 값이 부드럽게 해준다
[설명그래프](https://www.desmos.com/calculator/6cx02nwuzc)
부드럽게 하는 값은 y = x * x * (3.0 - (2.0 * x*)) 이다 3차원 그래프
