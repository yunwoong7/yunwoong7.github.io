---
layout: portfolio
title:  Face Mask Detector
date:   2020-06-08 12:03:22 +0300
image:  /assets/images/portfolio/face_mask_detector.png
author: Yunwoong Kim
tags:   ["detection"]
typora-root-url: ..
---

<div align="center">
  <img src="https://img.shields.io/badge/python-v3.6-blue.svg"/>
  <img src="https://img.shields.io/badge/opencv-v4.1.0.25-blue.svg"/>
  <img src="https://img.shields.io/badge/keras-v2.3.1-blue.svg"/>
  <img src="https://img.shields.io/badge/tensorflow-v2.2.0-blue.svg"/>
  <img src="https://img.shields.io/badge/dlib-v19.7.0-blue.svg"/>
</div>

<div align="center">
  <img src="/assets/images/portfolio/face_mask_dectector/face_mask_dectector.gif" width="70%">
</div>



## Description

<div align="center">
  <img src="/assets/images/portfolio/face_mask_dectector/face_mask_dectector_1.png" width="70%">
</div>
OpenCV, Keras / TensorFlow 및 Deep Learning으로 COVID-19 얼굴의 마스크 감지하는 프로그램을 개발하였습니다. 조금더 구체화 한다면 잠재적으로 개인과 타인의 안전을 보장하는 데 사용 할 수 있다고 생각합니다.



Python괴 Keras, TensorFlow를 이용하여 데이터 세트에서 안면 마스크 감지기 모델을 생성하는 스크립트를 작성합니다.



### 안면 마스크 데이터 세트는 어떻게 생성할까?

------

딥러닝, 머신러닝을 공부하다 보면 예제로 활용할 수 있는 데이터를 마련하거나 찾기가 어려워서 곤란할 때가 있습니다. 특히 라벨링이 된 이미지, 영상, 음성 등의 데이터의 경우 자체적으로 마련하기가 쉽지 않습니다.

저는 Landmark 를 이용하여 데이터 세트를 만들기로 했습니다.

1. 정상적인 얼굴 이미지 촬영
2. 얼굴을 찾아내는 Computer Vision 스크립트를 만들어 얼굴 이미지에 마스크를 추가하여 인공 데이타 세트를 만듬



<div align="center">
  <img src="/assets/images/portfolio/face_mask_dectector/face_mask_dectector_2.png" width="50%">
</div>

<i>얼굴을 감지하여 ROI (Region of Interest)를 추출</i><br>

<div align="center">
  <img src="/assets/images/portfolio/face_mask_dectector/face_mask_dectector_3.png" width="50%">
</div>

<i>얼굴 Landmark를 적용하여 눈, 코, 입등을 국소화</i><br>

아래와 같은 마스크 이미지 (투명한 배경 포함)가 필요합니다.

<div align="center">
  <img src="/assets/images/portfolio/face_mask_dectector/mask.png" width="30%">
</div>

마스크를 적용할 얼굴 최적의 위치(턱, 코를 따라 즉 포인트)를 계산하여 배치하여 이미지를 생성합니다.

<div align="center">
  <img src="/assets/images/portfolio/face_mask_dectector/face_mask_dectector_4.png" width="50%">
</div>

위의 프로세스를 반복하여 인공으로 마스크를 착용한 데이터 세트를 생성합니다.

마스크 이미지를 인공으로 생성한 이미지는 마스크가 없는 이미지로 **재사용**하면 안됩니다. 사용되지 않을 얼굴 이미지를 별도로 준비해야 합니다. 

<div align="center">
  <img src="/assets/images/portfolio/face_mask_dectector/face_mask_dectector_5.png" width="50%">
</div>



### Keras / TensorFlow로 COVID-19 마스크 인식 Model 생성

------

```python
[INFO] loading images...
[INFO] compiling model...
[INFO] training head...
Train for 34 steps, validate on 276 samples
Epoch 1/20
34/34 [==============================] - 30s 885ms/step - loss: 0.6431 - accuracy: 0.6676 - val_loss: 0.3696 - val_accuracy: 0.8242
Epoch 2/20
34/34 [==============================] - 29s 853ms/step - loss: 0.3507 - accuracy: 0.8567 - val_loss: 0.1964 - val_accuracy: 0.9375
Epoch 3/20
34/34 [==============================] - 27s 800ms/step - loss: 0.2792 - accuracy: 0.8820 - val_loss: 0.1383 - val_accuracy: 0.9531
Epoch 4/20
34/34 [==============================] - 28s 814ms/step - loss: 0.2196 - accuracy: 0.9148 - val_loss: 0.1306 - val_accuracy: 0.9492
Epoch 5/20
34/34 [==============================] - 27s 792ms/step - loss: 0.2006 - accuracy: 0.9213 - val_loss: 0.0863 - val_accuracy: 0.9688
...
Epoch 16/20
34/34 [==============================] - 27s 801ms/step - loss: 0.0767 - accuracy: 0.9766 - val_loss: 0.0291 - val_accuracy: 0.9922
Epoch 17/20
34/34 [==============================] - 27s 795ms/step - loss: 0.1042 - accuracy: 0.9616 - val_loss: 0.0243 - val_accuracy: 1.0000
Epoch 18/20
34/34 [==============================] - 27s 796ms/step - loss: 0.0804 - accuracy: 0.9672 - val_loss: 0.0244 - val_accuracy: 0.9961
Epoch 19/20
34/34 [==============================] - 27s 793ms/step - loss: 0.0836 - accuracy: 0.9710 - val_loss: 0.0440 - val_accuracy: 0.9883
Epoch 20/20
34/34 [==============================] - 28s 838ms/step - loss: 0.0717 - accuracy: 0.9710 - val_loss: 0.0270 - val_accuracy: 0.9922
[INFO] evaluating network...
              precision    recall  f1-score   support
   with_mask       0.99      1.00      0.99       138
without_mask       1.00      0.99      0.99       138
    accuracy                           0.99       276
   macro avg       0.99      0.99      0.99       276
weighted avg       0.99      0.99      0.99       276
```

<div align="center">
  <img src="/assets/images/portfolio/face_mask_dectector/plot.png" width="50%">
</div>

### OpenCV를 사용하여 이미지에 대한 COVID-19 마스크 감지기 구현

------

