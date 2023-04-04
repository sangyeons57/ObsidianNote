#language/HLSL  #unity  #unity/shader/transparent #unity/shader/Blend 
기본값에 다른값을 계산하는 계산식을 작성

알파블렌딩은 파이프라인에 마지막 단계에서 사용한다.
이유) 먼저그려진 픽셀과 합쳐져서 쓰기 위해서 이다, -> 투명효과를 내기위해

따라서 기본적으로 기본값을 가지고 계산한
[AlphaBlend공식문서](https://docs.unity3d.com/Manual/SL-Blend.html)

subshader 안에 작성하고
Blend {srcFactor} {dstFactor}
이런 방식으로 작성한다.

계산 방식
(src컬러 * src펙터) + (Dst컬러 * Dst펙터 *)

예) Blend one one
 =>Src컬러 * 1 + Dst컬러 * 1

### 명령어 정리
Source는 계산된 값을 말하고 Destinatio은 이미 화면에 표시된 값을 의미한다
그러니까 src는 현제 코드이고 des는 이미그려진놈
즉, 여러 오브젝트중에 이 오브젝트보다 먼저 그련진 놈들을 말한다.
따라서 src현제 내 오브젝트를 말하게 된

Off: 끄기
One: 1을 가지고 있는 값, 1을 곱한다
Zero: 0을 가지고 있는 값 값을 제거할때 사용한다, 0을 곱한다

SrcColor: 알파블렌딩으로 들어오기전 기본 color값을 곱한다.
SrcAlpha: 알파블랜딩으로 들어오기전 기본 Alpha값을 곱한다.
SrcAlphaSaturate: 소스 알파와 1 -  소스알파 알파중 작은값을 상용한다.

DstColor: destination 색상값을 곱한다
DstAlpha: destination 알파값을 곱한다

OneMinusSrcColor: 1-Source생삭값 을 곱한다
OneMinusSrcAlpha: 1-Source알파값 을 곱한다
OneMinusDstColor: 1-Destination생삭값 을 곱한다
OneMinusDstAlpha: 1-Destination알파값 을 곱한다

대표적인 Blend 옵션 조합
Blend SrcAlpha OneMinusSrcAlpha: 알파값 섥기
Blend One One: 포토샵에 Add같은 효과



알파 텍스쳐를 다루는데 있어서 pivot 기준으로 하기때문에 가장가까운부분과 피벗에 가까운 정도가 달라 텍스쳐가 잘리는 현상이  일어날수있다

해결방법은 큰 오브젝트를 작게 나누는 방법이 있고 예) 구름, 건물

다른방법으로는 Z-Buffer를 사용하지 않아서 그냥 덮어서 픽셀을 그리는거다
[[HLSL Unlit shader- ZWrite Off]]
하지만 이방법은 overDraw가 발생한다 
overdDraw는 한 픽셀에 두번이상 그리게 되는 경우이다.