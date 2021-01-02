---
title: Ghost에서 AWS S3를 이미지 저장소로 사용하기 - 2편
author: hoony9x
date: 2018-11-24 23:35:01
image: assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.14.47.png
categories:
  - "Develop"
  - "AWS"
tags:
  - "AWS"
  - "S3"
  - "Ghost"
---

[1편 내용](/use-aws-s3-as-image-storage-for-ghost-part-1)에 이어서 계속 진행되는 이야기이다.

이 글에서는 다음 내용을 다룬다.

## 2편 진행 순서

1. IAM 유저 생성 및 권한 설정
2. [ghost-storage-adapter-s3](https://www.npmjs.com/package/ghost-storage-adapter-s3) 설치
3. 최종 설정

### AWS IAM 유저 생성 & 권한 설정

이걸 하는 이유는, 리눅스에서 root 계정을 직접 사용하지 않는 이유와 같다.  
1편에서 생성한 bucket 에만 접근 가능한 계정을 따로 만들어서 사용하려는 것이다.

- IAM Management Console 이동

![IAM Setup - 1](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.17.35.png)

- 이런 화면이 뜨면 그냥 닫아주면 된다.  
(여기서 Get Started with IAM Users 로 가도 상관은 없는데, 분명히 Don’t show me this message again 을 눌러서 저 창이 뜨지 않는 사람도 있을 것이라 생각한다)

![IAM Setup - 2](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.21.14.png)

- 왼쪽은 이렇게 되어 있다. 일단 Policies 로 이동한다.
- 이동한 후에 Create Policy 버튼을 누른다.

![IAM Setup - 3](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.23.11.png)

- JSON 탭을 누르면 다음과 같이 데이터를 직접 입력할 수 있게 된다. 밑의 내용을 그대로 사용하면 된다.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::<your_bucket_name>"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:PutObjectVersionAcl",
        "s3:DeleteObject",
        "s3:PutObjectAcl"
      ],
      "Resource": [
        "arn:aws:s3:::<your_bucket_name>/*"
      ]
    }
  ]
}
```

- `<your_bucket_name>`에는 1편에서 생성했던 bucket 의 이름을 넣어주면 된다. (나의 경우는 cdn.khhan1993.com 이다.)
- 하단에 있는 Review policy 를 눌러 다음으로 진행한다.

![IAM Setup - 4](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.26.32.png)

- Name 부분은 알아서 적으면 된다.
- 다른 부분은 건드릴 필요가 딱히 없다. 다 했으면 Create policy 버튼을 누르면 된다.
- 그 다음에는 Users 탭으로 이동한다. 아마도 Add user 버튼이 있을 것인데 눌러주자.

![IAM Setup - 5](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.30.26.png)

- User name 은 알아서 지정하면 된다.
- Access type 은 Programmatic access 만 체크하도록 한다.
- 다 했으면 다음으로 넘어간다.

![IAM Setup - 6](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.32.33-1.png)

- Permission 을 지정하는 단계이다. 세 번째에 있는 Attach existing policies directly 를 선택한다.
- 하단에 있는 검색창에서 방금 전에 생성했던 policy 이름을 입력하면 검색 결과에 나오게 될 것이다. 해당 policy 를 선택하면 된다.
- 다 했으면 Next 눌러서 넘어가도록 하자.
- 다음 단계는 선택사항이다. 따로 지정할 내용이 없다면 넘어가도 좋다.

![IAM Setup - 7](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.36.20-1.png)

- Create user 를 눌러 생성을 하도록 하자.

![IAM Setup - 8](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.37.40-1.png)

- IAM user 생성이 완료되면 다음과 같이 Access key ID 와 Secret access key 를 확인할 수 있다.
- 이 값들은 이 화면을 벗어나게 되면 다시 확인할 수 없으니 이 창을 그대로 열어둔 채 밑의 단계를 진행하면 된다.

### ghost-storage-adapter-s3 설치

연동을 위한 adapter 는 [ghost-storage-adapter-s3](https://www.npmjs.com/package/ghost-storage-adapter-s3) 를 사용하게 될 것이다.

![Ghost 설치된 경로 ls 결과](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.46.45.png)

ghost 가 설치된 경로로 가면 다음과 같이 되어 있을 것이다. ([ghost-cli](https://github.com/TryGhost/Ghost-CLI) 를 이용하여 production 으로 설치했다고 가정한다)

해당 경로에서 밑의 내용을 그대로 따라한다.  
(내용 출처 : https://www.npmjs.com/package/ghost-storage-adapter-s3)

```bash
$ npm install ghost-storage-adapter-s3
$ mkdir -p ./content/adapters/storage
$ cp -r ./node_modules/ghost-storage-adapter-s3 ./content/adapters/storage/s3
```

혹시나 권한 문제가 발생한다면 sudo 를 붙여서 한다.

### 최종 설정

![Ghost 설치된 경로 ls 결과](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------2.46.45.png)

ghost 가 설치된 경로에 보면 config.production.json 이란 파일이 있다.

```json
{
  "url": "YOUR_BLOG_URL",
  "server": {
    "port": 2368,
    "host": "127.0.0.1"
  },
  "database": {
    "client": "mysql",
    "connection": {
      "host": "localhost",
      "user": "SECRET",
      "password": "SECRET",
      "database": "SECRET"
    }
  },
  "mail": {
    "transport": "Direct"
  },
  "logging": {
    "transports": [
      "file",
      "stdout"
    ]
  },
  "process": "systemd",
  "paths": {
    "contentPath": "YOUR_GHOST_INSTALLED_PATH/content"
  },
  "bootstrap-socket": {
    "port": 8000,
    "host": "localhost"
  }
}
```

ghost-cli 를 이용하여 설치한 후 따로 설정을 바꾼 것이 없다면 파일의 내용은 위와 같을 것이다.

이 파일에 다음 내용을 추가하면 된다.

```json
{
  "...": "...",
  "storage": {
    "active": "s3",
    "s3": {
      "accessKeyId": "IAM_USER_ACCESS_KEY",
      "secretAccessKey": "IAM_USER_SECRET_KEY",
      "region": "YOUR_S3_BUCKET_REGION",
      "bucket": "YOUR_S3_BUCKET_NAME",
      "assetHost": "YOUR_CLOUDFRONT_URL",
      "forcePathStyle": true
    }
  }
}
```

- accessKeyId: 위에서 생성한 IAM user 의 access key 입력.
- secretAccessKey: 위에서 생성한 IAM user 의 secret key 입력.
- region: s3 bucket 을 생성한 region 을 입력.  
(예를 들어 Asia Tokyo 일 경우 ap-northeast-1 이다.)
- bucket: 1편에서 생성했던 s3 bucket 이름을 입력.
- assetHost: 이 부분은 1편에서 CloudFront Distribution 생성 시 custom domain 을 사용했는지 여부에 따라 입력할 값이 달라지게 된다.
- custom domain 사용 시: `https://` 형태로 입력
- custom domain 미사용 시: 밑의 그림 참고

![CloudFront domain name 확인](/assets/images/2018-11-24-use-aws-s3-as-image-storage-for-ghost-part-2/-----------2018-11-24-------3.04.38.png)

- custom domain 미사용 시 위 그림에 있는 Domain Name 부분에 해당하는 값을 적으면 된다.  
(https://XXXXXX.cloudfront.net 형식)

여기까지 다 했으면 이제 변경사항 저장 후 다음을 입력한다.

```bash
$ ghost restart
```

자 이제부터 새로 업로드가 되는 이미지들은 전부 AWS S3 Bucket 에 담기게 되며 이들은 CloudFront 를 통해 블로그에서 보여지게 된다.

이상 “Ghost 에서 AWS S3 를 이미지 저장소로 사용하기” 끝.
