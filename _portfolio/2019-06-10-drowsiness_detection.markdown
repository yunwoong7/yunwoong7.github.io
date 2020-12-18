---
layout: portfolio
title:  Face Mask Detector
date:   2019-06-10 15:11:43 +0300
image:  /assets/images/portfolio/drowsiness_detection.png
author: Yunwoong Kim
tags:   ["detection"]
typora-root-url: ..
---

현대 도시에서 살아가기 위해 꼭 필요한 자동차, 현재 한국에 등록된 자동차는 2,344만대를 넘어섰고 인구 2.2명당 자동차 1대를 보유한 셈입니다. 그리고 늘 뉴스에 오르내리는 기사 중 하나가 바로 ‘졸음운전 사고’입니다. 특히 졸음운전의 교통사고 치사율은 전체 교통사고 치사율보다 2배가량 높다고 합니다. 대부분 장시간에 걸쳐 장거리 운전을 하는 사이 피로가 누적되며 졸음운전으로 이어지기 때문입니다. 

<div align="center">
  <img src="https://img.shields.io/badge/python-v3.6-blue.svg"/>
  <img src="https://img.shields.io/badge/opencv-v4.1.0.25-blue.svg"/>
  <img src="https://img.shields.io/badge/keras-v2.3.1-blue.svg"/>
  <img src="https://img.shields.io/badge/tensorflow-v2.2.0-blue.svg"/>
  <img src="https://img.shields.io/badge/dlib-v19.7.0-blue.svg"/>
</div>

<div align="center">
  <img src="/assets/images/portfolio/drowsiness_detection/drowsiness_detection_1.gif" width="70%">
</div>



## Description

OpenCV, Keras / TensorFlow 및 Deep Learning으로 자동차 운전자가 피곤해 지거나 운전대에서 잠들 었는지 감지하는 데 사용할 수 있는 졸음 감지 프로그램을 개발 했습니다.



***운전대에서 잠든 적이 있습니까?***



### 졸음 감지 알고리즘

------

1. 연속 영상에서 얼굴을 인식

   <div align="left">
     <img src="/assets/images/portfolio/drowsiness_detection/drowsiness_detection_2.gif" width="70%">
   </div>

   
2. 얼굴 Landmark를 적용하여 눈, 코, 입등을 국소화

   <div align="left">
     <img src="/assets/images/portfolio/drowsiness_detection/drowsiness_detection_3.gif" width="70%">
   </div>

   
3. 눈 영역을 추출

   <div align="left">
     <img src="/assets/images/portfolio/drowsiness_detection/drowsiness_detection_4.gif" width="70%">
   </div>

   <i>이제 눈 영역이 있으므로 눈이 닫혀 있는지 확인하기 위해 눈 종횡비를 계산할 수 있습니다.</i><br>

   

4. 특정시간동안 종횡비가 낮다면 운전자를 깨우기 위한 알람을 표시

   <div align="left">
     <img src="/assets/images/portfolio/drowsiness_detection/drowsiness_detection_5.gif" width="70%">
   </div>
