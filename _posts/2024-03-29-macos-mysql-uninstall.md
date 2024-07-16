---
title: mysql uninstall, reinstall on macos
date: 2024-03-28 18:15:00 +0900
categories: [MAC, MYSQL, MARIADB]
tags: [mysql, mariadb, mac]  # TAG names should always be lowercase
authors: [gonnichiwa]
---


# MySQL 완전 삭제하고 재설치하기 (MacOS)

- `mysql` | `mariadb` 삭제 후 재설치 함.
- `root` 비번분실 후 재설정 가능토록 가이드 함.

(https://github.com/rangyu/TIL/blob/master/mysql/MySQL-%EC%99%84%EC%A0%84-%EC%82%AD%EC%A0%9C%ED%95%98%EA%B3%A0-%EC%9E%AC%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-(MacOS).md?plain=1)

## MySQL 프로세스 죽이기

(homebrew로 설치했을 경우)

```
brew services stop mysql
```

(launchctl을 등록했다면 내리기)
```
sudo launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```

## 관련 파일 삭제하기

설치 경로 확인하기
```
which mysql
/usr/local/bin/mysql
```

homebrew로 삭제하기

```
brew uninstall --force mysql
```

혹은
```
brew uninstall mysql --ignore-dependencies
brew remove mysql
brew cleanup
```

다음의 라인을 한줄씩 입력해서 삭제한다.

```
sudo rm -rf /usr/local/mysql
sudo rm -rf /usr/local/bin/mysql
sudo rm -rf /usr/local/var/mysql
sudo rm -rf /usr/local/Cellar/mysql
sudo rm -rf /usr/local/mysql*
sudo rm -rf /tmp/mysql.sock.lock
sudo rm -rf /tmp/mysqlx.sock.lock
sudo rm -rf /tmp/mysql.sock
sudo rm -rf /tmp/mysqlx.sock
sudo rm ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/My*
```

완전 삭제한 이후 컴퓨터를 재부팅한다.


## homebrew로 재설치하기

```
brew install mysql
```

mysql 서비스 시작하기

```
brew services start mysql
```

비밀번호 없이 root 로그인
```
mysql -uroot
```

root 비밀번호 설정하기

```
mysql_secure_installation
```

=> VALIDATE PASSWORD PLUGIN 설치여부 물어볼 때는 N(아니오)를 선택할 것.


## mysql root 패스워드 분실 시

- 로컬 머신의 변경 등으로 mariadb 재설치한 뒤 잃어 버리는 경우가 꽤 있다.

### 서비스 종료
```
$ brew services stop mysql
Stopping `mariadb`... (might take a while)
==> Successfully stopped `mariadb` (label: homebrew.mxcl.mariadb)
```

### root 권한 스킵 모드 실행
```
$ mysqld -uroot --skip-grant-tables 
OR
$ mysqld --skip-grant

2024-07-17  0:14:52 0 [Warning] Setting lower_case_table_names=2 because file system for /usr/local/var/mysql/ is case insensitive
mysqld: One can only use the --user switch if running as root
2024-07-17  0:14:52 0 [Note] Starting MariaDB 11.4.2-MariaDB source revision 3fca5ed772fb75e3e57c507edef2985f8eba5b12 as process 1382
...
2024-07-17  0:14:53 0 [Note] Server socket created on IP: '127.0.0.1'.
2024-07-17  0:14:53 0 [Note] mysqld: ready for connections.
Version: '11.4.2-MariaDB'  socket: '/tmp/mysql.sock'  port: 3306  Homebrew
```

### 새창 열어서 mysql실행과 비밀번호 변경

- 새 터미널 열어서 실행함.
```
$ mysql -uroot mysql
OR
$ mariadb -uroot
```

```
---- MariaDB 10.4 미만일 경우

use mysql; 
Flush Privileges; # 앞의 --skip -grant 끄기 위함
UPDATE user SET password=PASSWORD('[변경할 PassWord]') where user='root'; 
Flush Privileges; # 비밀번호 적용 위함
quit(or Ctrl+C)

---- MariaDB 10.4 이상일 경우

use mysql; 
Flush Privileges; # 앞의 --skip -grant 끄기 위함
set password for 'root'@'localhost'=password('[변경할 PassWord]');
Flush Privileges; # 비밀번호 적용 위함
quit(or Ctrl+C)
```

- 아래는 명령과 결과 예시

```
$ mariadb -uroot
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 11.4.2-MariaDB Homebrew

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]> flush privileges;
Query OK, 0 rows affected (0.006 sec)

MariaDB [mysql]> set password for 'root'@'localhost'=password('1234');
Query OK, 0 rows affected (0.021 sec)

MariaDB [mysql]> flush privileges;
Query OK, 0 rows affected (0.000 sec)

MariaDB [mysql]> quit
Bye
```

### 로그인 시험.

```
$ mariadb -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 7
Server version: 11.4.2-MariaDB Homebrew

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> quit
Bye

```

### 이후

- 위 수행단계 중 root 권한 스킵 모드 실행 한 터미널에 `Ctrl+C` 등 걸어도 안꺼질 수 있음.
```
$ mysqld -uroot --skip-grant-tables  
```
- 재부팅해서 완전종료한다.

- 터미널만 종료 후 커맨드 다시 실행 시 실행불가함.
- 새로 설정한 root 계정은 정상 접속 가능.




##### 참고 : https://velog.io/@pindum/MariaDB-root-%ED%8C%A8%EC%8A%A4%EC%9B%8C%EB%93%9C-%EB%B6%84%EC%8B%A4-%EC%8B%9C-%EB%8C%80%EC%B2%98