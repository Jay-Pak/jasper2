---
layout: post
current: post
cover: False
navigation: True
title: Big Query ML 이용해 보기 (1)
date: 2018-09-06 10:18:00
tags: [bigquery]
class: post-template
subclass: 'post'
author: google-cloud
sitemap :
  changefreq : daily
  priority : 0.1

---



> [1. Big Query ML 이용해보기 (1)](./Big-Query-ML-Use-(1))



이번 포스트에서는 Binary logistics Regressor를 이용한 Classification Model을 BigQuery ML을 이용하여 만들어 보고자 한다. 



앞서 이야기한대로 `BigQuery ML`은 Linear Regression(선형 회귀), Binary logistic regression(이진 로지스틱 회귀)의 모델을 생성 할 수 있다. `Linear Regressor`는 수치를 예측 하고자 할 때, `Binary logistics regression`은 분류 예측을 하고자 할때 사용한다. 

이번 Post는 Google Merchandise Store의 구매 자료를 토대로 방문자의 구매 확률에 대해 예측 하는 모델을 생성 하고, 실제 생성된 모델을 기반으로 실제 구매 여부 및 모델 평가(Model Evaluation)까지 해볼 것이다. 







# 1. 데이터 탐색 및 데이터 이해하기 (Understanding Business)

 우선 Machine Learning을 위해서는 데이터에 대한 이해 및 탐색. 그리고, 예측을 위해 필요한 데이터의 특성은 무엇인가에 대한 이해가 선행 되어야 한다. 아래 예제에서 사용하는  `data-to-insights.ecommerce.web_analytics` 는 BigQuery ML을 Test 하기 위해 Merchandise Store의 데이터를 공개한 것이다.

## 1. 방문자 대비 구매자

우선 데이터의 이해를 위해 전체 방문자 대비 구매자의 비율을 구하는 쿼리를 수행 해보도록 한다. 



>  
>
> ```sql
> #standardSQL
> WITH visitors AS(
> SELECT
> COUNT(DISTINCT fullVisitorId) AS total_visitors
> FROM `data-to-insights.ecommerce.web_analytics`
> ),
> purchasers AS(
> SELECT
> COUNT(DISTINCT fullVisitorId) AS total_purchasers
> FROM `data-to-insights.ecommerce.web_analytics`
> WHERE totals.transactions IS NOT NULL
> )
> SELECT
>   total_visitors,
>   total_purchasers,
>   total_purchasers / total_visitors AS conversion_rate
> FROM visitors, purchasers
> ```
>
>



 위의 데이터는 전체 방문자 대비 실제 구매자를 구하는 데이터이다. 위의 with절의 visitor는 방문자의 ID를 Count 하는 데이터이고, purchasers는 전체 데이터중 Transaction Flag가 있는 경우를 거래가 이루어진 것으로 보고, Count 한 데이터이다. 

<center>
    <img src="../assets/images/bigquery/Bigquery-ML-1.png"/>
	<br/>
    <em>< Fig. 1 - 방문자 대비 실제 구매자 ></em>
</center>



전체 방문자 수는 741,721명이며(이는 전체 방문 횟수가 아닌 전체 방문자의 수이다.) 이 방문자들 중 실제 구매 한 사람의 수는 20015명이다. 즉 
$$
(20,015\div741,721)\times100 = 2.69\%
$$
2.69%의 방문자가 Login 후 구매를 진행 하는 것을 알 수 있다. 



## 2. 매출과 수익 탐색

앞서 방문자와 실제 구매자를 알아 보았다. 하지만, 이것 만으로는 정보가 충분하지 않다. 방문자가 방문해서 구매를 했다 한들, 어떤 어떤 방문자가 어떤 카테고리의 제품을 찾았고, 어떤 제품을 구매 하였는가에 대한 탐색이 필요하다. 아래의 쿼리를 실행 해보자. 

>  
>
> ```sql
> #standardSQL
> Select prod_name
>   , prod_cat_name
>   , Format("%'d",CAST(units_sold AS int64))   as unit_sold
>   , Format("%'.2f",CAST(revenue AS float64))  as revenue
>   From (
>         SELECT
>           p.v2ProductName prod_name,
>           p.v2ProductCategory prod_cat_name,
>           SUM(p.productQuantity) AS units_sold,
>           ROUND(SUM(p.localProductRevenue/1000000),2) AS revenue
>         FROM `data-to-insights.ecommerce.web_analytics`,
>         UNNEST(hits) AS h,
>         UNNEST(h.product) AS p
>         GROUP BY 1, 2
>         ORDER BY revenue DESC
>         LIMIT 5
> );
> ```
>
>  
>
>
>
> <center>
>     <img src="../assets/images/bigquery/Bigquery-ML-2.png"/>
> 	<br/>
>     <em>< Fig. 2 - 매출 상위 5개 제품 ></em>
> </center>
>
>  
>
> >  위 Query는 전체 매출 중 가장 많이 팔린 상품 명과 수량에 대해, 상위 5개를 보여주는 쿼리이다.이때, From 절의 Inline View 쿼리 중 `UNNEST(hits)`, `UNNEST(h.product)`를 볼 수 있는데, 이는 해당 Table의 `Denormalize` 된 테이블의 Reapeat Column을 사용하는 것이고, Target Table의 Dataset의 1개의 Row에 대응되는 Repeat 컬럼에 대해 배열로 나열 하여, 연산 하는데 사용하려는 것이다. 다시 말해 Unnest Keyword는 Denormalize 된 Table의 Row에 대해 하나의 배열 타입으로 바꿔서 하나의 테이블로 인식 하는 역할을 해주는 것이다. 즉, Query 상에서 Table의 Row에 대해 1:N의 형태로 풀어 주는 역할을 한다. 
>
>  



## 3. 정리

우리는 지금까지 간단하게 나마, Google의 Merchandise Store의 Data를 가지고 방문자와 실제 구매자, 그리고 매출 상위 5개의 제품에 대해 탐색 해보았다. 이것으로 이해 할지는 모르겠지만, 위에서 보는 것 처럼 `데이터를 이해 하는 것은 Business를 이해` 하는 것과 같다고 볼수 있을 것이다. 

가령, 위의 Dataset으로 방문자가 Site에 접속 하여 구매로 이어 지는 Pattern에 대해 분석을 하고 싶다고 가정을 하자. 접속하여 구매까지 이어지는 Pattern을 알기 위해서는 우선 사용자가 접속하는 경로를 알 필요가 있다. 검색을 해서 들어 온다던지, 직접 url을 입력(즐겨찾기까지 포함하여...)한다던지에 대한 유입경로에 대해 알 필요가 있다. 해당 가입자가 어떤 channel을 통해 가입을 했는지에 대해 









