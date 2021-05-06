# MariaDB 셋팅

여지껏 이런 기본적인 셋팅에 대한 정리를 잘 안했었던거 같다.

### 1. 설치
- Ubuntu Server 20.x
```
sudo apt-get install mariadb-server
```
### 2. Root 접속

sudo 권한을 얻은 후 
```mariadb
mysql -u root
```
입력하면 바로 접속이 된다.
```mariadb
use mysql;
UPDATE user SET password = password('비밀번호') WHERE user = 'root';
```
root의 비밀번호를 변경해준다.

### 3. 외부 접속 설정
```
vim /etc/mysql/mariadb.conf.d/50-server.cnf
```
그 다음 bind-address를 주석처리
```
#bind-address       = 127.0.0.1
```
root의 외부 접속 허용
```mariadb
use mysql;
grant all privileges on *.* to 'root'@'%' identified by 'root password';
```
Maria DB 서비스 재시작
```
service mariadb restart
```

ps. root의 외부 접속 허용은 보안상 위험하므로 해당 내용에 대한 찾아보는게 필요할 듯.
