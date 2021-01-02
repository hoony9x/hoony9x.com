---
title: UCI I-SURF 2017 참가 1주차 정리 글
author: hoony9x
date: 2017-07-03 21:13:09
image: assets/images/2017-07-03-uci-i-surf-2017-1st-week/IMG_2701.jpg
categories:
  - "Experience"
  - "UCI I-SURF 2017"
tags:
  - "United States"
  - "UCI"
  - "I-SURF 2017"
---

I-SURF 2017 참가자로 UC Irvine 에 왔고 벌써 1주일이 흘렀다. 앞으로도 1주일마다 정리를 하려고 한다. 프로젝트와 관련된 내용만 적는 것은 아니다.

## 6월 26일

![I-SURF 2017 nametag](/assets/images/2017-07-03-uci-i-surf-2017-1st-week/1.jpg)

(이 명찰은 받은 첫 날을 제외하고 사용한 적이 없다)

이 날은 우선 UCI에 Check-In 하는 절차를 진행했다. 이 체크인 절차는 미국 비자를 받아서 오는 모든 사람들이 반드시 진행해야 한다. (입국 후 목적지에 잘 도착했다는 것을 미국 시스템에 등록하는 절차라고 한다)

체크인에 필요한 준비물은 UCI 방문 시에 별도로 안내가 되므로 여기다가는 굳이 적지 않는다.

체크인 절차 후 Meal Card와 셔틀 버스 이용 스티커를 수령했다.

![UCI Meal Card](/assets/images/2017-07-03-uci-i-surf-2017-1st-week/2-2.jpg)

- Meal Card: UCI 내 캠퍼스에 있는 카페나 음식점, Book Store 등에서 이용할 수 있는 카드이다. 이번에 지급된 카드에는 USD 300 이 충전되어 있었다.

![UCI Shuttle Bus Sticker](/assets/images/2017-07-03-uci-i-surf-2017-1st-week/3-2.jpg)

- 셔틀 버스 이용 스티거: 저기에 SS1 2017 SS2 2017 이라고 적혀 있는 것이 셔틀버스 이용 티켓이다. 셔틀버스 탑승 시 이 스티커 부분을 보여주고 탑승을 하면 된다.

이후에 각 학생들과 담당 멘토 간 잠시 이야기를 하는 시간을 가졌다.
나를 담당하는 멘토는 [Bryan Donyanavard (Donny 라고 불러달라고 한다)](https://duttgroup.ics.uci.edu/doku.php/group) 이며 [Nikil Dutt 교수님](http://www.ics.uci.edu/~dutt) 밑에서 연구를 진행하는 분이다.

앞으로 진행할 프로젝트인 Benchmarking a Many-Core Platform 에 대해서 짧게 이야기를 하고 끝났다.

## 6월 27일

이날부터 본격적으로 프로젝트 진행이 시작되었다. 여기 연구실에는 오전 10시부터 오후 6시까지 있으며 상황에 따라 출퇴근 시간은 유동적으로 조절 가능하다.

프로젝트에서 내가 사용할 보드는 NVIDIA Jetson TX2 (Developer Kit)이다. 밑의 사진과 같이 생겼다.

![NVIDIA Jetson TX2 Dev Board](/assets/images/2017-07-03-uci-i-surf-2017-1st-week/4-2.jpg)

근데 이 보드가 이날 아침까지는 없어서 (Donny가 자기 보드가 아니라 다른사람한테 빌려와야 한다고 했었다) 점심 먹을때까지 멍때리고 있었다.

점심을 먹고 온 후 이 보드를 받고 초기 설정을 진행했다. 초기 설정에 대한 글은 이 글을 보면 된다.

초기 설정은 이 날 완료하지 못했다. 연구실 내에서의 Wi-Fi 속도가 너무 느려서 시간 내에 필요한 데이터를 다 다운받을 수 없었고 결국 데이터를 다운로드 받다가 오후 6시가 되어서 퇴근했다.

숙소에서의 Wi-Fi 속도는 꽤 빠른 편이라 여기에서 필요한 데이터의 다운로드를 완료했다.

## 6월 28일

오전에는 숙소에서 다운받았던 데이터를 보드에 설치하는 작업을 진행했다. 진행 과정에 대해서는 이 글(바로 위의 링크와 같음)을 보면 된다. 그리고 점심 시간에 있는 세미나를 들으러 이동했다.

이 날 세미나는 UCI [Brian Demsky 교수님](http://plrg.eecs.uci.edu/)께서 "Mode Checking Concurrent Data Structures for Relaxed Memory Models" 라는 주제로 진행을 하셨다.

지금까지 내가 강연을 들으면서 생각한 방식은 다음과 같았다.

- 어차피 모든 내용을 이해할 수는 없다.
- 그러니까 “아 이런 것들이 있구나” 정도만 이해하자.

이번 세미나도 역시 그런 생각을 가지고 들었다. 그렇게 세미나가 끝나고 바로 연구실로 돌아가서 나머지 프로젝트를 진행하려 했으나…

갑자기 [Said M. Shokair](http://www.urop.uci.edu/otherprojects.html) (이곳 UCI I-SURF 프로그램의 Director, 이하 Said 로 통칭) 가 우리를 혼내기 시작했다. Said 가 우리를 혼내는 이유는 다음과 같았다.

- 왜 아무도 필기를 하지 않는 것이냐
- 왜 세미나 참석 전에 강연자의 profile 을 읽고 오지 않는 것이냐

...살다가 외국인한테 혼나보기는 처음이었다. 뭐 강연 듣기 전에 당연히 해야 하는 것이기도 하겠지만 조금 당황하긴 했다. 그리고 이 세미나를 통해 배운 이 곳 문화 중 하나는 다음과 같다.

- 세미나를 점심을 먹으면서 하는구나…

연구실로 돌아와서 NVIDIA Jetson TX2 보드에서 전압 및 코어 on/off 를 조절하는 방법에 대해서 찾아봤다. 다음 글에 정리해뒀다.

그리고 그 다음에 뭘 해야하는지 멘토에게 물어봤으나 부재중이어서 결국 그냥 퇴근했다.

## 6월 29일 - 6월 30일

(2일을 묶어서 적는 이유는 금요일날 뭘 했는지 기억이 나지 않기 때문이다)

수요일에 했던 것에 이어서 실제로 보드의 전압 및 코어 on/off 를 조절해봤다. 물론 하나 하나 수동으로 하기에는 조금 귀찮아서(…) 내장되어 있는 명령어를 사용했다.

점심 시간에는 잠시 캠퍼스 중앙에 있는 공원 산책을 했다.

![UCI Aldrich Park - 1](/assets/images/2017-07-03-uci-i-surf-2017-1st-week/6-2.jpg)
![UCI Aldrich Park - 2](/assets/images/2017-07-03-uci-i-surf-2017-1st-week/7-1-1.jpg)
![UCI Aldrich Park - 3](/assets/images/2017-07-03-uci-i-surf-2017-1st-week/8-2.jpg)
![UCI Aldrich Park - 4](/assets/images/2017-07-03-uci-i-surf-2017-1st-week/5-2.jpg)

여기는 매일매일 해가 쨍쨍해서 이런 곳에서 산책하기 매우 좋다.

그리고 이제 Power Monitoring 을 어떻게 해야 하는지에 대해서 알아봐야 했다. 근데 이거는 클럭 및 코어 on/off 조절보다 정보를 찾기가 더 힘들었다. 열심히 삽질하다가 퇴근했다(…)

이렇게 첫 주차의 평일이 흘러갔다. 다음 글에서는 첫 주말 후기를 다룰 예정이다.
