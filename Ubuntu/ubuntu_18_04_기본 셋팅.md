## Ubuntu 18.04 LTS 시작

### 1. apt 패키지 업데이트

##### apt-get update

설치 되어있는 패키지들의 새로운 버젼이 있는지 확인할 때 해당 명령어를 사용

##### apt-get upgrade

위에 있는 `apt-get update`를 통해서 확인한 패키지들의 최신 버전에 따라서 패키지들의 버전을 업그레이드 해주는 명령어

-----

### 2. Terminator 설치 (터미널 분할)

1. `sudo apt-get install terminator` 명령어로 설치
2. 터미네이터 창분할 관련 단축키
   * 수평 분할 : Ctrl + Shift + O
   * 수직 분할 :  : Ctrl + Shift + E
   * 다음 창 활성화 : Ctrl + Tab  or  Ctrl + Shift + N
   * 이전 창 활성화 : Ctrl + Shift + Tab  or  Ctrl + Shift + P
   * 현재 활성화 된 창 닫기 : Ctrl + Shift + W
   * 터미네이터 실행 : Ctrl + Alt + T
   * 터미네이터 종료 : Ctrl + Shift + Q
   * 전체화면 : F11

-----

### 3. Chrome 설치

크롬 웹 브라우저 패키지를 설치하기 위해 인증키 등록

* `wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -`

크롬 웹 브라우저 패키지를 다운받을 PPA를 sources.list.d에 추가

* `sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'`

추가한 PPA로부터 설치 가능한 패키지 정보를 가져오기 위해 패키지 리스트를 업데이트

* `sudo apt-get update` 

크롬 웹 브라우저 설치

* `sudo apt-get install google-chrome-stable`

-----

### 4. vim 설치

* `sudo apt-get install vim`

-----

### * 마우스 특수 버튼 활성화

* VMware 가상 OS에서 마우스 측면 앞,뒤로가기 버튼 동작하지 않을 때

* 해당 이미지의 설정파일인 .vmx 파일 수정

  ``` basic
  mouse.vusb.enable = "TRUE"
  mouse.vusb.useBasicMouse = "FALSE"
  usb.generic.allowHID = "TRUE"
  ```

* 위 옵션 추가