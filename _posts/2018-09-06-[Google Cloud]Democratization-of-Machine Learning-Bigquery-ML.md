---
layout: post
current: post
cover: False
navigation: True
title: Democratization of Machine Learning - Bigquery ML
date: 2018-09-06 10:18:00
tags: [bigquery]
class: post-template
subclass: 'post'
author: google-cloud
comments: true
sitemap :
  changefreq : daily
  priority : 0.1
---

지난 7월 미국 San Francisco Mountain - View에서 있었던 Google Next'18 에서 Big Query ML에 대한 Announce 가 있었다. 이번 포스팅에서는 Big Query ML에 대한 간략한 소개에 대해 이야기 해보고자 한다. 

<center>
    <img src="https://lh3.googleusercontent.com/-yYQHr3BnoKU/WvsR07QaYiI/AAAAAAAABgA/V2WfyrpctnInmJb-9B9E-MuMQ7ehPdzkgCJoC/w530-h298-n-rw/NEXT_2018_COLOR_TWITTER%2B%25284%2529.gif"/>
    <br/>
	<em>Google Next</em>
</center>



BigQuery ML은 말 그대로 Big Query 내에서 Machine Learning Model을 생성 할 수 있는 서비스이다. 기업의 데이터 분석가들은 데이터를 활용한 분석 및 예측을 위해서는 R, Python 등 언어를 습득해야 하고, 데이터에 대한 이해도가 높은 사람만이 할 수 있어, 소수 인원의 전유물 처럼 여겨 졌다. 

이에 Google은 개발 언어를 배우지 않고도, 쉽게 Machine Learning Model을 생성 할 수 있도록 SQL만으로 데이터를 분석 할수 있는 Platform인 BigQuery에 Machine Learning Model을 생성 할 수 있도록 서비스하기 시작하였다. 이와는 별도로, Cloud Auto ML을 발표 하였는데, Cloud AutoML은 인식 시스템의 기계 학습 / AI를 사용한 사용자 정의 기계 학습 모델 구축을 On-Premise에서 실행할 수 있도록하고있다. Cloud AutoML은 이미지 내의 물체 인식 / 분류뿐만 아니라 문서의 분류, 번역에 대응 했다.



아래는 BigQuery ML의 Model 생성 및 이를 이용한 예측을 하는 방법을 간단히 설명하는 Image이다. 

<center>
    <img src="https://cloud.google.com/images/products/bigquery/bigquery-ml.gif"/>
    <br/>
	<em>[ 출처 : Google Cloud Big Query ML Documentation ]</em>
</center>



### 1. Create Support Model

- Linear Regression - 연속성을 가지는 수치 데이터에 대해 예측을 위한 모델.

- Binary Logistics Regression - 쉽게 말해 Classification. A인가? B인가? 혹은 어느 군집에 속하는가를 판별 하는 모델.





### 2. Quota 및 Limitation & Pricing

- BigQuery ML은 Machine Learning을 위한 Model을 생성하는데 쓰이는 Query에 대한 Quota를 따른다. Create Model에 대한 Quota는 없다. 자세한 내용은 [Big Query Quotas and Limits](https://cloud.google.com/bigquery/quotas)에서 확인 바란다.



### 3. 사용 가능 환경

- BigQuery UI

- bq CLI Tool

- Big Query Rest API 

- Jupyter Notebook 또는 BI Platform과 같은 외부 플랫폼



다음 포스팅은 Big Query ML을 간단히 사용해보는 포스팅을 해보고자 한다. 