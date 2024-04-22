# Abstract
- 판독문을 Plain language로 바꾸는 실행 가능성 확인.
- 전체 138건 중 약 37%에 대해 ChatGPT는 보고서의 결과를 기반으로 구체적인 제안을 제공
- 너무 정보가 없을 때는 랜덤하게 뱉어냈지만, prompt를 디테일하게 작성하면 해결가능.

## Experimental design
- `Please translate a radiology report into plain language that is easy to understand.`
- `Please provide some suggestions for the patient.`
- `Please provide some suggestions for the healthcare provider.`

##  Performance evaluation
- Overall score, completeness, correctness로 판단.
- 영상의학과 전문의들이 1~5 사이의 score로 점수를 매기고, 없어진 부분이 있다면 비율로 정량화함.
- Suggestion evaluation으로는 많이 질문된 제안, 특정 finding이 있을때의 특정 질문 비율, report와 관계없는 질문을 기록.

## Key results
- ChatGPT는 fewer words를 사용했다. - 특히 많은 섹션에서 no abnormality를 나타낼 때.
- 모든 Negative findings를 한 문장으로 표현했다.
- 좀더 환자 친화적으로 작성되어 이해하기 쉬웠다. (의학 용어를 쉬운 용어로 대체)
- 정보를 잘 통합함.

## Evaluation of ChatGPT-generated suggestions
- patients에 대한 답과 healthcare providers 사이에 상관관계가 높다. (maintain a healthy life <-> Encourage a healthy life)
- 하지만, 판독문에 따른 독특한/특별한 대답은 없었다.
- 흡연기록에서 1년에 30팩을 30년으로 오표기
- mild~로 시작하는 작은 질환은 무시해버림.

## Optimized prompt for improved translation
- 같은 input을 주어도 대답이 항상 다름 - 언어모델의 uncertainty.
- 다음과 같이 질문을 자세하게 바꿈:
- `First paragraph introduces screening description including reason for screening, screening time, protocol,
patient background, and comparison date`
- `Second paragraph talks about specific findings: how many nodules detected, each lung nodule’s precise
position and size, findings on lungs, heart, pleura, coronary artery calcification, mediastinum/hilum/axilla,
and other findings. Please don’t leave out any information about findings`
- `Third paragraph talks about conclusions, including overall lung-rads category, management recommendation
and follow-up date, based on lesion`
- `Ff there are incidental findings, please introduce in the fourth paragraph.`

- ChatGPT-4에서 성능이 더 늘어남

# Discussions
- ChatGPT를 사용하면 conciseness, clarity, comprehensiveness가 증가함.
- 애매한 prompt를 주면 너무 간소화를 하며 중요한 정보를 빼먹는다.
- ChatGPT는 built-in template이 없다. - Clear한 instruction과 format을 설정하면 좋아질듯?
- 최적화된 prompt가 완성도를 증가시킬 순 있지만, 완벽하진 않다.
