# AWS CloudFormation + Serverless deploy

- [AWS CloudFormation + Serverless deploy](#aws-cloudformation--serverless-deploy)
  - [AWS CloudFormation](#aws-cloudformation)
  - [Serverless](#serverless)
  - [CloudFront Origin Access Identity(OAI)](#cloudfront-origin-access-identityoai)
  - [Project setup (ex. Reactjs)](#project-setup-ex-reactjs)
  - [Local deploy](#local-deploy)
    - [aws credential](#aws-credential)
    - [AWS CloudFormation References](#aws-cloudformation-references)
    - [Purge CDN](#purge-cdn)

## AWS CloudFormation

- CloudFormation은 Amazon Web Services(AWS) 리소스를 자동으로 생성해 주는 서비스이다. 사용하려는 AWS 리소스를 템플릿 파일로 작성하면, CloudFormation이 이를 분석해서 AWS 리소스를 생성한다. 이렇게 생성된 리소스를 스택이라고 한다.
- CloudFormation은 템플릿 작성, 템플릿 업로드, 스택 생성, 스택 설정 및 리소스 생성의 4단계로 실행된다.
- 템플릿은 [CloudFormation 디자이너](https://console.aws.amazon.com/cloudformation/designer)에서 작성할 수 있다.
- [reference](https://medium.com/pplink/aws-cloudformation%EC%9C%BC%EB%A1%9C-%EC%9D%B8%ED%94%84%EB%9D%BC-%EC%9E%90%EB%8F%99%ED%99%94-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-9fe13cdf08c9)

## Serverless

- 서버를 직접 관리할 필요가 없는 아키텍처
- AWS CloudFormation + Serverless 조합은 IaaS(Infrastructure as a Service). 인프라 가상화
  - 시스템에서 필요한 모든 인프라 자원(네트워크, 스토리지, 서버)까지 가상화한 방식
- references
  - https://jaehoney.tistory.com/77
  - https://pearlluck.tistory.com/98

## CloudFront Origin Access Identity(OAI)

- S3 + CloudFront로 배포했을 때 CloudFront 주소로만 접근할 수 있게 설정하는 것
- [reference](https://darrengwon.tistory.com/1345)

## Project setup (ex. Reactjs)

```
npx create-react-app react-aws-cloudformation --template typescript
```

```
npm i -D serverless serverless-s3-sync serverless-stack-termination-protection
```

```
npm run build
```

## Local deploy

```
npm i -g serverless
```

```
sls --version
```

```
serverless config credentials --provider aws --key 'aws_access_key_id' --secret 'aws_secret_access_key'
```

### aws credential

- vi ~/.aws/credential
- 여러 계정을 쓰는 경우 `[default]` 에 이름 지정해서 사용
- example

  ```
  [default]
  aws_access_key_id=ASLKDJKLSAJDLA
  aws_secret_access_key=ZXCOIXJCO/asdaweqw121

  [tag1]
  aws_access_key_id=ASLKDJKLSAJDLA
  aws_secret_access_key=ZXCOIXJCO/asdaweqw121

  [tag2]
  aws_access_key_id=QOKWPQWPOQK
  aws_secret_access_key=GJPSOJDPWEQ/eroeiroeir1
  ...
  ```

- deploy command examples
  ```
  (default)sls deploy
  ```
  ```
  AWS_PROFILE=tag1 sls deploy
  ```

### AWS CloudFormation References

- Templates
  - s3 bucket: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-s3.html#scenario-s3-bucket-website-customdomain
  - cloudfront: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-cloudfront.html
- Resources and Properties
  - s3 bucket: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-s3-bucket.html#cfn-s3-bucket-bucketname
  - cloudfront: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_CloudFront.html
  - s3 bucket policy: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-policy.html
- Resource Attribute: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-product-attribute-reference.html

- Serverless Framework References

  - serverless-s3-sync(plugin for s3 bucket upload): https://www.serverless.com/plugins/serverless-s3-sync
  - serverless-stack-termination-protection: https://www.npmjs.com/package/serverless-stack-termination-protection

- React + Serverless deploy
  - https://ichi.pro/ko/awsui-s3-mich-cloudfronte-seobeoliseu-react-aeb-saengseong-mich-baepo-87428259930096

### Purge CDN

- CDN 을 사용한다면, 만약에 프로젝트에 업데이트가 발생했을 때 기존에 CDN 에 퍼져있는 파일들을 새로고침 해주어야 한다. 해당 작업을 처리하기 위해선 Purge, 혹은 Invaldiation 이라는 작업을 해주어야 한다. CDN 에 퍼져있는 파일을 제거처리함으로서 새로 받아오게끔 하는 방식.
- https://react-etc.vlpt.us/08.deploy-s3.html
