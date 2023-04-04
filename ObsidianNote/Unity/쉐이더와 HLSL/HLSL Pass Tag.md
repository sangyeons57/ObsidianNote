#unity/shader/pass/tags #language/HLSL 
[공식문서](https://docs.unity3d.com/520/Documentation/Manual/SL-PassTags.html)

사용방식은 subshader tag 와 같다
Tags{ "테그이름" = "값" , ...}

## LIghtMode Tag
조명 파이프라인에서 Pass의 역할을 정의 한다  이테그는 대부분 수동으로 거의 사용되지 않는다. 대부분의 겨우 조명과 상호작용 해야하는 세이더는 표면 셰이도로 작성된 자동으로 처리된다.
- Always: 항상 렌더링 된다, 라이팅이 적용되지않는다
- ForwardBase: Forward rendering에서 사용된다, 주변광 주요 방향성조명, 정점/SH조명및 라이트맵이 적용된다.
- ForwardAdd: Forward rendering에서 사용된다, 추가 픽셀당 조명이 적용되며 조명당 하나의 pass다
- Deferred: Deferred Shading 에사용한다, g-buffer에서 사용한다.
- ShadowCaster: 오브젝트깊이를 그람자맵또는 깊이 텍스쳐로 렌더링한
- PrepassBase: Legacy Defferred Lighting에서 상용한다. 노멀과 반사지수를 렌더링한다
- PrepassFinal: Legacy Defferred Lighting에서 사용한다, 조명과 텍스쳐를 결합해서 마지막 컬러를 표현한다.
- Vertex: 오브젝트가 라이트맵핑되지 않을경우  Legacy Vertex Lit rendering 에서 사용한다. 모든 버텍스에 빛을적용한다.
- VertexLMRGBM: 오브젝트가 라이트매핑 되어있을때 Legacy Vertex Lit rendering 에서 사용한다. 라이트맵이 RBGM으로 인코딩된 플렛폼
- VertexLM: 오브젝트가 라이트매핑 되어있을때 Legacy Vertex Lit rendering을 사용한다. 라이트맵이 이중 LDR로 인코딩 된 플렛폼에서

## RequireOptions
패스는 일부 외부조건이 충족 될때만 렌더링 되야함을 표연한다
- SoftVegetation: Soft Vegetaio이 켜저있는 경우에만 렌더링한다.
- 