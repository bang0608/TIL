### 1. git 다운로드 및 설치

[https://gitforwindows.org/](https://gitforwindows.org/)

-----

### 2. ssh key  생성

1. cmd 또는 git bash 실행
2. ssh-keygen 명령어 입력
3. 완료되면 [RSA 2048] ~~~ 메세지가 출력된다.
4. 해당 경로에 id_rsa(개인키)와 id_rsa.pub(공개키) 생성 확인

-----

### 3. github에 ssh key 등록

[https://github.com/settings/keys](https://github.com/settings/keys)에서 id_rsa.pub 내용 (SSH public key) 등록

---

이후 Repository 생성, clone 등의 과정을 통해 git 사용

나 같은 경우는 git GUI 소스트리(SourceTree)를 사용했다.