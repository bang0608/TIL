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

1. travis.yml 파일에 코드 추가

```
before_deploy:
  - zip -r springboot-webservice *
  - mkdir -p deploy
  - mv springboot-webservice.zip deploy/springboot-webservice.zip

deploy:
  - provider: s3
    access_key_id: $AWS_ACCESS_KEY # Travis repository settings에 설정된 값
    secret_access_key: $AWS_SECRET_KEY # Travis repository settings에 설정된 값
    bucket: banghyunwoo # S3 버킷
    region: ap-northeast-2
    skip_cleanup: true
    acl: private # zip 파을 접근을 private로
    local_dir: deploy # before_deploy에서 생성한 디렉토리
    wait-until-deployed: true
```

* 빌드 후 `Skipping a deployment with the s3 provider because this branch is not permitted: main` 메세지와 함께 s3 버킷에 .zip 파일 업로드가 되지 않았다.
* deploy 시 권한 문제로 업로드가 안되는 현상

```
wait-until-deployed: true
    on:
      branch: main
```

* 아래에 on: branch: main 추가 작성하여 권한 부여 후 정상 동작 확인

---

### 5. Travis CI와 AWS S3, CodeDeploy 연동

1. EC2가 CodeDeploy를 연동 받을 수 있게 IAM 역할 생성
   * IAM 역할 만들기 : 역할 서비스 EC2 선택
   * 권한 정책 연결 : AmazonEC2RoleforAWSCodeDeploy 체크

2. EC2 인스턴스 IAM 역할 변경
   * 인스턴스 마우스 우클릭 - 보안 - IAM 역할 연결 수정 선택
   * AWS 역할 선택 후 `재부팅` (재부팅 해야만 역할이 정상적으로 적용됨)

3. CodeDeploy 에이전트 설치

   * EC2 접속 후 다음 명령어 입력
   * `aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/install . --region ap-northeast-2`
   * 내려받기가 성공하면 아래 메세지 콘솔에 출력됨.
   * `download: s3://aws-codedeploy-ap-northeast-2/latest/install to ./install`

   * install 파일에 실행 권한 부여 : `chmod +x ./install`
   * 설치 진행 : `sudo ./install auto`
     * `/usr/bin/env: ‘ruby’: No such file or directory`라는 메세지와 함께 설치가 안될 경우
     * `sudo apt-get install ruby` 명령어로 ruby 설치 후 진행

   * `sudo service codedeploy-agent status` 명령어로 Agent 상태 확인

---

### 6. CodeDeploy를 위한 권한 생성

1.  AWS IAM  역할 생성 : AWS 서비스 유형 선택 - CodeDeploy 선택
2. 역할 이름 및 태그 작성 후 권한 생성 완료

---

### 7. CodeDeploy 생성

1. AWS CodeDeploy 서비스 이동 - **애플리케이션 생성 버튼** 클릭
2. 애플리케이션 이름 작성, 컴퓨팅 플랫폼 : **EC2/온프레미스** 선택
3. 생성이 완료되면 배포 그룹을 생성하라는 메시지 나타남 : **배포 그룹 생성** 버튼 클릭
4. 이름 입력, 서비스 역할은 **6번에서 생성한 codedeploy 권한** 선택, 배포 유형 **현재 위치** 선택
5. 환경 구성 : **Amazon EC2 인스턴스** 선택
6. 배포 설정 : **CodeDeployDefault.AllAtOnce** 선택 (기본값)
7. 로드 밸런싱 활성화 체크 해제 후 배포 그룹 생성 완료

---

### 8. Travis CI, S3, CodeDeploy .연동

1. S3에서 넘겨줄 zip 파일 저장할 디렉토리 생성
   * `mkdir ~/dev/zip`

2. travis.yml 파일에 codedeploy 내용 추가

```
  - provider: codedeploy
    access_key_id: $AWS_ACCESS_KEY
    secret_access_key: $AWS_SECRET_KEY
    bucket: banghyunwoo
    key: springboot-webservice.zip # 빌드 파일을 압축해서 전달
    bundle_type: zip
    application: springboot-webservice # AWS 웹 콘솔에서 등록한 CodeDeploy 어플리케이션
    deployment_group: springboot-webservide-group # 웹 콘솔에서 등록한 CodeDeploy 배포 그룹
    region: ap-northeast-2
    wait-until-deployed: true
    on:
      branch: main
```

3. appspec.yml 파일 추가 (travis.yml 파일과 같은 위치)

```
version: 0.0 # CodeDeploy 버전, Project 버전과는 별개
os: linux
files:
  - source:  / # CodeDeploy 에서 전달해 준 파일 중 destination 으로 이동시킬 대상 ( 루트 경로 지정 시 전체 파일 )
    destination: /home/ubuntu/dev/zip/
    overwrite: yes
```

4. 깃허브 커밋 & 푸시, Travis CI 완료 후 CodeDeploy 화면서 배포 내역 확인 가능
5. EC2에 정상적으로 프로젝트 파일 확인