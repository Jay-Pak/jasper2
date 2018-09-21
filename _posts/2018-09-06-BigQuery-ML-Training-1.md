---
layout: post
current: post
cover: assets/images/Icons/Products-Services/Big-Data/BigQuery.svg
navigation: True
title: Big Query ML 뜯어보기
date: 2018-09-06
tags: [bigdata]
class: post-template
subclass: 'post'
author: bigquery
sitemap :
  changefreq : daily
  priority : 1.0
---

이번 포스트는 Big Query ML을 뜯어 보고자 한다. 



Bigquery ML을 위해서는 아래와 같이 2가지 Step을 거친다. 



## 1. Model Creation (모델 생성)

- Bigquery ML의 모델을 생성 하는 것은 익히 알고 있는 Create 구문과 비슷하며, Model 생성에 필요한 Option Parameter는 {Key = Value}의 형태로 사용하면 된다.



> ```sql
> [ CREATE MODEL | CREATE MODEL IF NOT EXISTS  | CREATE OR REPLACE MODEL ] model_name       
> OPTIONS
> (
> 	[Options..]
> )
> AS 
> [SQL_Statement]
> ```
>

- Options 

>

| NAME                         | VALUE                                                     | Details                                                      | Example                                                      |
| ---------------------------- | --------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **model_type**               | linear_reg<br />logistic_reg                              | 'linear_reg'는 [linear regression](https://developers.google.com/machine-learning/glossary/#linear_regression) model을 생성하는데 사용하는 Option이고, 'logistic_reg' [logistic regression](https://developers.google.com/machine-learning/glossary/#logistic_regression) model을 생성하는데 사용한다.해당 Option은 필수로 입력 해야 한다. | model_type=‘linear_reg’<br />model_type=‘logistic_reg’       |
| **input_label_cols**         | STRING                                                    | 학습에 이용되는 Label 컬럼의 이름을 배열로 열거 한다. 배열이라 해도 하나의 컬럼일 경우 하나의 Label 컬럼을 명시 할 수 있다. 만약, 이 Option이 지정되지 않은 경우 Query 하는 Dataset 컬럼 명 중, “label”이라는 이름이 된 컬럼의 경우 이것을 하나의 input_label로 인식한다. 그 마저 없다면, 모델 생성은 실패한다. Linear Regression 모델의 Label column은 실제 수치 Measure 값(숫자값)으로 하고, Logistics Regression의 경우 Cardinality가 낮은 컬럼을 대상으로 설정 하도록 한다. | inupt_label_cols=[‘col1’,’col2’]                             |
| **l1_reg**                   | FLOAT64                                                   | Model의 L1 정규화를 진행. 여기서 말하는 정규화는 Model의 가중치를  조절 하여, 모델 접합성을 향상 시킨다. 기본값은 **0**. | l1_reg=0                                                     |
| **l2_reg**                   | FLOAT64                                                   | Model의 L2 정규화를 진행. 여기서 말하는 정규화는 Model의 가중치를  조절 하여, 모델 접합성을 향상 시킨다. 기본값은 **0**. | l2_reg=0                                                     |
| **max_iterations**           | INT64                                                     | 최대 반복 학습 횟수 기본값은 **20**                          | max_iterations=20                                            |
| **learn_rate_strategy**      | line_search<br />constant                                 | Training 동안 특정 Learning rate(학습률)를 선정함에 있어, 전략을 선택한다. 2가지 Option을 선택할 수 있다. 기본 값은 ‘line_search’line_search Option은 학습 속도를 늦추고 처리되는 바이트 수를 늘리지 만, 지정된 초기 학습 속도가 더 높더라도 일반적으로 수렴된다는 점에서 트레이드 오프가 될 수 있다. 이 때 learn_rate_stretegy Option에 대해 설정 하지 않거나 line_search일 경우, is_init_learn_rate값을 통해 ml.learn_info에 나타나는 learn_rate의 값을 두배로 설정 하길 Recommand 한다. 최적의 초기 learning rate는 모델 마다 다르므로, 이점을 확일 할 필요가 있다. | learn_rate_strategy=‘line_search’<br />learn_rate_strategy=‘constant’ |
| **learn_rate**               | FLOAT64                                                   | learn_rate_strategy Option이 **constant**일 경우 경사 하강법([gradient descent](https://developers.google.com/machine-learning/glossary/#gradient_descent))의 learning rate를 설정 한다. 만약, learn_rate_strategy의 Option이 **line_search**일 경우 오류를 return 한다. 기본 값은 **0.1**이다. learn_rate_strategy가 ‘constant’일 경우 에만 사용하는 Option이다. | learn_rate=0.1                                               |
| **early_stop**               | BOOL                                                      | relative loss improvement(상대적 손실 개선)이 min_rel_progress 미만인 첫 번째 반복 이후에 학습이 중지되어야 함을 나타낸다. 기본값은 **ture** | early_stop=ture                                              |
| **min_rel_progress**         | FLOAT64                                                   | The minimum relative loss improvement necessary to continue training when early_stop is set to true. For example, a value of 0.01 specifies that each iteration must reduce the loss by 1% for training to continue. The default value is 0.01. |                                                              |
| **data_split_method**        | auto_split<br />random<br />custom<br />seq<br />no_split | 입력 데이터에 대해 Training과 Evaluation dataset을 나누는 방법에 대해 나타낸다. 여기서 Training data는 Model을 학습(training)시키는 데이터를 나타내고, Evaluation Data set은 training 이 후 early stop 시 [overfitting](https://developers.google.com/machine-learning/glossary/#overfitting)을 피하기 위해 Training Model의 검증의 용도로 사용된다. 기본값은 auto_split이다. **auto_split** : Training data와 Evaluation data를 자동으로 분할 해주는 Option이다. **random** : 말 그대로 Dataset을 임의적으로 나눈다. (다른 Training을 실시 하면, 서로 다른 Split 결과가 나타난다.)**custom** : BOOL이란 이름으로 사용자가 컬럼을 만들어 Training Data와 Evaluation Data를 분할 한다. 여기서 컬럼의 값이 ‘true’일 경우 evaluation data로 사용하고 ‘false’일 경우 training data로 사용 한다. **seq** : 사용자가 제공한 Column을 순차적으로 나눈다. 이때 순차적으로 사용 할 수 있는 Column은 NUMERIC, STRING, TIMESTAMP이다. 나머지 Row는(Null을 포함한 모든 값)은 Evaluation data로 활용된다. **no_split** : 모든 데이터를 Training data 로 사용한다. 만약, data_split_method Option을 명시 하지 않으면, 해당 Option은 auto_split으로 수행되며 auto_split은 아래와 같은 순서로 진행 된다.      1. 입력 데이터가 500개 이하라면, 모든 데이터를 Training Data로 활용 한다.      2. 입력 데이터가 500개에서 50000개 사이면 20%의 비율을 Evaluation Data로 활용한다.      3. 50000개 이상의 입력 Data의 경우 무작위로 10000개의 데이터를 나누어 Evaluation Data로 사용 된다. | data_split_method=‘auto_split’<br />ordata_split_method=‘random’<br />ordata_split_method=‘custom’<br />ordata_split_method=‘seq’<br />ordata_split_method=‘no_split’ |
| **data_split_eval_fraction** | FLOAT64                                                   | 이 옵션은 data_split_method Option이 ‘random’, ‘seq’ Option일 경우 사용한다. 이 Option은 평가에 사용된 데이터에 대한 소수점 값으로 해당 값은 평가에 대한 기준점역할을 한다. 기본값은 **0.2** | data_split_eval_fraction=0.2                                 |
| **data_split_col**           | STRING                                                    | 데이터를 나누는데 기준이되는 Column을 명시하며, 선택된 컬럼은 자동으로 feature Column에서 제외 된다.  data_split_method option이 ‘custom’일 경우 해당 Option에 명시되는 Column의 타입은 Boolean이여야 한다. 이 때, Row에 해당 Column이 true나 Null값이 있을 경우, 해당 컬럼은 Evaluation Data로 사용 되고, 나머지 false인 값은 Training데이터로 사용된다. data_split_method option이 ‘seq’일 경우 해당 Column의 마지막 data_split_fraction Row (최소값에서 최대값까지)이 평가 데이터로 사용된다. 첫번째 Row는 training data로 사용된다. | data_split_col=‘col1’                                        |
| **ls_init_learn_rate**       | DOUBLE                                                    | learn_rate_strategy=‘line_search’일경우 사용하며, 이 Option은 line_rate_strategy=‘line_search’일 경우만 사용 할 수 있다. |                                                              |
| **warm_start**               | BOOL                                                      | 이 Option은 새로운 Training Data와 새로운 모델의 옵션, 혹은 둘다 사용 하는데 이용된다. 명시 적으로 재 정의되지 않으면 모델을 학습하는 데 사용 된 초기 옵션이 Warm start 실행에 사용됩니다. 기본값은 **false**.Warm start 실행에서는 반복 횟수가 0에서 시작하도록 재 설정된다. training_run No. 또는 TIMESTAMP 열은 Warm start 실행을 원래 실행과 구별하는 데 사용될 수 있다. model_type과 label Option, 그리고 Training Data의 Schema의 변경은 Warm start에서 변경 할 수 없다. |                                                              |





## 2. Prediction (예측)

Prediction은 ML.PREDICT를 이용하여 예측결과를 도출 한다. 사용법은 아래와 같다. 

```sql
#StandardSQL
Select **predictive_label** // 예측 결과 Label 
	,  	col1
	,	col2 ... 
From ML.PREDICT(MODEL '[MODEL_NAME]', [SQL_Statement] )
```





다음 포스트는 실제로 사용하기 위한 예제를 포스팅 해보도록 하겠다. 