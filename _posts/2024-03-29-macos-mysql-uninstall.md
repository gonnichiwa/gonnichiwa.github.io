---
title: gradlew build spring boot proj on mac
date: 2024-03-28 18:15:00 +0900
categories: [MAC, MYSQL, MARIADB]
tags: [mysql, mariadb, mac]  # TAG names should always be lowercase
authors: [gonnichiwa]
---
# MySQL 완전 삭제하고 재설치하기 (MacOS)
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