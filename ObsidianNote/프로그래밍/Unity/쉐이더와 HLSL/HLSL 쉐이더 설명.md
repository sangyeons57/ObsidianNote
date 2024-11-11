#language/HLSL  #unity/shader  

## shader
- Properties - 변수를 받는곳, 데이터 받는곳
- SubShader
	- pass 
		- Vertex Shader
		- Fragment Shader
	- pass 는여러개를 가질수있다
- SubShader는 여러개를 가질수있다

Vertex Shader 와 Frgment Shader는 HLSL영역이고
나머지부분은 유니티문법이다.


vertex Shader
vertex점 이 위치하는걸 편집하는 놈이다.
점 과 점 3개 사이에 이미지가 표현됨으로 직접저그올 이미지에 영향을 주지는 않는다.
