---
layout: portfolio
title:  "Paperless Hospital"
date:   2021-12-15 16:36:13 +0300
image:  /assets/images/portfolio/paperless_hospital/paperless_hospital.png
author: Yunwoong Kim
tags:   ["RPA", "detection"]

typora-root-url: ..
---



<h2 align="center">
  Paperless Hospital
</h2>

<div align="center">
    <img src="https://img.shields.io/badge/kotlin-v1.6.10-blue.svg"/>
  <img src="https://img.shields.io/badge/python-v3.8-blue.svg"/>
  <img src="https://img.shields.io/badge/PyQt5-v5.14.1-blue.svg"/>
  <img src="https://img.shields.io/badge/firebase_admin-v5.0.3-blue.svg"/>
  <img src="https://img.shields.io/badge/opencv_python-v4.5.4.60-blue.svg"/>
  <img src="https://img.shields.io/badge/pillow-v8.4.0-blue.svg"/>
</div>

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/paperless_hospital.png" width="100%">
</div>



## 골칫거리 "종이서류"

동네 병원을 가보면 데스크의 서랍과 가려진 벽면 뒤에는 진료 종이서류로 빼곡하게 쌓여 있습니다. **병원의 종이차트 의무보관 기간은 5년**으로, 늘어나는 환자 수 만큼 종이차트 역시 늘어나기 있고 병원은 보관 방법과 폐기 문제를 놓고 골머리를 앓고 있습니다.

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/paper.png" width="70%">
</div>


그나마 재정에 여유가 있는 대형 병원들은 수십억원의 예산을 투자하여 디지털 사업을 추진하고 있지만 중/소규모의 병의원에서는 단독으로 프로젝트를 외주에 줄만큼 양이 되지 않기 때문에 외부 사업자가 일을 맡지도 않을뿐더러 전문성이 없거나 비싼 비용을 지불해야 하는 경우가 대부분입니다.

특정 의료장비의 경우는 직접 전자 문서화된 데이타로 보관 할 수 있지만, 노후화된 의료장비나 장비의 특성상 출력을 필요로 하는 경우는 종이서류로 존재하게 됩니다. 



## Mobile App 하나로 진료접수부터 서류관리까지

**Paperless Hospital**은 Mobile App을 통해 촬영 후 이미지를 환자별로 서버에 저장 관리하고 검색하는 것이 목적입니다. 나아가 진료접수 부터 예진표 작성까지 App을 통해 비대면으로 처리함으로써 진료 시간이 지체되지 않도록 하기 위함입니다.



<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/app_loading.gif" width="200px">
</div>

------



## 기능

### 1) 병원관리 (계정/환자등록)

Google 계정 연동(최초 1회 ) 및 병원, Device 관리 등을 할 수 있습니다. 환자 등록의 경우는 Step으로 구성되어 있습니다. 

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/login.png" width="100%">
</div>

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/add_patient.gif" width="100%">
</div>



### 2) 이미지 등록/조회

등록된 환자에 카메라 촬영/갤러리에서 선택 등을 통해 이미지를 추가 할 수 있습니다. 촬영한 이미지는 자동으로 SCAN 이미지 형태로 변환 할 수 있으며 여러장의 이미지 추가가 가능합니다. 

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/add_camera.gif" width="100%">
</div>



### 3) 전자 예진표 작성

전자 예진표의 양식은 서버에서 관리하며 양식의 정보(순서, 항목 추가 등)가 변경되면 실시간으로 반영됩니다. 따라서 예진표를 미리 출력해놓거나 변경으로 인해 출력해놓은 이전 버전의 예진표를 폐기할 필요가 없습니다. 전자 예진표 작성 시에는 저장된 환장의 정보는 자동으로 입력 처리되어 작성 시간을 단축합니다.

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/form1.png" width="100%">
</div>

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/write_form.gif" width="100%">
</div>



### 4) 진료접수

종이로 작성하는 진료증을 전자로 대체로 합니다. Mobile App에서 진료 접수를 입력하면 실시간으로 PC에 연동되어 접수 담당자가 확인 후 처리가 가능합니다.

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/sync.png" width="100%">
</div>



### 5) PC에서 Mobile App 연동

PC에서 Mobile App\ 화면 전환이 가능하며, Mobile App에서 입력한 정보는 PC로 실시간 전송됩니다.

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/send.gif" width="100%">
</div>



### 6) 이미지 확인/예진결과 작성 (PC)

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/imageview.png" width="100%">
</div>



### 7) 이미지 프린트

<div align="center">
  <img src="/assets/images/portfolio/paperless_hospital/print.png" width="100%">
</div>

------

*<시연 영상 보기>*

<div class="video-container">
<iframe width="1075" height="753" src="https://www.youtube.com/embed/7gj2POhsHMg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

