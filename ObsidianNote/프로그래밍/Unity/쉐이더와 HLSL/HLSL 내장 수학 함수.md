#unity/shader #language/HLSL 

### 삼각함수
sin(x), cos(x), tan(x) : 기본 삼각함수

asin(x), acos(x), atan(x) : x의 각 성분에 역삼각함수
범위
	PI / 2 < asin < PI / 2
	1 < cos < 1
	PI / 2 < atan < PI / 2
 
atan2(y, x) :  atan(y/x) 에 해당하는 함수로
  atan() 는 범위가 위와 같지만 atan2는 -PI < atan2 < PI 
  의 범위를 가진다 일반적으로 x != 0 인경우 더 확실한 값을 얻을수있어 선호된다.

sinh(x), cosh(x), tanh(h) : 쌍곡 삼각함수

sincos(x ,s ,c)  :
sin(x) 와 cos (x) 를 동시에 s, c 에 리턴한다. 이때 s, c 는 x와 동일한 차원이여야한다.

degrees(x) : 라디안단위 x에 해당하는 디그리 값을 리턴한다. 

radians(x) : 디그리 x에 해당하는 라디안 값을 리턴한다.

[[파이와 디그리 그리고 라디안]]

-----
### 수학함수
sqrt(x)  : 제곱근

rsqrt(x) : 제곱근의 역수 (1 / sqrt(x))

exp(x) : e^x        e를 바닥으로하는 지수 ex를 돌려준다.

epx2(x) : 2^x      2를 를 바닥으로하는 지수를 돌려준다.

pow(x, y) : x^y    x의 y 제곱

Idexp(x, y) : x * 2^y    2의 y승 * x

log(x), log10(x), log2(x)
x 값은 양수여야한다.

----
### 값 변환 함수
abs(x) : 절대값

sign(x) : 부호에따라 음수이면 -1 , 0이면 0, 양수면 1을 리턴

ceil(x) : 무조건 올림한 정수를 리턴
floor(x) : 무조건 내림한 정수를 리턴
round(x) : 반올림한 정수를 리턴
-> 결과값을 정수지만 타입은 float이다.

max(x, y)  min(x, y) : 최대 최소값

clamp(x, min, max) : x가  min보다 적은 min값으로, max보다 크면 max값으로 리턴한다

saturate(x) : x를 0부터 1사이 범위로 잘라넨다
	clamp(x, 0, 1) 과 같은거 같다.

lerp(x, y, x) : x + s * (y - x) 로 선형보간 한다.
	s가 0인 경우 x, s가 1인 경우 y, 를 리턴한다.

step(x, y) : x <= y 이면 1을 리턴 하고, 그렇지 않으면 0을 리턴한다.

smoothstep(min, max, x) : x가 min 과 max 사이인 경우에 대해서
	0 과 1 상이에서 부드럽게 변하는 Hermite보간법을 리턴한다.
	x 가 min 보다 작으면 0, x가 max보다 크면 1을 리턴한다.

asfloat(x) : 입력인자값을 float 타입으로 바꾼다.
asint(x) : 입력인자값을 int 타입으로 바꾼다.
asuint(x) : 입력인자값을 uint타입으로 바꾼다. (uint : 0 ~ 양의 정수 만 있는값)

fmod(x, y) : x / y 의 나머지를 실수로 리턴한다.

frac(x) : x의 소수점 이하 부분을 리턴한다.

frexp(x, e) : 주어진 실수 x의 표현에서의 소수점이하인 값인 가수부분가 지수부분을 동신에 리턴한다
	가수부분은 e로 리턴하고 , 지수부분은 함수 리턴값으로 리턴한다. 
	x가 0인 경우 전부 0으로 리턴한다.

mod(x, i) : x의 정수 부분을 i로 리턴하고, 소수점 이하부분을 함수값으로 리턴한다.

-----
### 테스트 함수
all(x) : x의 모든 성분이 0 이외의 값인지를 테스트한다.

a11(x) : 인자값의 모든 원소들이 0이 아닌지를 검사한다. 모두 0이 아니면 true를 리턴

any(x) : x의 원소중에 0이 아닌 원소가 하나라도 있으면 true값을 리턴한다.

isinf(x) : 무한대 값 이면 true를 리턴한다.

isnan(x) : not a  number이면 true를 리턴한다.

-----
### 백터 함수
cross(x, y) : 두 백터의 외적을 계산한다, 두인자와 리턴은 모드 float3 타입이다.

dot(x,y) : 두 백터의 내적을 계산한다

distance(x,y) : 두 백터의 길으를 계산한다.

normalize(x) : 정규화된 백터를 리턴한다. 즉 리터 값은 x / length(x) 와 동일하다.

