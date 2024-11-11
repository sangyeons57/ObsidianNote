#language/HLSL  #SAMPLE_TEXTURE2D #tex2D #unity/shader 

tex2D와 SAMPLE_TEXTURE2D는 셰이더 스크립트에서 텍스처를 샘플링하는 방식에 차이가 있습니다. 
tex2D는 고정된 샘플러 상태를 사용하고, SAMPLE_TEXTURE2D는 사용자가 정의한 샘플러 상태를 사용합니다.
샘플러 상태는 텍스처 필터링, 래핑, mipmapping 등의 옵션을 결정합니다.
tex2D는 레거시 셰이더에서 사용되고, SAMPLE_TEXTURE2D는 Shader Graph나 HDRP에서 사용됩니다
또한, tex2D는 이미지 이펙트에서만 작동하고, SAMPLE_TEXTURE2D는 포스트 프로세싱 이펙트에서도 작동합니다