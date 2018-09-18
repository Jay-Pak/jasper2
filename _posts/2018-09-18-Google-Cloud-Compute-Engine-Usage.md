---
layout: post
current: post
cover: False
navigation: True
title: Google Cloud Compute Engine(GCE) 사용 해보기 (1)
date: 2018-09-06 10:18:00
tags: [getting-started]
class: post-template
subclass: 'post'
author: google-cloud-compute-engine
sitemap :
  changefreq : daily
  priority : 1.0


---

이번 Post에서는 Google Cloud의 자원 중 Compute Engine을 사용하는 방법에 대해 알아 보도록 하겠다. [Google Cloud Compute Engine(GCE)](https://cloud.google.com/compute/)은 Google Cloud 내에 [가상 머신(Virtual Machine=VM)](https://ko.wikipedia.org/wiki/%EA%B0%80%EC%83%81_%EB%A8%B8%EC%8B%A0#%EA%B0%80%EC%83%81_%EB%A8%B8%EC%8B%A0_%EC%9D%91%EC%9A%A9_%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4)으로 Google Cloud의 [Iaas(Infrastructure as a Service)](https://ko.wikipedia.org/wiki/%EC%84%9C%EB%B9%84%EC%8A%A4%EB%A1%9C%EC%84%9C%EC%9D%98_%EC%9D%B8%ED%94%84%EB%9D%BC%EC%8A%A4%ED%8A%B8%EB%9F%AD%EC%B2%98)이다. Compute Engine을 이용하면, Google의 Peta byte급의 Network 및 Persistant Disk 등의 자원을 저렴하게 이용할 수 있고, 높은 수준의 [SLA(Service Level Agreement)](https://cloud.google.com/compute/sla)를 보장한다. 

Compute Engine의 하나의 VM은 Instance라 부르고, 하나의 인스턴스는 Group으로 묶어 Instance Group으로 운영을 할 수 있다. 그리고, VM Instance는 SnapShot Image를 통해 

# Google Cloud Platform Console에서 Compute Engine Instance 생성

Google Cloud의 Compute Engine