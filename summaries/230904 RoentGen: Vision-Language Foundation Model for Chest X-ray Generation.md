https://arxiv.org/abs/2210.04133
- Adapting Pretrained Vision-Language Foundational Models to Medical Imaging Domains

- 요약: Stable Diffusion (SD)모델을 어떻게 fine-tuning해야 Medical domain (Chest x-ray)에 잘 적용할 수 있는지에 대해 연구한 논문. (Text를 condition으로 넣을때는 normal이라는 단어만 넣어야할지? a photo of a lung x-ray라고 문장을 넣어야할지? fine-tuning은 vae만 해야할지 unet만 해야할지? 등 선택의 폭이 많은데, X-ray 합성 task에 한해 정리가 잘 되어있다.)

#Abstract
- 자연영상과 비교했을 때 의료영상에 대한 언어는 매우 좁고 다르며, 동시에 의미론적으로는 풍부하고, 도메인에 특정한 단어를 사용한다. => 자연영상에 학습된 image-text 사용불가
- Fine-tuning한 모델을 Data augmentation 방법으로 사용했을 때 합성 이미지+실제 이미지 = classification accuracy 5% 향상 / only 합성 이미지 (하지만 학습셋이 많음) = classification accuracy 3% 향상이 측정됨
- Pneumothorax 등의 특정 질병에서 25%정도 representation 능력이 향상
#Introduction
- 의료영상에 판독문이 있긴 하지만 여전히 자연 언어로 표현될 수 있는 것보다 훨씬 더 좁음
- 사전학습된 text-to-image LDM의 vision-language 얽힘을 활용하면 관련 의료 키워드 또는 관심 개념을 제시하여 합성 의료 영상 데이터를 생성하는 직관적인 메커니즘을 제공할 수 있음
1. 의료 도메인이 적용된 text-to-image 모델을 평가하는 방법을 제시
   - 사전학습된 classifier를 사용한 분류
   - Radiology report 생성
   - Image-to-image and text-to-image retrieval
3. UNet과 CLIP text encoder를 모두 fine-tuning하면 가장 좋은 결과를 낼 수 있음
4. 원래의 CLIP text encoder를 도메인 특정한 text encoder로 바꾸면 fine-tuning 이후 성능이 향상됨. (text encoder freeze하고 UNet만 학습한 경우)
5. SD fine-tuning 작업은 UNet 학습 시 in-domain knowledge -> text encoder로 추출이 가능하여 희귀 질병과 등의 표현 능력을 향상시킬 수 있음
6. Data augmentation tool로서는 1k~5k 정도의 작은 데이터셋으로 fine-tune가능하며, 합성 데이터만 사용할시 분류 성능은 비슷했지만, 실제 영상과 합쳐서 학습했을 때 5% 향상을 보임

#Related Work
<SD로 CXR 생성한 연구>  
- text prompt를 통해 single class 이미지 합성을 위해 few-shot 학습 (SD fine-tuning)
- Real 영상에 학습한 분류기가  생성된 pathology를 95% 정확도로 구별할 수 있음 증명
- GAN에 비해 multi-label 생성을 얼마나 잘하는지 확인
- Class-conditional image generation에 집중. Real vs Real+Synthesized multi-label 분류에서 mean AUROC -9.7% (합성데이터를 합쳤더니 -9.7%)
- Downstream 태스크 자체를 비교한 논문은 없었음
<다른 모달리티>
- breast MRI segmentation에서 Dice score +0.04를 얻었으나, 그러나 임상 실습에 사용되는 것보다 훨씬 낮은 해상도로 이러한 모델을 학습했음에도 불구하고 방사선 전문의가 평가한 50개 스캔 중 32%에서 주요 해부학적 불일치 보임
#Datasets
- MIMIC-CXR dataset
  - 모든 radiology report는 Findings/ Impression으로 나뉨.
  - Findings: normal / abnormal
  - Impression: medical decision을 위한 findings 해석
- 이 연구에서는 impression 섹션을 입력으로, 7~77개의 character만을 가진 데이터만 사용
- Findings는 더 길기 때문에 (77개 이하로 줄이려면 40%가 없어짐) 사용안한듯
- “No Finding” 판독문 수를 제한함 (10,005개)
- PA학습 / PA+AP+LAT학습 등 여러개 학습데이터셋을 구축.
#Stable Diffusion Fine-Tuning

