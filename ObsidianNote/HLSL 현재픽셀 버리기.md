#HLSL #unity #cllip #discard #shader 

```HLSL

clip(x)
x값이 0보다 작으면 이픽ㅓㅓ셀을 제거된다(무시된다)
clip(col.r - _CutoutThresh_)

discard
이것을 만나면 break 처럼 이픽셀을 제거된다(무시된다)
if(col.r < _CutoutThresh_) discard;
```