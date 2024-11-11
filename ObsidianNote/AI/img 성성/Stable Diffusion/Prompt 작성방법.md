prompt에는
Positive Prompt :  AI가 해당 요소를 포함하도록 하는 Propmpt
Negative Prompt : AI가 해당 요소를 배제하도록 하는  Prompt
가있다.


## Prompt
### 강조
() 소괄호를 사용할떄마다 해당 태그를 강조할수 있다.

### 비율
비율, resolution, ratio 등에 해당하는 것들도 상당히 중요하다
너무 작거나 큰경우에 퀄리티가 나쁠수있다.

## Positive Prompt 확장

### Tagger

## Embedding
[영상1](https://www.youtube.com/watch?v=kdmaex0W_RI&list=PLKuQxQX8EZn3WK9uZQpdy8cdcF0lwHiGA&index=8)
**Embedding**은 텍스트나 이미지를 고차원 벡터로 변환하여, 모델이 이를 이해하고 연관성 또는 유사성을 계산할 수 있도록 만드는 과정입니다.
Stable Diffusion에서의 Embedding은 특정한 스타일, 개체, 혹은 개념을 학습하여 모델이 이를 기억
하게 하고, 프롬프트에서 호출할 때마다 해당 속성이나 스타일을 이미지에 반영할 수 있게 합니다.

Stable Diffusion에서는 Embadding폴더에 해당 파일을 넣어서 Texttual Inversion 에서 설정한다.
### Embedding의 작동 방식
1. **학습**: 특정 개념이나 스타일에 해당하는 이미지를 모델이 학습하여 벡터 표현을 생성합니다. 이를 통해 모델은 해당 개념을 기억하게 됩니다.
2. **호출 및 사용**: 프롬프트에 Embedding 이름을 입력하면, 모델이 이 벡터 표현을 불러와 학습한 스타일이나 속성을 이미지에 반영합니다.

즉 모델이 학습 한 데이터를 사용할수있게 파일로 만들어논것


## LoRA(Low-Rank Adaptation of Large Language Models)
LoRA(**Low-Rank Adaptation of Large Language Models**)는 대규모 AI 모델을 효율적으로 미세 조정할 수 있는 기법입니다. 특히 Stable Diffusion에서, LoRA는 일부 가중치만을 조정해 특정 스타일, 인물, 또는 특성을 모델에 추가할 수 있게 합니다.

즉, 필요한 부분만 변경시킬떄 LoRa를 쓴다.

LoRA주의점
- **학습 데이터 품질**: 잘못된 데이터로 LoRA를 학습하면 원하지 않는 왜곡이 발생할 수 있습니다.
- **과적합 방지**: 너무 적은 이미지나 과도한 학습 스텝은 특정 특성에 과적합될 위험이 있습니다.
- **조합의 제한**: 여러 LoRA를 조합할 때 충돌이 발생할 수 있어, 실험을 통해 조합의 효과를 확인하는 것이 중요합니다.
- **모델 호환성**: LoRA는 특정 Stable Diffusion 모델과 호환되지 않을 수 있으며, 사용하기 전 호환 여부를 확인하는 것이 좋습니다.

LoRA를 잘 활용하면 모델에 다양한 스타일이나 기능을 추가하면서도 성능과 속도를 유지할 수 있습니다.
/models/Lora 폴더에 LoRA파일을 넣으면 준비를 할수 있습니다.


## Hires.fix
일반적으로 업스케일링을 할때 쓰는데
일반적으로  Upscaler는 아레 2가지를 쓰다고하는데 직접 사용해봐야 알것 같다.
R-ESRGAN 4x+ Anime6B : 애니메이셔느낌
R-ESRGAN 4x+: 실사느낌
