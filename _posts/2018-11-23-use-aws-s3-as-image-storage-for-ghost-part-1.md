---
title: Ghost에서 AWS S3를 이미지 저장소로 사용하기 - 1편
author: hoony9x
date: 2018-11-23 23:23:35
image: assets/images/2018-11-23-use-aws-s3-as-image-storage-for-ghost-part-1/-----------2018-11-23------9.55.10.png
categories:
  - "Develop"
  - "AWS"
tags:
  - "AWS"
  - "S3"
  - "Ghost"
---

1달 전부터 Ghost 를 개인 블로그 플랫폼으로 사용하기 시작했다. (이전 플랫폼은 Wordpress 였다.)

이미지는 용량을 많이 먹는다. 이는 서버의 저장 공간을 많이 차지할 뿐만 아니라 트래픽 증가에도 기여한다.

그런데, [이 글](https://docs.ghost.org/concepts/storage-adapters/)에서 이 이미지들을 서버의 로컬 저장소가 아니라 외부 저장소(Custom Storage 라고 표현하고 있다)를 이용할 수 있다는 것을 알게 되었다.

나는 AWS S3 를 이용하기로 했다.

## 1편 진행 순서

1. S3 Bucket 생성
2. 생성한 Bucket 을 Public Read 가 가능하도록 설정
3. CloudFront 설정
4. (CloudFront 에서 Custom Domain 사용 지정 시) Route 53 설정

### 우선 S3 Bucket 부터 만들자

- AWS Management Console 로 이동
- S3 Console 이동
- Create Bucket 클릭

![S3 Bucket Create - 1](/assets/images/2018-11-23-use-aws-s3-as-image-storage-for-ghost-part-1/-----------2018-11-23------7.26.21.png)

- Bucket 이름 지정 (나의 경우는 cdn.khhan1993.com 으로 지정했다.)
- Configure options 는 나의 경우는 따로 수정한 것이 없다.

![S3 Bucket Create - 2](/assets/images/2018-11-23-use-aws-s3-as-image-storage-for-ghost-part-1/-----------2018-11-23------7.28.56.png)

- Set permissions 에서 “Manage public bucket policies for this bucket” 확인 후 체크박스를 둘 다 해제한다.
- 이렇게 하지 않으면 이후에 Public Policy 지정을 할 때 Access Denied 가 발생하게 된다.
- 다 했으면 Create Bucket 을 진행한다.

### Bucket 에 Public Read 권한 부여하기

- 생성된 Bucket 을 클릭 후 Permissions 탭으로 이동한다.

![S3 Bucket Permission Setup](/assets/images/2018-11-23-use-aws-s3-as-image-storage-for-ghost-part-1/-----------2018-11-23------8.44.35.png)

- Bucket Policy 클릭 후 밑의 내용을 그대로 붙여넣는다.  
(해당 bucket에 대해서 모든 사용자(익명 포함)에게 읽기 권한을 부여하게 된다.)

```json
{
  "Version": "2012-10-17",
  "Id": "Policy1542891309409",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::<your_bucket_name>/*"
    }
  ]
}
```

- `<your_bucket_name>` 에는 자신이 지정한 bucket 이름을 적으면 된다.
- Save 를 누른다.

여기까지 하면 S3 Console 에서 해야 할 설정은 끝난다.

### CloudFront 설정하기

- CloudFront Console 이동

![CloudFront Create - 1](/assets/images/2018-11-23-use-aws-s3-as-image-storage-for-ghost-part-1/-----------2018-11-23------9.31.08.png)

- 첫 번째에 있는 Web 을 클릭

![CloudFront Create - 2](/assets/images/2018-11-23-use-aws-s3-as-image-storage-for-ghost-part-1/-----------2018-11-23------9.31.59.png)

- Origin Domain Name 을 클릭하면 방금 전에 생성한 Bucket 이 보일 것이다. 이것을 선택하도록 하자.

![CloudFront Create - 3](/assets/images/2018-11-23-use-aws-s3-as-image-storage-for-ghost-part-1/-----------2018-11-23------9.33.53.png)

- Default Cache Behavior Settings 에서 Viewer Protocol Policy 는 두 번째 항목을 선택하도록 한다.  
(이렇게 하는 이유는, 만약 https 가 적용된 사이트에서 http 로 resource 요청을 할 경우 브라우저 자체에서 이를 차단해버리는 경우가 발생한다. 이를 미연에 방지하기 위해 https 로 redirect 를 하도록 하려 한다.)

![CloudFront Create - 4](/assets/images/2018-11-23-use-aws-s3-as-image-storage-for-ghost-part-1/-----------2018-11-23------9.36.06.png)

- Compress Objects Automatically 는 Yes 를 선택하도록 한다.

![CloudFront Create - 5](/assets/images/2018-11-23-use-aws-s3-as-image-storage-for-ghost-part-1/-----------2018-11-23------9.37.27.png)

- Distribution Settings 에서 만약 자신의 Custom Domain 을 사용하고 싶다면 Alternate Domain Names 부분에 사용할 도메인 이름을 적으면 된다.
(나의 경우 cdn.khhan1993.com 을 적었다)
- Custom Domain 사용 시 Custom SSL Certificate 를 지정해야 한다. Certificate 는 이미 가지고 있는 것을 사용할 수도 있고, Amazon 에서 발급하는 인증서를 사용할 수도 있다.
- 나머지는 그대로 놔두고 맨 밑에 Create Distribution 을 누르면 된다.

### Route53 설정하기

이 내용은 자신이 위의 CloudFront 설정 중 Custom Domain 을 사용하기로 했을 경우에만 따라하면 된다. 이 외에는 그냥 넘어가도 된다.

- Route53 Console 로 이동

![Route53 Setup](/assets/images/2018-11-23-use-aws-s3-as-image-storage-for-ghost-part-1/-----------2018-11-23------9.47.54.png)

- 도메인 네임 지정
- Type 은 A Record
- Alias 에 Yes 를 클릭 후, Target 에서 방금 전에 생성한 CloudFront Distribution 을 선택한다.
- Create 버튼을 누른다.

여기까지 했다면 절반 정도 끝낸 것이다. 나머지 내용은 [2편](/use-aws-s3-as-image-storage-for-ghost-part-2)에서 계속...
