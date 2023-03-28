#unity #shader 
제공해주는 vertexdata
처음 shader만들때
struct appdata {} 로 써져있다

버텍스 메쉬에 대한 제공해주는 데이터이다 똑같이써야 작동한다
appdata는 말을 색깔리게 써놔서 MeshData 라고 일부러 바꿔쓴는거 같기도하다.
``` shader
struct appdata
{
	float4 vertex : POSITION;
	float3 normals : NORMAL;
	float4 tangent : TANGENT;
	float4 color : COLOR;
	float2 uv0 : TEXCOORD0;
	float2 uv1 : TEXCOORD1;
}
```

POSITION: 버텍스의 포지션 이다                          float3나 float4를 쓴다
NORMAL: 버텍스의 노말 이다                              float2를 쓴다
TANGENT: 탄젠트 백터이다 노멀맵핑에 쓰인다   float4를 쓴다
COLOR: 베텍스에 제공되어있는 색이다                float4를 쓴다
TEXCOORD0: 첫번째 UV 좌표이다                        float2 float3 float4를 쓴다
TEXCOORD1: TEXCOORD 0번째 1번째 2번째 이렇게 계속쓴다

왼쪽 변수이름부분은 맘데로 써도 된다.
