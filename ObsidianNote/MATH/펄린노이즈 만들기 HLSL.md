#language/HLSL #unity/shader #MATH/perlin 

```HLSL
float Noise(float2 xy)
{
    float2 noise = (frac(sin(dot(xy, float2(12.9898, 78.233) * 2.0)) *  43758.5453));
    return abs(noise.x + noise.y) * 0.5;
}
```
우선 dot product 내적 을 한다.
내적은 두 좌표를 가져가서 스칼라 값을 리턴한다
내적 : 두 백터 사이의 각도의 코사인 값을 리턴
dot(xy, float2(12.9898, 78.233) * 2.0)
이부분이 실제적으로 랜덤을 만드는 부분이 아닐까 한다.

여기서 나온 값을 sin() 에 넣고
43758.5453  배 를 해줘서 그래프에 깊이를 크게 한다.
그후 frac 로 정수화 시킨다.

그후 이 값의 평균값을 리턴한다.


```HLSL
float SmoothNoise(float integer_x, float integer_y)
{
    float corners = (Noise(float2(integer_x - 1, integer_y - 1)) +
	     Noise(float2(integer_x + 1, integer_y + 1)) +
	     Noise(float2(integer_x + 1, integer_y - 1)) +
	     Noise(float2(integer_x - 1, integer_y + 1))) / 16.0f;
    float sides = (Noise(float2(integer_x, integer_y - 1)) + 
	     Noise(float2(integer_x, integer_y + 1)) +
	     Noise(float2(integer_x + 1, integer_y)) +
	     Noise(float2(integer_x - 1, integer_y))) / 8.0f;
    float center = Noise(float2(integer_x, integer_y)) / 4.0f;
    return corners + sides + center;
}
```
smoothNoise함수는 3x3 그리드에 평균노이즈 값을 계산해서 작동한다.
각지점에 노이즈를 계산하고 가중 평균을 계산 하여 평활화된 노이즈값을 생성한다.

corners 9개의 그리드중 4개의 꼭짓점을 합해서 16을 나눠서 평균을 구한다.

sides 9개의 그리드중 4개의 옆부분을 합해서 8을 나눠서 평균을 구한다.

center 9개의 그리드중 1개의 중간부분 부분을 4로나눈다

3개를 합해서 리턴한다.

center를 1/4 를 하는 이유는
1/4만큼의 가중치를 주기 위해서 이다.
따라서
중심점에는 1/4 측면지점에는 1/8 꼭진점에는 1/16의 가중치를 부여한다.
1/4 * 1 + 1/8 * 4 + 1/16 * 4 =
1/4 + 1/2 + 1/4 = 1


```HLSL
float InterpolatedNoise(float x, float y)
{
    float integer_x = x - frac(x), fractional_x = frac(x);
    float integer_y = y - frac(y), fractional_y = frac(y);

    float p1 = SmoothNoise(integer_x, integer_y);
    float p2 = SmoothNoise(integer_x + 1, integer_y);
    float p3 = SmoothNoise(integer_x, integer_y + 1);
    float p4 = SmoothNoise(integer_x + 1, integer_y + 1);

    p1 = CosineInterpolation(p1, p2, fractional_x);
    p2 = CosineInterpolation(p3, p4, fractional_x);

    return CosineInterpolation(p1, p2, fractional_y);
}

float CosineInterpolation(float x, float y, float fractional)
{
    float ft = 3.141592f * fractional;
    float f = (1.0f - cos(ft)) * 0.5f;
    return x * (1.0f - f) + y * f;
}
```

CosineInterpolation 부분 
파이값을 넣어줘서 cos그래프로 주기를 맞춘다
-1 < cos < 1 임으로 1.0 - cos 
2부터 1사이 * 0.5 = 1 부터 0.5 사이의 값이 만들어진다
x, y 값에 해당 f값을 반비례로 곱한다.

InterpolatedNoise 부분
노이즈 보간하기

frac() 로 소수점 이하와 소수점 부분을 나눈다.

x, y 와 x + 1, y값을 코싸인 보간을 해준다.
x, y+1 와 x + 1, y + 1값을 코싸인 보간을 해준다.

그다음에 y값으로 보간을 해주고 리턴해준다.


```HLSL
float CreatePerlinNoise(float x, float y)
{
    float result = 0.0f;
    float amplitude = 0.0f;
    float frequency = 0.0f;
    float persistance = 0.1f;

    for (int i = 1; i <= 4; i++)
    {
        frequency += 2;
        amplitude += persistance;

        result += InterpolatedNoise(x * frequency, y * frequency) * amplitude;
    }
	return result
}
```

그냥 처음에 렌덤만들고 무한 보간느낌인듯
