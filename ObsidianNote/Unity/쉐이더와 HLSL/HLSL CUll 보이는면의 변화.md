#language/HLSL #unity/shader #unity/shader/cull
culling 뜻은 무언가 제거한다는 뜻이

Pass{} 안에쓴다
```HLSL
SubShader {
	Tags{}
	Pass {
		Cull 상태값;
		
		CGPROGRAM
		ENDCG
	}
}
```

-  Back
	뒷면이 렌더링되지않는 기본적인 Defula상태
- Front
	앞면이 랜더링 되지않는 뒷면만 랜더링 되는 상태
- Off
	앞뒷면이 전부다 보이는 상