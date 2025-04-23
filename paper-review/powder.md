---
description: >-
  Nan Jiang, Bangjie Sun, and Terence Sim, National University of Singapore; Jun
  Han, KAIST
---

# Can I Hear Your Face? Pervasive Attack on Voice Authentication Systems with a Single Face Image

**논문 요약**:\
Foice는 단일 얼굴 이미지만을 사용해 피해자의 음성 샘플 없이도 음성을 합성하여 WeChat, Microsoft Azure, Siri 등 음성 인증 시스템을 속이는 새로운 딥페이크 공격 기법이다. 얼굴과 음성의 생물학적 상관관계를 활용하며, 소셜 미디어에서 쉽게 얻을 수 있는 얼굴 이미지를 이용해 공격의 확장성과 위협성을 높인다. Foice는 Train Phase에서 신경망을 학습하고, Attack Phase에서 합성 음성을 생성해 인증 시스템을 우회한다.

***

#### **세부 내용**

**1. Train Phase**

Train Phase에서는 신경망을 학습시켜 얼굴 이미지에서 음성 특징을 추출하고 합성할 수 있는 모델을 구축한다.\
**구성**: Data Processing → Face-dependent Voice Feature Extractor → Face-independent Voice Feature Generator

**Data Processing**

* **설명**: 온라인 공개 데이터셋(VoxCeleb1, VoxCeleb2 등)을 전처리한다.
  1. **Face Processing**:
     * **Face Cropping & Normalization**: 얼굴을 탐지해 크롭하고, 눈의 위치를 기준으로 크기와 방향을 정규화한다.
     * **Blurriness Assessment**: Sobel edge detection으로 이미지 선명도를 평가하며, 품질 점수가 100 미만인 흐린 이미지는 제외한다. 단, Foice는 저해상도 이미지에서도 효과적이다(§5.5.3).
  2. **Voice Processing**:
     * Speaker Encoder를 사용해 음성 녹음에서 voice feature vector(F\_GT)를 추출한다.
     * NISQA 모델로 음질을 평가해 고품질 음성만 선별하며, 이는 F\_GT로 사용되어 Face-dependent Voice Feature Extractor와 Face-independent Voice Feature Generator의 학습에 활용된다(§5.2.2).
* **수정**:
  * 원문에서 "이렇게 얻어낸 feature vector는 deep-learning model의 ground truth(f\_gt) 역할"은 맞지만, F\_GT가 **Face-dependent Voice Feature Extractor**와 **Face-independent Voice Feature Generator**에서 구체적으로 어떻게 사용되는지 명시해야 한다.
  * Voice Processing은 단순히 feature vector 추출뿐 아니라 음질 필터링(NISQA)을 포함한다.
* **개선점 (보완)**:
  * 원문의 제안(음성 노이즈 제거)은 적절하며, 이는 SV2TTS의 노이즈 민감성 문제를 피할 수 있는 Foice의 장점과 연결된다(§5.2.1).
  * 추가로, 데이터셋의 다양성(예: 다양한 언어, 억양)을 확보하면 모델의 일반화 성능이 향상될 수 있다.

**Face-dependent Voice Feature Extractor**

* **설명**:
  * 얼굴 이미지(Img\_face)에서 음성과 상관된 특징(F\_dep)을 추출한다: F\_dep = C\_f->v(E\_face(Img\_face)).
  * **E\_face**: ResNet 같은 CNN으로, 성별, 나이, 입술 모양 등 음성과 관련된 얼굴 특징(F\_face)을 추출한다.
  * **C\_f->v**: F\_face를 음성 특징(F\_dep)으로 변환한다.
  * 학습은 F\_dep와 ground truth 음성 특징(F\_GT) 간의 거리 기반 loss 함수(Err(F\_dep, F\_GT))를 최소화하며, E\_face와 C\_f->v를 공동으로 훈련한다.
  * 결과적으로, 성별(예: 남성의 낮은 피치), 나이(예: 고령자의 얇은 음성) 등 얼굴과 상관된 음성 특징을 추출한다(Observation 1, §A.2.1).
  * 실험적으로, Foice는 성별 정보를 90% 이상 정확도로 추출했다(§A.2.1, Figure 17).
  * 입술, 턱선 등 특정 얼굴 특징이 피치와 포먼트 주파수에 영향을 미친다(§5.6).

**Face-independent Voice Feature Generator**

* **설명**:
  * 얼굴로 추론할 수 없는 음성 특징(F\_indep)을 생성하고, F\_dep와 결합해 완전한 음성 특징(F\_recon)을 재구성한다.
  * **F\_indep 생성**: Bottleneck(B(·))은 F\_GT를 입력받아 F\_indep를 출력한다(F\_indep = B(F\_GT)). F\_indep는 성대, 비강 구조 등 얼굴과 무관한 특징만 포함해야 하므로, bottleneck dimension(최적: 48)을 조절해 F\_dep를 필터링한다(Observation 2, §A.2.2).
  * **F\_recon 생성**: Reconstructor(R(·,·))는 F\_indep와 F\_dep를 결합해 F\_recon을 생성한다(F\_recon = R(F\_indep, F\_dep)).
  * **학습**: 두 가지 loss를 최소화한다:
    1. **Reconstruction error**: F\_recon과 F\_GT의 차이(Err(F\_recon, F\_GT))를 최소화.
    2. **KL-divergence loss**: F\_indep가 표준 정규분포(𝒩(0, I))를 따르도록 유도해 연속적인 search space를 생성. 이는 학습 데이터에 없는 피해자의 F\_indep도 포함한다.

