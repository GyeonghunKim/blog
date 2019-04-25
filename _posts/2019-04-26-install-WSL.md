---
title: "윈도우 10에 WSL 설치하기"
categories: 환경구축
tags:
  - WSL
  - Window10
  - Ubuntu
  - 윈도우

last_modified_at: 2019-04-26T13:00:00+09:00
toc: true 
toc_label: "Table of Contents"
toc_icon: "cog" 
toc_sticky: true 
author_profile: true
comments: true

gallery1: 
  - url: /assets/images/WSL_install/000.PNG
    image_path: /assets/images/WSL_install/000.PNG
    alt: "placeholder image "
    title: "Image 1 title caption"

gallery2: 
  - url: /assets/images/WSL_install/001.PNG
    image_path: /assets/images/WSL_install/001.PNG
    alt: "placeholder image "
    title: "Image 1 title caption"

gallery3: 
  - url: /assets/images/WSL_install/002.PNG
    image_path: /assets/images/WSL_install/002.PNG
    alt: "placeholder image "
    title: "Image 1 title caption"

gallery4: 
  - url: /assets/images/WSL_install/003.PNG
    image_path: /assets/images/WSL_install/003.PNG
    alt: "placeholder image "
    title: "Image 1 title caption"

gallery5: 
  - url: /assets/images/WSL_install/004.PNG
    image_path: /assets/images/WSL_install/004.PNG
    alt: "placeholder image "
    title: "Image 1 title caption"

gallery6: 
  - url: /assets/images/WSL_install/005.PNG
    image_path: /assets/images/WSL_install/005.PNG
    alt: "placeholder image "
    title: "Image 1 title caption"

gallery7: 
  - url: /assets/images/WSL_install/006.PNG
    image_path: /assets/images/WSL_install/006.PNG
    alt: "placeholder image "
    title: "Image 1 title caption"
---

## 1. 설치 가능 조건
WSL을 설치하기 위해서는 64비트 PC, Windows 10 1607 이상이면 가능합니다. 본인 컴퓨터의 윈도우 버전을 확인하기 위해서는 아래의 경로로 들어가면 됩니다. 

> 설정 > 시스템 > 정보   

그러면 아래와 같이 사양을 확인할 수 있습니다.   

{% include gallery id="gallery1" %}


## 2. WSL설치하기 
WSL의 설치과정은 크게 세 단계로 나누어집니다. 첫 번째 단계는 WSL의 설치를 위해서 윈도우 기능을 켜는 것, 두 번째 단계는 window store에서 ubuntu를 설치하는 것, 그리고 마지막은 설치 된 ubuntu를 실행해 계정을 만드는 것 입니다. 

### 1. 윈도우에서 Linux용 하위 기능 켜기
윈도우에서 Linux용 하위 기능을 켜기 위해서는 "Windows 기능 켜기/끄기"를 검색해서 들어가면 됩니다. 그 후에 아래 그림과 같은 "Linux용 Windows 하위 시스템"을 체크 하면 됩니다. 이제 첫 단계가 끝났습니다!

{% include gallery id="gallery2" %}

### 2. Window store에서 ubuntu 설치하기
이후에 window store에 접속해서 ubuntu를 설치하면 됩니다. 아래 그림과 같은 아이콘을 실행한 뒤, 무료 > 설치 를 클릭해 설치를 하면 됩니다. 

{% include gallery id="gallery3" %}

### 3. Ubuntu 계정 만들기
위의 설치가 마무리 되었다면 설치 된 응용프로그램 Ubuntu를 실행합니다. 그러면 다시 설치중이니 기다리라는 메시지가 나옵니다. 

> Installing, this may take a few minutes... 

수 분을 기다린 뒤에 아래 이미지와 같이 계정을 등록하라는 문구가 나오면 user id와 passwd, passwd 확인을 순차로 입력하면 됩니다. 

{% include gallery id="gallery4" %}

그러면 설치가 모두 끝났습니다!

{% include gallery id="gallery5" %}


## 3. WSL 기본 사용법

WSL환경에서 기존의 우분투 shell환경과 다른 것이 있다면 윈도우 파일에 접근이 가능하다는 것 입니다. 물론 그 외에도 사소한 차이가 있긴 합니다만, 일반적으로 관련 에러메시지와 WSL을 함께 검색하면 해결 방법이 구글에 나와 있습니다. 
WSL환경을 사용하는 것이 기존의 VMware를 사용하는 것에 비해 어떤 점이 나아졌냐고 한다면 가장 큰 것은 windows파일에 직접 접근이 가능하다는 것 입니다. 

### 1. 윈도우 파일에 접근하기
설치한 Ubuntu shell에서 윈도우 파일에 접근하기 위해서는 아래와 같이 **/mnt**를 이용하여 접근할 수 있습니다. 

> /mnt/c/Users/[유저이름]/[윈도우 상의 디렉토리 이어서]

예를 들어서 유저 이름이 aass98998인 계정의 바탕화면에 접속하고 싶다면 아래와 같이 디렉토리를 이동할 수 있다. 

> cd /mnt/c/Users/aass98998/Desktop/

그러나 항상 mnt를 이용하여 긴 디렉토리 명을 적는 것은 매우 귀찮고 이를 해결하기 위해서 **ln -s**를 이용한다. 

> ln -s "/mnt/c/Users/aass98998/Desktop/somefolder/" /home/ghkim/somefolder

이 결과를 테스트 하기 위해서 윈도우 바탕화면에 somefolder 폴더를 만들고 그 안에 test.txt 파일을 아래와 같이 만들었다. 

{% include gallery id="gallery6" %}

그 위에 Ubuntu shell에서 

> cd /home/ghkim/somefolder  
> vim test.txt

를 실행하면 아래와 같이 성공적으로 윈도우 디렉토리 파일에 접근한 것을 확인할 수 있다. 

{% include gallery id="gallery7" %}