determinat(m) : 행력식을 리턴한다. 입력인자는 정방행렬 이여야 한다.
	정방행렬은 같은 행과 같은 같은 열을 가지는 행렬을 의미한다.

transpose(m) : m 의 전차행렬을 리턴한다.

mul(x, y) : 두 행렬의 곱을 계산한다.

reflect(i, n) : 정반사광의 방향 백터를 구하는 백터 반사 함수.
	첫번째 인자로 입사광의 방향백터
	두번째 인자로 반사면에 법선을 받는다.

-----
### 기타 함수
tex1D(s, t) : 1D 텍스쳐의 참조
	s 는 셈프러 또는 sampler1D
	t 는 스칼라
tex1D(s, t, ddx, ddy) : 미분을 지정한 1D의 텍스처 참조.
	s는 샘플러 또는 sampler1D 개체,
	 t, ddx, ddy는 스칼라.

tex1Dproj(s, t) : 1D의 투영 텍스처 참조.
	s는 샘플러 또는 sampler1D 개체,
	 t는 4D 벡터.
	 t는 참조가 실행되기 직전의 성분으로 나눔
	  
tex2D(s, t) : 텍스쳐 샘플링에 사용하는 HLSL 함수.
	첫 번째 인자에서 두 번째 인자 좌표에 있는 텍셀(텍스쳐의 1화소)을 구하는 함수.
	 s는 샘플러 또는 sampler2D 개체,
	  t는 2D 텍스처 좌표.

tex2D(s, t, ddx, ddy) : 2D의 투영 텍스처 참조.

tex2Dbias(s, t) : 2D의 바이어스 텍스처 참조.

tex3D(s, t) : 3D의 볼륨 텍스처 참조.
	s는 샘플러 또는 sampler3D 개체,
	 t는 3D 텍스처 좌표.

tex3D(s, t, ddx, ddy) : 미분을 지정한 3D의 볼륨 텍스처 참조.

tex3Dproj(s, t) : 3D의 투영 볼륨 텍스처 참조.

tex3Dbias(s, t) : 3D의 바이어스 텍스처 참조.

texCUBE() : 입방체 텍스쳐를 샘플링하는 함수.
	3D 큐브의 텍스처 참조.
	 s는 샘플러 또는 samplerCUBE 개체,
	  t는 3D 텍스처 좌표.

texCUBE(s, t, ddx, ddy) : 미분을 지정한 3D의 큐브 텍스처 참조.

texCUBEproj(s, t) : 3D 투영의 큐브 텍스처 참조.

texCUBEbias(s, t) : 3D 바이어스 큐브 텍스처 참조.

**ddx(x), ddy(x)** : 스크린공간의 x, y 좌표에 대한 x, y의 편미분을 리턴한다.

**fwidth(x)** : abs( ddx(x) ) + abs( ddy(x) ) 를 리턴한다.

**clip(x)** : x의 한 원소가 0보다 작으면 현재 픽셀을 버린다. x의 각 성분이 면으로부터의 거리를 나타내는 경우, 이 함수를 사용해, 클립면을 시뮬레이션 한다.  
→ 이 함수들을 **픽셀셰이더에서만** 사용할 수 있다.


이 밑에거는 어려운거

**faceforward(n, i, ng)** : 관찰자를 향하는 표면 노말(-n * sign(dot(i, ng)))을 리턴한다.  

**reflect(i, n)** : 반사벡터를 리턴한다.
	입사 방향 i, 표면 법선 n로 했을 경우,
	 v = i - 2 * dot(i, n) * n 에 의해 구할 수 있는 반사 벡터 v를 리턴한다.  

**refract(i, n, R)** : 굴절벡터를 리턴한다.
	입사 방향 i,
	 표면 법선 n,
	  굴절 R 의 상대 인덱스가 주어졌을 경우 굴절 벡터 v를 리턴한다.
	   i와 n사이의 입사각이 지정된 R보다 너무 크면 (0, 0, 0)을 리턴한다.  

**lit(n · l, n · h, m)** : 조명계수 벡터(앰비언트, 디퓨즈, 스펙큘러, 1)를 리턴한다.
	앰비언트 = 1;
	디퓨즈 = (n · l < 0) ? 0 : n · l;
	스펙큘러 = (n · l < 0) || (n · h < 0) ? 0 : (n · h * m);  

**D3DCOLORtoUBYTE4(x)** : 4D 벡터 x의 성분을 교체 및 스케일링 해, 일부 하드웨어에 있는 UBYTE4 지원의 부족을 보정한다.

**noise(x)** : 연기나 화재효과에 사용되는 Perlin 노이즈값을 리턴한다. [[Project - Minecraft 2023-04-02 정리]]

### 데이터 타입

sampler2D:  택스처에서 텍셀을 구해올때 사용하는 샘플러 데이터 형.
