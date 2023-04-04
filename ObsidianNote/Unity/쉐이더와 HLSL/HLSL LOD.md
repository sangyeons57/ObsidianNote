#unity/shader/LOD #language/HLSL 
[공식문서](https://docs.unity3d.com/Manual/SL-ShaderLOD.html)
SubShader에 LOD값을 할당할수있다. 

LOD는 (level of detail) 거리와 관련이 없다

LOD는 낮은 LOD 부터 SubShader를 실행시킨다.

수독으로 입력된 값으로 사용된다.
```HLSL
Shader "Examples/ExampleLOD"
{
    SubShader
    {
        LOD 200

        Pass
        {                
              // The rest of the code that defines the Pass goes here.
        }
    }

    SubShader
    {
        LOD 100

        Pass
        {                
              // The rest of the code that defines the Pass goes here.
        }
    }
}
```

숫자의 의미는 공식문서에 나와있다
