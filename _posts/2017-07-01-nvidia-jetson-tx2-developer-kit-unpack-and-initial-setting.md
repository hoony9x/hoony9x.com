---
title: NVIDIA Jetson TX2 Developer Kit 개봉기 및 초기 설정
author: hoony9x
date: 2017-07-01 14:06:34
image: assets/images/2017-07-01-nvidia-jetson-tx2-developer-kit-unpack-and-initial-setting/TX2_Module_170203_0017_TRANSP_2000px.png
categories:
  - "Experience"
  - "UCI I-SURF 2017"
tags:
  - "United States"
  - "UCI"
  - "I-SURF 2017"
  - "NVIDIA Jetson TX2"
---

NVIDIA Jetson TX2 Developer Kit 개봉기 및 초기 설정에 관한 글이다.

![http://www.nvidia.com/object/embedded-systems-dev-kits-modules.html](/assets/images/2017-07-01-nvidia-jetson-tx2-developer-kit-unpack-and-initial-setting/1.png)

Developer Kit 은 Jetson TX2 보드에 여러 가지 입출력 인터페이스가 장착된 보드가 함께 있는 것이다.

가격은 북미에서 $599 이며 만약 이 지역에서 학생 신분이라면 1회에 한하여 $299 에 구매가 가능하다.

TX2 의 사양은 다음과 같다.

![http://www.nvidia.com/object/embedded-systems-dev-kits-modules.html](/assets/images/2017-07-01-nvidia-jetson-tx2-developer-kit-unpack-and-initial-setting/2.png)

구성품들을 다 꺼내서 연결하면 다음과 같은 모습이 된다.

![NVIDIA Jetson TX2 개발보드 실물](/assets/images/2017-07-01-nvidia-jetson-tx2-developer-kit-unpack-and-initial-setting/3.jpeg)

기본 제공 운영체제는 Ubuntu 16.04 LTS 이며 초기 설정을 자동으로 해주는 JetPack 이라는 프로그램을 제공한다.

이 프로그램은 [여기](https://developer.nvidia.com/embedded/jetpack)에서 다운로드 받을 수 있으며 다운로드를 위해서는 로그인이 필요하다. 그리고 공식적으로는 Ubuntu 14.04에서 동작한다고 하지만 Ubuntu 16.04 에서 테스트를 해 본 결과 문제 없이 동작한다.

2017년 6월 30일 기준으로 해당 프로그램을 다운받으면 JetPack-L4T-3.0-linux-x64.run 라는 파일을 볼 수 있을 것이다.

- 2017년 7월 24일 기준 3.1 버전이 나왔다.

이 파일을 특정 directory에 넣고 다음 커맨드를 입력하여 초기 설정 작업을 진행한다.

```bash
$ chmod +x JetPack-L4T-3.0-linux-x64.run
$ ./JetPack-L4T-3.0-linux-x64.run
```

그러면 GUI로 Installer 가 뜨게 된다.

![Installer - 1](/assets/images/2017-07-01-nvidia-jetson-tx2-developer-kit-unpack-and-initial-setting/4.png)

Next를 누르다 보면 Select Development Environment 선택 화면이 뜬다.

![Installer - 2](/assets/images/2017-07-01-nvidia-jetson-tx2-developer-kit-unpack-and-initial-setting/5.png)

만약 다른 보드를 쓰고 있다면 알아서 잘 선택하도록 하자.

Next를 누르면 root 권한이 필요한지 비밀번호를 요구한다. 현재 로그인된 유저의 비밀번호를 입력하고 넘어가자.

그 다음에는 어떤 Component 를 설치할지 여부를 묻는 창이 뜬다.

![Installer - 3](/assets/images/2017-07-01-nvidia-jetson-tx2-developer-kit-unpack-and-initial-setting/6.png)

나는 잘 몰라서(…) 그냥 Full Install 을 선택했다.

Next를 누르면 Terms and Conditions 창이 뜬다. Accept All 누르고 넘어간다.

그러면 이제 필요한 요소를 자동으로 다운로드하게 된다.

다운로드가 완료 후 Next를 누르면 다음과 같은 화면이 나온다.

![Installer - 4](/assets/images/2017-07-01-nvidia-jetson-tx2-developer-kit-unpack-and-initial-setting/7-1.png)

컴퓨터-공유기, TX2-router/switch 상태로 연결되어 있으면 "Device accesses Internet via router/switch" 를 선택하면 된다.

컴퓨터와 TX2 보드가 직접 연결되어 있으면 "Device accesses Internet via host machine through setting up a new DHCP server configuration on host" 를 선택하면 된다.

Next를 누르면 Network Interface 선택을 하는 화면이 뜬다.

- "Device accesses Internet via router/switch" 를 선택한 경우 현재 router/switch 와의 통신이 이루어지는 interface를 선택하면 된다.
- "Device accesses Internet via host machine through setting up a new DHCP server configuration on host" 를 선택한 경우 선택해야 할 interface가 2개가 뜨는데, 각각 TX2와 연결된 interface와 인터넷과 연결된 interface를 선택하면 된다.

이후 계속 Next를 넘어가면 Terminal이 자동으로 실행되며 다음과 같은 내용이 표시된다.

![Installer - 5](/assets/images/2017-07-01-nvidia-jetson-tx2-developer-kit-unpack-and-initial-setting/8.png)

화면에 뜬 지시사항을 그대로 따라한 후 Enter를 누르면 자동으로 초기화가 진행된다.

각 보드에 있는 입출력장치 및 버튼은 Developer Kit 패키지에 동봉된 설명서를 참고하거나 NVIDIA 홈페이지를 참고하면 된다.

초기화가 완료되면 Terminal 창을 닫으라는 메세지가 Terminal상에 표시된다. 창을 닫으면 설치가 완료되었다는 창이 뜨게 된다.

여기까지가 NVIDIA에서 제시하는 초기 설정 방법이다.

여기까지 문제 없이 진행을 했다면 이제 보드를 사용할 준비가 된 것이다.

다음 글에서는 DVFS (Dynamic Voltage and Frequency Scaling) 에 대해서 다룬다.
