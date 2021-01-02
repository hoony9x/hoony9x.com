---
title: AWS S3 와 CloudFront 연동 시 S3 Bucket 으로 307 redirect 되는 문제 해결
author: hoony9x
date: 2019-07-06 23:49:40
image: assets/images/2019-07-06-how-to-fix-aws-s3-and-cloudfront-307-issue/curl-request.png
categories:
  - "Develop"
  - "AWS"
tags:
  - "AWS"
  - "S3"
  - "CloudFront"
---

어느 날 개발을 하던 도중에 S3 bucket 과 CloudFront 를 연동해서 Static Web Hosting 을 하려고 했다.

여기 에 있는 과정을 마친 후에 웹 브라우저로 열어보았으나...

![?!?!](/assets/images/2019-07-06-how-to-fix-aws-s3-and-cloudfront-307-issue/web-browser.png)

정상적이라면 https://hyu-oms.com 으로 떠야 할 도메인이... 
갑자기 https://hyu-oms.com.s3.ap-northeast-2.amazonaws.com/index.html 로 지 혼자 redirect 가 되는 것이었다.

그래서 열심히 구글링을 해 본 결과 [‘Amazon S3에서 HTTP 307 임시 리디렉션 응답을 받는 이유는 무엇입니까?’](https://aws.amazon.com/ko/premiumsupport/knowledge-center/s3-http-307-response/)을 찾을 수 있었다.
간단히 요약하자면…

- S3 Bucket 을 생성한 후에는 이 정보가 다른 region 에 아직 전달이 되지 않았을 수 있으며 최대 24시간이 걸릴 수 있다.
- 24시간 동안 기다리고 싶지 않다면 Origin Domain Name and Path 를 특정 region 으로 수정하면 된다.

말로 하자니 뭔 소린가 싶을 수도 있기 때문에 사진으로 보여주자면

![CloudFront Setup](/assets/images/2019-07-06-how-to-fix-aws-s3-and-cloudfront-307-issue/cloudfront-setup.png)

위 사진에서 보이는 hyu-oms.com.s3.amazonaws.com 을  
hyu-oms.com.s3-ap-northeast-2.amazonaws.com (Seoul Region 일 경우)  
이런 식으로 바꿔주면 된다고 한다.

수정 후의 모습은 다음과 같다.

![수정 후](/assets/images/2019-07-06-how-to-fix-aws-s3-and-cloudfront-307-issue/after-setup-changed.png)

그리고 다시 테스트를 해 봤다.

![수정 결과](/assets/images/2019-07-06-how-to-fix-aws-s3-and-cloudfront-307-issue/fixed.png)

이제 잘 된다.

이 방법은 임시방편이라 생각한다. 급하게 사이트를 띄워야 하는 경우가 아니면 굳이 이렇게 할 필요가 있을까 싶다. 그냥 기다리면 되니까.
