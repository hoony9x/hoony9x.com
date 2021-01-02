---
title: Elastic Beanstalk 로 생성된 S3 Bucket 삭제가 되지 않을 경우 해결 방법
author: hoony9x
date: 2018-11-22 23:18:18
image: assets/images/2018-11-22-delete-aws-s3-bucket-created-by-elasticbeanstalk/elasticbeanstalk.png
categories:
  - "Develop"
  - "AWS"
tags:
  - "AWS"
  - "Elastic Beanstalk"
---

언제부터인가 내 S3 Bucket 목록에는 elasticbeanstalk-ap-northeast-2-764930618406 라는 이름의 bucket이 있었다.

이름을 보아하니 예전에 elastic beanstalk 로 뭔가를 테스트하다가 말았는데 그때 생성되고 남아있던 모양이다.

아마 이 bucket 을 지우려고 하면 Access Denied 라고 하면서 안지워질 것이다.

```bash
$ aws s3 rb s3://elasticbeanstalk-ap-northeast-2-764930618406
remove_bucket failed: s3://elasticbeanstalk-ap-northeast-2-764930618406 An error occurred (AccessDenied) when calling the DeleteBucket operation: Access Denied
```

그래서 해결 방법을 찾아봤다. 바로 [이 글](https://forums.aws.amazon.com/thread.jspa?threadID=145366)에서 확인할 수 있었다.

1. 해당하는 bucket 의 bucket policy 를 확인한다.
2. 웹 콘솔에서는 이 순서(bucket -> properties -> permissions -> edit bucket policy)를 통해 확인 가능하다.
3. 아마 DeleteBucket 을 막는 내용이 적혀있을 텐데, 귀찮은 관계로 걍 policy 내용을 싸그리 지워버린다.
4. 그 다음에 bucket 을 지우려고 하면 지워질 것이다.

이 글을 보는 분들이 문제가 잘 해결되었기를 바랍니다.
