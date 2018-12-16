---
layout: post
current: post
cover: False
navigation: True
title: Google Cloud Load Balancer
date: 2018-12-03
tags: [infrastructure]
class: post-template
subclass: 'post'
author: compute-engine
sitemap :
  changefreq : daily
  priority : 1.0
---

# Google Cloud Load Balancer 

## Load Balancer? 

Load Balancer(부하 분산기)는 왜 필요 할까? 만약, 이글을 읽고 있는 당신이 인터넷 Shopping Mall을 운영 한다고 가정을 하자. 인터넷 쇼핑몰을 운영 하기 위해서는 당연한 이야기지만 Web Application이나 Mobile App이 필요 할 것이다. 그리고, 주문을 받거나, Contents를  저장할 Database나 Storage등이 필요 할 것이다. 여기서 만약 쇼핑몰이 장사가 잘 안된다면? 그리고, 들어오는 사람이 많이 없다면? 좋은 인프라가 필요 없을 것이다. 하지만, 시간이 흘러 입소문을 타면서 쇼핑몰에 들어오는 사람이 많아진다면? 쇼핑몰 운영자인 당신은 Hardware의 Up-grade(Scale-up)을 먼저 생각 할 것이다. 하지만 이것은 임시 방편에 불과 하다. Hardware의 성능을 더 올린다고 해서 

오늘은 Load Balancer에 대해 이야기 해보려고 한다. 

Google Cloud의 Network Load Balancer는 Region Base로 지원 하며, L3, L7 Loadbalencer를 지원한다. 

* Cloud Shell 
   - Cloud Shell은 사용자가 필요한 모든 개발 Tool이 설치된  VM(Virtual Machine)을 불러 온다. 
   - 5GB의 Persistant Disk를 가지고, Google Cloud위에서 동작 하며, 네트워크 성능과 인증을 크게 향상 시킨다. 
   - Cloud Shell에 처음 연결 하면 이미 인증이 된 상태를 볼수 있고, 자신의 PROJECT_ID가 설정 되어 있고, 확인은 아래 명령으로 확인 할 수 있다. 
     ​    -- gcloud  auth list 
     ​              -- Credentialed accounts:
     ​                   - <myaccount>@<mydomain>.com (active)    
     ​    -- gcloud config list project 
     ​              -- [core]
     ​                  project = <PROJECT_ID>

** gcloud <Cloud SDK>
   - `gcloud`는 Google Cloud Platform을 위한 강력 하며, 통일된 Command-list Tool 이다. 자세한 내용은 https://cloud.google.com/sdk/gcloud에서 확인 할 수 있다. 이것은 이미 Cloud shell에 이미 설치 된 상태로 제공 되며, Local PC에 설치를 위해서는 이 블로그의 <Cloud SDK 설치>를 참조 하기 바란다. 



--------------------------------------------------------------------------------------------------------------------------

** 모든 Resource를 위한 Default  Region과 zone 설정 하기 
​      1. Cloud shell에서 Default Region or Zone을 설정 하려면 
​       - gcloud config set compute/zone <zone_name>
​       - gcloud config set compute/region <region_name>
​          *** Google이 제공하고 있는 Region/Zone의 List를 확인 하고 싶다면, 
​          ​        - gcloud compute regions list 
​          ​        - gcloud compute zones list 
​          1-1. Region 설정 하기 
​        - gcloud config set compute/zone asia-northeast1-a 
​        - gcloud config set compute/region asia-northeast1
​          *** 개인 PC에서 gcloud를 사용할 때, config setting은 여러 session에서 지속적으로 적용 된다. Cloud Shell에서는 새로운 연결이나 재 연결시 설정을 해주어야 한다. 


** Multiful Web Server Instance  생성 하기
​      1. Cluster 환경에서 Service를 Test하려면,. Instance Template 및 Managed Instance Group을 사용하여 nginx Web Server로 간단한 Cluster를 생성해서  Test하면 된다. Instance Template은 Cluster 내에 모든 Virtual Machine의  Template을 정의 한다. (Disk, CPU, Memory등) Managed Instance Group은 
​      Instance Template을 이용하여 Virtual Machine을 Instance 화 한다. 

nGinx Web server Cluster 를 만들기 위해서는 다음과 같은 준비가 필요하다. 
​                1. 모든 Virtual Machine Instance가 시작 될 때 Nginx Server 를 설정하는데 사용되는 Start up Script 
​                2. 시작 Script를 사용하기 위한 Instance Template
​                3. Target Pool 
​                4. Instance Template을 사용한 Managed Instance Group 

1. Nginx Server 설정을 위한 Start up Script 만들기 
      ​     - Cloud Shell을 열어 아래와 같은 절차로 shell File을 생성 한다. 

     ================================================
     cat << EOF > startup.sh
     #! /bin/bash
     apt-get update
     apt-get install -y nginx
     service nginx start
     sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/'   \
     /var/www/html/index.nginx-debian.html
     EOF
     ================================================


2. Instance Template을 생성한다. 
     ================================================
     gcloud compute instance-templates create nginx-cluster-template \

     --metadata-from-file startup-script=startup.sh 
     ================================================

3. Target pool 생성 
     Target Pool은 모든 Instance Template을 이용하여 생성된 VM Instance에 대해 단일한 Access Point를 제공/허용하며, 이후 Load Balancer에서 필요하다. 
     ================================================
     gcloud compute target-pools create nginx-pool
     ================================================

4. 앞서 생성한 Instance Template을 이용한 Managed Instance Group 생성 
     ================================================
     gcloud compute instance-groups managed create nginx-group \
     --base-instance-name nginx \
     --size 2 \
     --template nginx-cluster-template \

     --target-pool nginx-pool 
     ================================================

