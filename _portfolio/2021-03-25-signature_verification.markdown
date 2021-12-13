---
layout: portfolio
title:  "Signature Verification"
date:   2021-03-25 13:54:11 +0300
image:  /assets/images/portfolio/signature_verification/signature_verification.png
author: Yunwoong Kim
tags:   ["Deep Learning"]

typora-root-url: ..
---



<h2 align="center">
  Signature Verification
</h2>

<div align="center">
  <img src="https://img.shields.io/badge/python-v3.8-blue.svg"/>
  <img src="https://img.shields.io/badge/PyQt5-v5.14.1-blue.svg"/>
  <img src="https://img.shields.io/badge/pandas-v1.2.3-blue.svg"/>
  <img src="https://img.shields.io/badge/keras-v2.4.3-blue.svg"/>
  <img src="https://img.shields.io/badge/tensorflowv-v2.4.0-blue.svg"/>
</div>

다양한 Form 양식에서의 서명 검증 솔루션

------



## 1) 이미지 전처리 (특징 점 기반)

- Fome Image는 템플릿 양식 이미지
- Input Image는 Sacn된 이미지

<div align="left">
  <img src="/assets/images/portfolio/signature_verification/signature_verification_1.png" width="70%">
</div>

- 이미지 정렬 알고리즘은 feature-based 방법과 keypoint detectors (DoG, Harris, GFFT, etc.), local invariant descriptors (SIFT, SURF, ORB, etc.), and keypoint matching (RANSAC and its variants)을 이용
- 두 개의 이미지에서 키포인트를 감지하고 키포인트를 일치 시키고 대응을 결정

<div align="left">
  <img src="/assets/images/portfolio/signature_verification/signature_verification_2.png" width="70%">
</div>

<div align="left">
  <img src="/assets/images/portfolio/signature_verification/signature_verification_3.png" width="50%">
</div>
<div class="video-container">
<iframe width="1280" height="720" src="https://www.youtube.com/embed/GKFl84EGfWI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

## 2) ROI (Region of interest)

### 각 필드의 좌표를 설정하여 위치 기반의 영역을 추출

```python
ROILocation = namedtuple("ROILocation ", ["id", "bbox"])
ROI_LOCATIONS = [ROILocation ("signiture1", (1260, 425, 280, 40)),
                 ROILocation ("signiture2", (1260, 525, 280, 40)),
                 ROILocation ("signiture3", (1260, 620, 280, 40)),
                 ROILocation ("signiture4", (1260, 1550, 280, 40))]
```

<div align="left">
  <img src="/assets/images/portfolio/signature_verification/signature_verification_4.png" width="70%">
</div>

### Classification dataset

<div align="left">
  <img src="/assets/images/portfolio/signature_verification/signature_verification_5.png" width="100%">
</div>

## 3) Machine Learning Model

- Convolutional Neural Network를 이용하여 서명/미서명 여부를 예측하는 모델을 생성

- CNN architecture는 "SmallerVGGNet"을 이용

  <div align="left">
    <img src="/assets/images/portfolio/signature_verification/signature_verification_6.png" width="50%">
  </div>

<div align="left">
  <img src="/assets/images/portfolio/signature_verification/signature_verification_7.png" width="70%">
</div>



- 100 epoch 을 수행하였으며 Training set에서 99.04%, Testing set에서 97.14%의 성능으로 측정