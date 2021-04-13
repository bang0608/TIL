## Ubuntu Java & MariaDB 설치

## Java 설치

### 1. 구 버전 삭제 (설치되어 있을 경우)

* openjdk-java 시리즈

  `# sudo apt-get remove openjdk*`

* oracle-java 시리즈

  `# sudo apt-get remove oracle*`

* 완료 후 아래 명령어도 실행

  ```
  # sudo apt-get autoremove --purge
  $ sudo apt-get autoclean
  ```

-----

### 2. openjdk 설치

```
# sudo add-apt-repository ppa:openjdk-r/ppa
# sudo apt install openjdk-11-jdk
```

성공적으로 설치가 된다면 아래와 같이 출력됨을 확인할 수 있다.

```
# java -version
openjdk version "11.0.10" 2021-01-19
OpenJDK Runtime Environment (build 11.0.10+9-Ubuntu-0ubuntu1.18.04)
OpenJDK 64-Bit Server VM (build 11.0.10+9-Ubuntu-0ubuntu1.18.04, mixed mode, sharing)
```



#### * 주의

```
# sudo apt install openjdk-11-jdk
# java -version
openjdk version "10.0.2" 2018-07-17
OpenJDK Runtime Environment (build 10.0.2+13-Ubuntu-1ubuntu0.18.04.4)
OpenJDK 64-Bit Server VM (build 10.0.2+13-Ubuntu-1ubuntu0.18.04.4, mixed mode)
```

`# sudo apt install openjdk-11-jdk ` 이렇게 설치하고 나서 자바 버전을 확인하면 10.0.2로 나온다.

\[관련 자료] [https://askubuntu.com/questions/1037646/why-is-openjdk-10-packaged-as-openjdk-11](https://askubuntu.com/questions/1037646/why-is-openjdk-10-packaged-as-openjdk-11)

-----

## MariaDB

### 1. 설치

1. `# sudo apt-get install mariadb-server` 명령어로 설치

2. 설치 후 서비스 동작 확인 `# sudo systemctl status mariadb`

   ```
   ● mariadb.service - MariaDB 10.1.47 database server
      Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; vendor preset: enabled)
      Active: active (running) since Tue 2021-04-13 16:54:17 UTC; 1min 3s ago
        Docs: man:mysqld(8)
              https://mariadb.com/kb/en/library/systemd/
    Main PID: 29059 (mysqld)
      Status: "Taking your SQL requests now..."
       Tasks: 27 (limit: 1140)
      CGroup: /system.slice/mariadb.service
              └─29059 /usr/sbin/mysqld          
   ```

3. 다음을 사용하여 버전 확인 가능

   ```
   # mysql -V
   mysql Ver 15.1 Distrib 10.1.47-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
   ```

4. `# sudo mysql_secure_installation` 명령을 실행하여 MariaDB 설치 보안을 향상

-----

### * Access Denied 에러

* 일부 리눅스 시스템에서 mysql을 설치하고 `# mysql -u root -p` 으로 로그인 시도를하면 ,

  'ERROR 1698 (28000): Access denied for user 'root'@'localhost'이라는 에러를 발생할때가 있다.

* 이는 기본적으로 초기설정되어있는 mysql의 root 계정의 패스워드 타입때문인데 이 타입을 변경해주면된다.

```
# sudo mysql -u root (sudo를 사용하여 root계정으로 mysql에 접속)

mysql> USE mysql;
mysql> SELECT User, Host, plugin FROM mysql.user;

+------+-----------+-------------+
| User | Host      | plugin      |
+------+-----------+-------------+
| root | localhost | unix_socket |
+------+-----------+-------------+
```

* 위처럼 root의 plugin이 unix_socket으로 되어 있는 것을 확인할 수 있다.

* 이 값을 mysql_native_password로 변경해주면 일반적인 로그인이 가능하다.

```
mysql> update user set plugin='mysql_native_password' where user='root';
mysql> flush privileges;
mysql> select user, host, plugin from user;

+------+-----------+-----------------------+
| user | host      | plugin                |
+------+-----------+-----------------------+
| root | localhost | mysql_native_password |
+------+-----------+-----------------------+

mysql> exit;
Bye
```

* 다시 root로 접속해보면 정상적으로 동작 할 것이다.