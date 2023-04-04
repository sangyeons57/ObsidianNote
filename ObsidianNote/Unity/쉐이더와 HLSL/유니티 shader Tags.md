#unity/shader/Tags #language/HLSL
Tags: 언제 어떻게 렌더링엔진에서 렌더링 할 지 정하는것
	입력방식은 key-value방식이다
	SubShader에 순서 및 기타 파라미터를 판정하는데 사용한다
	unity에서 인식되어 SubShader안에서 작성되어야 한다

예제)
Shader
{
	Properties { }
	SubShader
	{
		Tags { "Key" = "Value" }
		LOD
		Pass { }
	}
}

## Queue
	오브젝트를 그리는 순서를 결정하는값.

1. Background: 가장먼저(가장뒤에) 렌더링된다. 배경으로사용될거에 쓰인다.
2. Geometry: Queue를 설정하지 않은면 기본적으로 설정된다. 불투명 오브젝트에 사용된다.
3. AlphaTest: 투명한 오브젝트에 사용된다.
4. Transparent: Geomatry와 AlphaTest후에 뒤부터 순서데로 렌더링한다. 유리/파티클 등에사용된다
5. Overlay: 오버레이효과에 사용된다 가장 마지막에 렌더링하는경우에 사용한다. 

Background: 1000
Geometry: 2000
AlphaTest: 2450
Transparent: 3000
Overlay: 4000
숫자값이 낮을수록 먼저 렌더링 된다.

## RenderType
	쉐이더를 사전에 정의된 여러 그룹으로 분류한다.
	쉐이더 종류를 분류하기위해 사용한다.
	키와 값을 통해 쉐이더가 분류되면 프로그램을 통해 특정 카메라에 지정한 셰이더 종류만 보이도록
	ShaderReplacement 쉐이더제거 기능을 사용할수있다.
1. Opaque: 대부분의 불투명쉐이더
2. Transparent: 대부분의 부분적으로 투명한쉐이더
3. TransparentCutout: 마스킹된 투명 쉐이더, 반투명이 없는 없는 쉐이더 (완전히 투명 or 불투명)
4. Background: skybox 쉐이더
5. Overlay: 플레어, 후광, GUI(그레픽인터페이스)
6. TreeOpaque: 나무껍질
7. TreeTransparentCutout: 엔진 나뭇잎
8. TreeBillboard: Terrain엔진 나뭇잎 빌보드
9. Grass: grass
10. GrassBillboard: Terrain엔진 빌보드


## DisableBatching
	오브젝트의 모양을 변경하는(로컬좌표게에서 오브젝트일부분을 늘리거나 줄이다) 
	쉐이더가 동작하지 않도록 할수있다 
1. True: 이셰이더에대해 배칭을 비활성화
2. False: 배칭을 비활성화 하지 않음: 기본값
3. LODFading: LOD fading이 활성화 되었을때 배칭을 비활성화, 두개의 LOD를 블렌딩하여 부드렁누 전환 효과를준다; 주로 나무에사용함


## ForceNoShadowCasting
	그림자를 투영하지 않게 한다 shaderReplacement랑 같이 사용한다고 한
1. True: 그림자를 투영하지 않는다.
2. False:  그림자 투영  false는 안쓴다


## IgnoreProjector
	쉐이더가 프로젝터에 영항을 받는지 유무
True: 프로젝터의 영향을 받는다
False:프로젝터에 영향을 받지 않는다, 반투명물체와 지형바닥에 사용

프로젝터
	효과를주는카메라, 그림자생성, 탄흔생성, 조명효과 등을 만들기 위한 기능


## CanUseSpriteAtlas
	쉐이더가 스프라이트를 위한것임을 선언
True: 스프라이트를 위한 것임을 선언
False: 스프라이트용인데 아틀라스로 자동하지않을때 사용


## PreviewType
	미리보기에서 material의 표시 방법
Plane, Skybox, Sphere 등

