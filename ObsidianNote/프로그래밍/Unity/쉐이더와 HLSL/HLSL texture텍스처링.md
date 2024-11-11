#unity/shader/texture #language/HLSL 
```txt
samler2D _MainTex 와
float4 _MainTex_ST 는
같이쓰이는거 같기는한데 뒤에 _ST 를 붙이면 tile 사용할때 여러게 할때 사용하기위해 
자동으로 지정되 언어 같다.

```
텍스처링은 fragmetn부분에서 하는거같다.

```HLSL
float4 col = tex2D(_MainTex, IN.uv);
return col;

(텍스쳐, 좌표) 해당 픽셀에 칼라 반환 이런 느낌 인거 같다.
또한 텍스쳐는 좌표가 초과해도 반대편 좌표로 자동계산이 되는거같다.

이걸이용해서

uv 값에 시간을 덜해서 흐르는 텍스처를 만들수도있다.

또한 월드 좌표로 만들어서 고정되 텍스쳐를 오브젝트를 움직이면서
볼수도있다.

또한

tex2Dlod()
라느게 있는데 level of detail을 수동으로 조정할수있게 해주는거같다.
(텍스쳐, float4(좌표x, 좌표y, lod x, lod y))
이런느낌이거같다

이걸로 수동으로 lod를 조정해서 텍스쳐가 뿌여게 보이게 할수있다.

```