위와 같은 절차를 거치고 나면 `nginx-`라는  prefix로 2개의 instance가 생성이 된것을 볼 수 있다. 

모든 작업이 완료 되고 나면, 아래 명령으로 VM Instance 생성을 확인 해 보도록 한다. 
​     ================================================
​     gcloud compute instances list 
​     ================================================

외부 IP를 통해 80 port로 연결 할 수 있는 방화벽을 구성하도록 한다. 
​     ================================================
​     gcloud compute firewall-rules create www-firewall --allow tcp:80
​     ================================================
작업이 완료 되고 나면 각 instance의 외부  IP를 통해 Instance에 연결 할 수 있어야 한다. local pc 에서 http://EXTERNAL_IP를 통해 확인 해보록 한다. 


Newwork Balancer 생성하기 (L3)
Network Load Balancer를 이용하면  IP Address, Port 및 Protocol 유형과 같이 인입되는 IP Protocol Data를 기반으로 System의 부하를 분산할 수 있다. 
하지만, HTTP Load Balancer와 함께 사용할 수 없는 몇가지 Option이 있다. 
예를 들면, SMTP Traffic과 같이 추가 적인 TCP/UDP기반 Protocol을 Local Balancing 할 수 있다. 그리고 Application이 TCP-Connection-related 관련 특성에 대해 HTTP Network Load Balencer와는 달리  Application 이 Packet을 검사 할 수 있다. 자세한 내용은 https://cloud.google.com/load-balancing/docs/network/에서 확인 할 수 있다. 

아래 Script를 통해 앞서 생성한 Instance Group에 L3(Network Layer) Network Load Balancer를 생성한다. 
​     ================================================
​      gcloud compute forwarding-rules create nginx-lb \
​         --region asia-northeast1 \
​         --ports=80 \
​         --target-pool nginx-pool
​     ================================================
생성 후 Instance group의 forwarding rule을 확인 한다. 
​     ================================================
​     gcloud compute forwarding-rules list
​     ================================================

위 Command를 통해 확인된 IP Addess를 통해 접속이 가능한지 확인하도록 한다. 
​     ================================================
​     NAME     REGION       IP_ADDRESS     IP_PROTOCOL TARGET
​     nginx-lb  us-central1  X.X.X.X              TCP                    us-central1/targetPools/nginx-pool
​     ================================================
Browser를 열어 http://[IP_ADDRESS]를 통해 확인 해보록 한다. 


HTTP(S) Load Balancer 생성하기 (L7)
HTTP(S) Load Balancer는 Compute Engine Instance에 인입 되는 HTTP(S) 요청(Request)에 대해 Global Load Balancing을 지원 한다. 일부의  URL을 하나의 Instance Set으로 Routing 하고, 다른 URL을 다른 Instance로 Routing 하는 URL Rule을 구성 할 수있다. 요청들은 조건에 부합하고, 충분한 capacity가 있는 경우 항상 가까운 instance group에 Routing 된다. 가장 가까운 Instance Group 의 용량이 충분 하지 않은 경우 가장 가까운 Instance Group으로 전달 된다. 자세한 내용은 https://cloud.google.com/compute/docs/load-balancing/http/ 에서 확인 해보도록 한다. 


1. Health check 생성. 
     - Health Check는 HTTP/HTTPS Traffic에 응답하는지 확인 한다. 
       ================================================
       gcloud compute http-health-checks create http-basic-check 
       ================================================

2. HTTP Service를 정의하고, Port 이름을 Instance Group에 관련된 Port에 Mapping 한다. 
     ================================================
     gcloud compute instance-groups managed \
       set-named-ports nginx-group \

       --named-ports http:80
     ================================================

3. Back-end Service 생성 
     ================================================
     gcloud compute backend-services create nginx-backend \

     --protocol HTTP --http-health-checks http-basic-ckeck --global 
     ================================================

4. back-end Service에 instance Group을 추가 한다. 
     ================================================
     gcloud compute backend-services add-backend nginx-backend \
     --instance-group nginx-cluster-group \
     --instance-group-zone asia-northeast1-a \

     --global 
     ================================================

5.  들어오는 모든 Request들을 모든 Instance로 보내는 Default URL Map을 만든다. 
     ================================================
     gcloud compute url-maps create web-map \

     --default-service nginx-backend
     ================================================

6. 앞서 생성한 Default URL Map에 요청을 Route하는 Target HTTP Proxy를 생성한다. 
     ================================================
     ```
     gcloud compute target-http-proxies create http-lb-proxy \
     --url-map web-map
     
     
     ```

7. 인입되는 요청을 처리하고 Route하는 Global forwarding Rule을 만든다. 전달 규칙은 IP Address및 Protocol 및 지정 Port에 따라 Traffic을 특정 Target HTTP/HTTPS Proxy 로 보낸다. Global Forwarding Rule은 여러 포트를 지원하지 않는다. 
     ================================================
     gcloud compute forwarding-rules create http-content-rule \
        --global \
        --target-http-proxy http-lb-proxy \--ports 80

      Global forwarding Rule을 만든 후에 Config가 Provisioning되기까지는 시간이 걸릴 수 있다. 아래에서 확인 하도록 한다. 


     gcloud compute forwarding-rules list
     ================================================
     전달 규칙에 대해 http-content-rule에 명시된 IP_ADDRESS를 기억 한다. 

browser 에서 http://IP_ADDRESS를 통해 연결을 시도 해보도록 한다. 모든 전달 규칙이 적용 되는데는 몇분이 걸리기때문에 안 될 시에 다시 load를 해보도록 한다. 