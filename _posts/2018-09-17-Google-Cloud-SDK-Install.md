---
layout: post
current: post
cover: assets/images/Icons/Products-Services/Developer-Tools/Cloud-SDK.svg
navigation: True
title: Google Cloud SDK Install
date: 2018-09-17
tags: [getting-started]
class: post-template
subclass: 'post'
author: google-cloud
sitemap :
  changefreq : daily
  priority : 0.1
---



Google Cloud는 Cloud위의 Resource를 관리할 수 있는 2가지 도구를 제공한다. 하나는 [`Cloud Web Console`](https://console.cloud.google.com)이고, 또 다른 하나는 `CLI(Command Line Interface : 명령표시줄)`로 관리할 수있는 `Cloud SDK`가 그것이다. 뿐만아니라 Google은 Cloud 위의 자원을 관리하기 편하도록 `RestAPI`를 이용한 도구를 제공하여 자원의 Deploy를 Automation 할 수 있도록 Library를 제공한다. (Java 혹은 PIP, NodeJS etc.). Cloud SDK는 Cloud Resource를 관리 함에 있어 편리하고 빠르게 관리 할 수 있는 방법을 제공한다.

여기서는 Cloud SDK를 설치하는 방법에 대해 설명한다.



# 1. Prerequire

Google Cloud SDK는 Python으로 Programing되어 있으며, 모든 Environment에 Python이 설치 되어 있다는 것을 가정한다. 아래 명령으로 Python이 설치 되어 있는가를 확인 하도록 한다. 

```shell
python --version

python3 --version 
```

<center>
    <img src="https://cdn.steemitimages.com/DQmRpuiYWzFLS4JQmF9NjiofhNo9VNWhchcijXhJGnmTcRz/1.png" align="center"/>
    <br/>
	<em>Fig. 1 - Python Version 확인</em>
</center>



# 2. Installation

### Windows

1. Google Cloud SDK Installer를 [Download](https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe) 받는다.

2. Installer의 Guide에 따라 설치를 진행한다. 

3. 명령 Prompt를 실행하여, 아래와 같이 실행하여, 설치 여부를 확인한다. 

   ```shell
   gcloud --version
   ```



### Linux / Mac OSX

Windows와 같이 Download 받아서 진행 할 수 있지만, 여기에서는 curl을 이용하여 Download부터 설치까지 진행 해보도록 한다.

Terminal을 열어 아래와 같이 명령을 실행 하여 본다. 

```shell
$ curl https://sdk.cloud.google.com | bash
```

<center>
    <img src="https://cdn.steemitimages.com/DQmdSLYbPNEuEffCiovsnyWSmJRcBJ6KS4V9knijGtxi4N7/2.png"/>
	<br/>
    <em>Fig. 2 - Cloud SDK 설치</em>
</center>

- 설치 진행 후 아래와 같이 Cloud SDK Bug report를 보내겠냐는 내용이 나오는데, Y를 입력 하고, Google Cloud SDK설치를 진행 하도록 한다.  

<center>
    <img src="https://cdn.steemitimages.com/DQmYVB8tpPY4GCZpaeC75Z3o8BESTCNbCLH3deKxhXN6KQJ/3.png"/>
	<br/>
    <em>Fig. 3 - Bug Report 동의 여부</em>
</center>

- 해당 과정이 지나면, 본격적으로 Google Cloud SDK 설치 및 Update가 진행 된다. 

<center>
    <img src="https://cdn.steemitimages.com/DQmdFuixQbd8hcQ7XJTNDoADPXvz6RENV5fWTupF2vMGVi9/4.png"/>
	<br/>
    <em>Fig. 4 - 설치 진행</em>
</center>

- 이때 설치 및 Update이후 환경 변수 PATH에 추가 하겠냐는 Prompt가 나오는데 이때 Y를 입력 해서 Path에 추가 해주도록 한다. 그렇지 않으면 Cloud SDK를 실행 할때 마다 Terminal에서 해당 경로로 이동하여, 실행 해야하는 불편함이 있다. 

<center>
    <img src="https://cdn.steemitimages.com/DQmXVyDKgafNnNBXcr7NsdYf8pL6efKSXhbs42D82qog2jD/5.png"/>
	<br/>
    <em>Fig. 5 - 환경 변수 추가</em>
</center>

- 이 과정 까지 끝나면 gcloud --version을 입력 하여 제대로 설치 되었음을 확인 한다. 하지만 바로 실행 되지는 않는다. 앞서 말했다시피 Cloud SDK 설치 이후 환경 변수에 등록은 되었으나 바로 실행 되지는 않는다. 바로 실행을 하고 싶다면 아래 와 같이 진행 하면 바로 gcloud 명령을 확인 할수 있다. 

<center>
    <img src="https://cdn.steemitimages.com/DQmaZtPkRJVrhFozL1d4o9XqQkVdr3P2SZFHB9ZRDRehh3v/6.png"/>
	<br/>
    <em>Fig. 6 - 설치 확인</em>
</center>



이렇게 Cloud SDK 설치가 완료 되었다. 다음은 Cloud SDK를 사용한 Cloud 자원을 이용하는 방법에 대해 하나하나 해보도록 하겠다. 
