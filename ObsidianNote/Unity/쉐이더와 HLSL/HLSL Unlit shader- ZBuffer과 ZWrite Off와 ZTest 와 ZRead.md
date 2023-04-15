#language/HLSL #unity/shader/zwrite  #unity/shader/zbuffer #unity/shader/ztest
z-Buffer : 깊이버퍼 : z축이라는뜻 아닐까?
## z - Buffer
Zbuffer 은 화면의 각 픽셀마다 카메라와 가장 가까운 오브젝트의 표면 까지의 거리값은 0 ~ 1
로 기록해놓은 텍스쳐이다.
카메라의 가장 가까운 위치(Near Plane)있으면 0 값을 갖고
가장먼곳(Far Plane)에 있으면 1값을 갖는다.
그리고 아무 오브젝트도 기록하지 않은 상태면 ZBuffer의 모든 값은 1이다.

ZTest ZWrite 둘다 Pass안 CGPROGRAM밖에 쓴다.
```HLSL
SubShader {
 Tags {}
 Pass{
	여기에

	CGPROGRAM
 }
}
```

## ZTest 
[공식문서](https://docs.unity3d.com/kr/2021.3/Manual/SL-ZTest.html)
ZBuffer에 이미 기록된 값에 자신의 깊이값을 비교하여, 자신의 픽셀을 렌더링 할지 여부를 결정하는 연삿을 나타내눈 구문이다.
- Less : 기존 지오메트리보다 앞에있는  지오메트리를 드로우한다. 같은거리에 있거나 뒤에있는 지오메트리는 드로우 하지 않는다.
- LEqual :  위에있는 Less에다가 추가적으로 같은 거리에 있는 지오메트리를 드로우한다. 기본값이다.
- Equal : 기존 지오메트리와 같은 거리에 있는 지오메트리르 드로한다. 즉 겹처야 드로우한다.
- GEqual : 기존 지오메트리 뒤에있거나 기존 지오메트리와 같은 거리에 있는 경우 드로우한다. 즉 오브젝트를 거처야 볼수있다.
- Greater : 지오메트리 뒤에 있는 경우에만 드로운 한다. 즉 오브젝트를 거처서 봐야한다.
- NotEqual: 같은 거리에 있지 않은 지오메트리를 드로우 한다. 
- Always: 뎁스 테스트가 실행되지 않는다 거리와에 관계없이 모든 지오메트리를 드로우한다.

기본적으로 둘다 Always인 경우 그려지는 순서에 따라 늦게 그련진게 화면에 표시되는거같다.

또한 ZWrite off는 강장 멀리있는 1값과 동일 하기 때문에
Zwrite Off와 ZTest Always와 Cull off를 쓰면 오브젝트 안에 있는 오브젝트를 통과해서 양쪽며이 다보이게 할수있다.

### ZWrite
ZWrite는 ZTest이후에 행하게 된다.
따라서 일단 쓰여있는 ZBuffer값을 내 깊이값과 비교해해서 그릴지 안그릴지 결정하고, 그 이후에 ZBuffer값을 기록(Write)한다.
결과적으로 ZTest에서 통과해야지 Zwrte를 할수있다.

ZWrite 깊이버퍼를 "쓰는것을" 끈다
즉, 깊이버퍼를 업데이트하지 않느다는 거다
즉, 이전그려진 객체 의 정보 그대로 유지한다
또한 ZWrite off 는 ZBuffer == 1로 가장 멀리있다는 의미를한다.

객체를 그릴때 값을 읽어오기한하고 쓰지는 않는다
[[유니티 shader Tags]]  에서 Queue
부분을 보면 숫자가 낮을수록 먼저렌더링 되는데
Background가 가장 낮으므로 Background가 그려지고 그위에 다른 것들이 "쓰여"야 하는데
"Zwirte Off"를 했으므로 "쓰지를" 못해서 2개다 그려졌지만 Background가 보이게된다.

이것을 쓰는경우는 Trnasparent를 쓰는 경우 뒤에 오브젝트뒤에 오브젝트도 그리기 위해 쓰인다.

또한, 거리상으로 멀리이있는 객체위에 세로운 객체를 그릴수도 있다.

