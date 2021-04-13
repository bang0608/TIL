## IntelliJ 사용 팁

### 로그 한글 깨짐

* 기본 설정 파일의 http,logback encoding을 UTF-8로 변경해도 로그에서 한글이 깨짐

* 해결방법

1. [ Ctrl+Shift+A ] Edit Custom VM Options... 클릭
2. vmoptions 파일이 열리면, 아래 내용을 파일에 추가
3. `-Dfile.encoding=UTF-8`
4. IntelliJ 재부팅하면 한글이 정상적으로 나온다.

