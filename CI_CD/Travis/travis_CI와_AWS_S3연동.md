## Travis CI 와 AWS S3 연동

### 1. AWS Key 발급

* 일반적으로 AWS 서비스에 외부 서비스가 접근할 수 없다.
* 접근 가능한 권한을 가진 Key를 생성해서 사용해야 한다.
* AWS에서는 이러한 인증 관련된 기능을 제공하는 서비스로 IAM(Identity and Access management)가 있다.

1. AWS IAM 사용자 추가
2. 액세스 유형 : 프로그래밍 방식 액세스 선택
3. 권한 설정 방식 : 기존 정책 직접 연결 선택
   * AmazonS3FullAccess와 AWSCodeDeployFullAccess 체크

4. 액세스 키와 비밀 멕세스 키 확인

---

### 2. Travies CI에 키 등록

1. [https://travis-ci.org/](https://travis-ci.org/) 이동
2. 프로젝트 설정 하단에 Environment Variables 항목에 AWS IAM 키 등록
   * AWS_ACCESS_KEY : 액세스 키 ID
   * AWS_SECRET_KEY : 비밀 엑세스 키

---

### 3. AWS S3 버킷 생성

1. [https://s3.console.aws.amazon.com/s3](https://s3.console.aws.amazon.com/s3) 에 접속하여 S3 버킷 생성
2. 생성 시 퍼블릭 액세스 차단 설정에서 모든 퍼블릭 엑세스 차단 체크

---

### 4. 프로젝트 Travis 설정하기