***

**2. Attack Phase**

* **설명**:
  * 피해자의 얼굴 이미지 한 장을 사용해 음성 인증 시스템을 공격한다.
  * **F\_dep 생성**: 학습된 E\_face와 C\_f->v로 Img\_face에서 F\_dep를 추출한다.
  * **F\_indep 샘플링**: 표준 정규분포(𝒩(0, I))에서 N개의 F\_indep를 무작위로 샘플링한다.
  * **F\_recon 생성**: Reconstructor로 F\_dep와 각 F\_indep를 결합해 N개의 F\_recon\_i = R(F\_indep\_i, F\_dep)를 생성한다.
  * **음성 합성**: Voice Synthesizer는 F\_recon\_i와 텍스트 입력(예: WeChat 인증 숫자, "Hey, Google!")을 받아 N개의 합성 음성을 생성한다.
  * **공격 실행**: N개의 음성을 외부 스피커로 순차적으로 재생해 인증 시스템을 우회한다. 낮은 임계값(0.5\~0.6)과 무제한 시도를 악용한다(§2.2).
  * 실험 결과, WeChat Voiceprint는 10명 모두 우회되었으며, 평균 30%의 합성 음성이 성공했다(§5.2.1).
  * 공격은 인증 시스템의 취약점(낮은 임계값, 무제한 시도)을 활용한다.

***

**3. 왜 모델이 작동하는가?**

* **설명**:\
  Foice가 작동하는 이유는 다음과 같다:
  1. **얼굴-음성 상관관계**: 얼굴 특징(성별, 나이, 입술 등)은 음성 특징(피치, 포먼트 주파수 등)과 생물학적으로 상관관계가 있다(§2.3). Face-dependent Voice Feature Extractor는 이를 학습해 F\_dep를 정확히 추출한다. 예를 들어, 입술과 턱선은 피치와 포먼트 주파수에 영향을 미친다(§5.6, Figure 15). 실험적으로, 성별 정보는 90% 이상 정확도로 추출되었다(§A.2.1).
  2. **Search space의 일반화**: Face-independent Voice Feature Generator는 bottleneck을 통해 F\_indep를 표준 정규분포로 생성, 연속적인 search space를 만든다. 이는 학습 데이터에 없는 피해자의 음성도 포함하며, N을 늘릴수록 실제 음성에 가까운 F\_recon을 찾는다(§4.4, Observation 2). 최적 bottleneck dimension(48)은 F\_dep를 최소화한다(§A.2.2, Figure 18).
  3. **인증 시스템 취약점**: 낮은 임계값(0.5\~0.6)과 무제한 시도는 Foice의 성공률을 높인다(§2.2).
  4. **견고성**: Foice는 저해상도 이미지, 얼굴 가림, 5회 시도 제한에서도 효과적이다(§5.5).
*
  * 실험 결과: Foice는 WeChat(100% 성공), VGGVox(67.6%), DeepSpeaker(87.7%)를 우회했다(§5.2).
  * Foice는 SV2TTS와 결합 시 성공률을 3배 향상시켰다(§5.4, Figure 11).
  * Face-TTS 대비 30\~60배 높은 성공률을 달성했다(§A.3, Table 4).

***

**4. 실험 결과**

* **대상**: WeChat, Siri, Google Assistant, Bixby(온디바이스), Microsoft Azure, iFlytek, VGGVox, DeepSpeaker(클라우드).
* **성과**:
  * WeChat: 10명 모두 우회, 평균 30% 성공률(§5.2.1).
  * 클라우드: VGGVox(67.6%), DeepSpeaker(87.7%), Microsoft/iFlytek에서 SV2TTS와 유사/우수(§5.2.2).
  * Foice는 성별/나이 외의 정보(피치, 포먼트 주파수)를 활용, Descriptive Attack(9.7%)보다 3배 높은 29.7% 성공률(§5.3).
  * Augmentation Attack(Foice + SV2TTS)은 모든 시스템에서 SV2TTS 단독보다 우수(§5.4).
* **견고성**: 저해상도, 얼굴 가림, 5회 시도에서도 효과적(§5.5). 입술/턱선이 피치/포먼트 주파수에 큰 영향(§5.6).

***

**5. 개선점 및 윤리적 고려**

* **개선점**:
  * 음성 데이터 노이즈 제거 전처리 강화.
  * 3D 얼굴 이미지나 음성 샘플 결합으로 정확도 향상(§6).
  * 딥페이크 탐지와 라이브니스 탐지 통합(§6).
* **윤리적 고려**:
  * IRB 승인, 참가자 프라이버시 보호, 데이터 익명화/삭제(§A.4).
  * 취약점 책임 공개로 기업의 수정 조치 촉진.
  * 음성 인증 시스템의 새로운 위협 경고 및 대책 개발 촉구(§8).

***

#### **결론**

당신의 요약은 Foice의 핵심과 구조를 잘 잡아냈지만, **F\_indep 생성 과정**, **F\_recon 입력 오류**, **실험 결과 및 윤리적 고려 누락** 등의 문제가 있었습니다. 수정된 요약은 기술적 정확성을 높이고, 실험 결과와 윤리적 맥락을 추가해 논문의 전체적인 가치를 반영했습니다. 추가 질문이나 특정 부분에 대한 심화 설명이 필요하면 말씀해주세요!
