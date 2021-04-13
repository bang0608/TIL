## AWS EC2 시작

### 1. EC2 인스턴스 생성

1. `region`을 서울로 변경 후 인스턴스 생성
2. AMI - Ubuntu Server 18.04 LTS 선택
3. 인스턴스 유형 t2.micro (프리 티어 사용 가능) 선택
4. VPC, subnet 외 설정 기본 (차 후 변경)
5. 스토리지 용량 30GiB 설정
6. 보안그룹 인바운드 규칙 일부 설정 (80, 8080, 443 포트)
7. pem key 생성 (백업 필수!)

-----

### 2. Elastic IP(EIP) 고정 아이피

* 인스턴스의 public IP는 고정된 IP가 아니라 유동적인 IP 주소
* EC2 인스턴스를 재시작하면 IP 주소가 바뀜
* 탄력적 IP (Elastic IP)를 할당하면 인스턴스를 재시작 하여도 IP는 고정
* EIP는 인스턴스를 종료해도 요금이 발생
* 결론 : 일단 보류

-----

### 3. SSH 클라이언트를 사용하여 Linux 인스턴스에 연결

* [참고] [Linux 인스턴스용 사용 설명서](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html)

1. VMware Ubuntu pem key 복사

2. shell script 파일 생성

   ```
   #!/bin/bash
   
   sudo ssh -i ~/[pem file name].pem ubuntu@ec2-xxx-xxx-xxx-xxx.ap-northeast-2.compute.amazonaws.com
   ```

3. sh 파일 권한 수정

   `chmod +x [file name].sh`

4. sh 실행

​	