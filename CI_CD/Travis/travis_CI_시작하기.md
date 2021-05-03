## Travis CI 시작하기

### 1. 웹 서비스 설정

1. [https://travis-ci.org/](https://travis-ci.org/) 이동
2. 깃허브 계정으로 로그인
3. Settings에서 깃허브 저장소 활성화

------

### 2. 프로젝트 설정

1. build.gradle과 같은 위치에 travis.yml 파일 생성
2. config 코드 작성

```
language: java
jdk:
  - openjdk11

# Travis CI를 어느 브랜치가 푸시될 때 수행할지 지정
branches:
  only:
    - main

# Travis CI 서버의 Home
# Gradle 을 통해 의존성을 받게 되면 이를 해당 디렉토리에 캐시하여, 같은 의존성은 다음 배포 시 다시 받지 않도록 설정
cache:
  directories:
    - '$HOME/.m2/repository'
    - '$HOME/.gradle'

# main 브런치에 푸시되었을 때 수행하는 명령어
script: "./gradlew clean build"

# CI 실행 완료시 메일로 알람
notifications:
  email:
    recipients:
      - b940608@gmail.com
```

3. main 브런치에 커밋과 푸시를 하고 Travis CI 저장소 페이지 확인
4. yml 파일에 등록한 메일에서 빌드 성공 메일 확인

