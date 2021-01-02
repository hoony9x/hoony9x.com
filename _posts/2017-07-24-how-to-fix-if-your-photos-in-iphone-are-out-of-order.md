---
title: iPhone 사진 뒤죽박죽 되었을 때 해결방법
author: hoony9x
date: 2017-07-24 21:07:28
updated: 2019-07-30 16:54:23
image: assets/images/2017-07-24-how-to-fix-if-your-photos-in-iphone-are-out-of-order/IMG_0229.png
categories:
  - "Tech"
tags:
  - "Apple"
  - "iPhone"
featured: true
---

오늘 아침에 iPhone을 오랜만에 DFU 초기화를 하고 사진을 iCloud 로부터 다시 내려받았다. 그런데 초기화 전까지 찍은 날짜 순으로 잘 되어있던 사진들이 갑자기 뒤죽박죽 섞여 있었다. 이런 젠장… 그래서 이번 글은 iPhone 사진 뒤죽박죽 해결방법이다.

대략 구글을 뒤져본 결과 비슷한 문제를 겪은 사람들이 몇몇 있지만 흔한 사례는 아닌 것 같다.

- [애플 커뮤니티 글 1 (영어)](https://discussions.apple.com/thread/7101409?tstart=0)
- [애플 커뮤니티 글 2 (영어)](https://discussions.apple.com/docs/DOC-9942)

대략 살펴보니 iOS 는 기본적으로 파일의 생성일을 기준으로 정렬 이 된다고 한다.  
(이는 사진을 찍은 날짜와 다르다)

그래서 나는 다음과 같이 해결했다.

1. 우선 iPhone 에 있는 사진을 전부 컴퓨터로 옮긴다. iCloud 에 백업된 사진이 있다면 그것까지 전부 옮긴다.
2. iPhone 에 있는 사진을 전부 삭제. (iCloud 에 백업된 것까지 전부 삭제)
3. [다음 글](다음 글)을 참고하여 파일의 생성일을 사진을 찍은 날짜로 변경한다.  
(나는 [ExifTool](https://en.wikipedia.org/wiki/ExifTool) 을 사용했다)
4. 업데이트된 사진들을 다시 iPhone 에 집어넣는다.
5. ???
6. PROFIT!

끝. 질문이 있다면 댓글로…

> ### 2019년 7월 30일 추가
> iOS 13 부터는 사진의 정렬 방식이 변경되었습니다.
> 이 버전부터 파일의 생성일이 아니라 (EXIF 정보 상) 사진의 촬영일을 기준으로 정렬됩니다.
