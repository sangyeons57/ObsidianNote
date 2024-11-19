
모델링을 만들기위한 기본 재료 가장 크게 영향을 미친다고 한다.
Stable Diffusion의 **Checkpoint**는 모델의 특정 학습 상태를 저장한 파일로, 주로 `.ckpt` 또는 `.safetensors` 확장자로 제공됩니다.
각 체크포인트는 특정 데이터셋과 학습 조건을 기반으로 한 모델 상태를 나타내며, 이로 인해 생성 이미지의 스타일과 품질이 달라집니다.
- **기본 체크포인트**: 예를 들어 `Stable Diffusion 1.5`와 같은 기본 체크포인트는 광범위한 이미지 데이터셋으로 학습되며, 일반적인 이미지 생성에 적합합니다.
- **Fine-Tuned Checkpoints**: 특정 스타일이나 테마에 최적화된 체크포인트도 있으며, 예를 들어 애니메이션 스타일, 특정 색감, 사진 스타일 등을 강조하도록 미세 조정된 모델이 있습니다.

스타일 설정에 주요하다

체크포인트 사용할떄 비율이 다르면 문제가 발생할수있다.
### Checkpoint 설정 방법

1. **Checkpoint 파일 로드하기**:
    
    - Stable Diffusion Web UI에서 Checkpoint를 설정하려면 먼저 원하는 Checkpoint 파일을 다운로드하여 `models/Stable-diffusion` 폴더에 저장해야 합니다.
    - Web UI의 설정 메뉴에서 원하는 Checkpoint를 선택할 수 있으며, UI에서 선택한 Checkpoint에 따라 이미지 생성 스타일이나 세부 사항이 변경됩니다.
2. **해상도와 Checkpoint의 호환성 확인**:
    
    - 모델의 학습 데이터와 이미지 해상도가 다를 경우 왜곡이나 품질 저하가 생길 수 있으므로, **해상도 설정**을 Checkpoint가 훈련된 해상도에 가깝게 조정하면 좋습니다.
3. **세부 설정 조정**:
    
    - **CFG Scale** (Classifier-Free Guidance Scale), **Sampling Steps**, **Sampler Method** 등의 설정을 통해 Checkpoint의 출력을 세밀하게 조정할 수 있습니다.
    - ControlNet이나 LoRA와 같은 확장 기능이 지원되는 경우, 추가 설정을 통해 더욱 복잡한 이미지 생성이 가능합니다.




# CheckPoint 종류
##  Realistic Vision V6.0 B1
[civitAI](https://civitai.com/models/4201/realistic-vision-v60-b1)
**Sampler = DPM SDE++ Karras or another / 4-6+ steps  
CFG Scale = 1.5-2.0 (the lower the value, the more mutations, but the less contrast)**

**I also recommend using ADetailer for generation (some examples were generated with ADetailer, this will be noted in the image comments).**