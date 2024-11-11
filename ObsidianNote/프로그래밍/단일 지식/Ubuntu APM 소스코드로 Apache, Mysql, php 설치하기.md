#ubuntu
[1번째 Apache 설치](https://rotoma-code.tistory.com/entry/Apache-1-APM-%EC%86%8C%EC%8A%A4%EC%88%98%EB%8F%99-%EC%BB%B4%ED%8C%8C%EC%9D%BC-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-1)
[2번째 mysql 설치](https://rotoma-code.tistory.com/entry/PHP-2-APM-%EC%86%8C%EC%8A%A4%EC%88%98%EB%8F%99-%EC%BB%B4%ED%8C%8C%EC%9D%BC-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-3)
[3번째 PHP 설치](https://rotoma-code.tistory.com/entry/PHP-2-APM-%EC%86%8C%EC%8A%A4%EC%88%98%EB%8F%99-%EC%BB%B4%ED%8C%8C%EC%9D%BC-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-3)

mysql 작동안할
[여기](https://king-ja.tistory.com/95) 로 해보니 됨 안되면 또 다른거 찾아야
## Apache
[설치 wget 위치  https://archive.apache.org/dist/httpd/](https://archive.apache.org/dist/httpd/)
apache 시작방법
```ubutnu
sudo apache2.4/bin/httpd -k start
```

```ubuntu
ps aux | grep httpd
```
ps명령어로 demon을 확인할수있다.
## MySQL


mysql 소스코드로 설치하는 경우 환경변수 설정이 안되서 직접 해줘야한다.

세션이 유지되는동안 1회성
```ubuntu
export PATH=$PATH:/usr/local/mysql/bin
```
영구적인 설정
```ubuntu
echo 'export PATH=$PATH:/usr/local/mysql/bin' >> ~/.bashrc
source ~/.bashrc
```

mysql 데이터 디렉토리 초기화명령어
```ubuntu
sudo mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
```
mysql 서버 시작
```
sudo mysqld_safe --user=mysql &
```
이걸 입력시 비밀번호 설정
MySQL 클라이언트 실행방법에 대해서 작성해야함

환경변수 설정한 경우
MySQL 서비스시작
```ubuntu
sudo systemctl start mysql
```
MySQL 서비스 상태 확인
```
sudo systemctl status mysql
```

환경변수 설정이 안될경우
/usr/local/mysql/bin/mysqld 처럼  mysqld 명령을 대신할수있다.



초기화
```ubuntu
sudo /usr/local/mysql/bin/mysqld --initialize --user=mysql --datadir=/var/lib/mysql/data
```
설저잉 안되서 sudo mysqld 대신 쓰는거고 datadir 는 mysql  서버 저장소이다.
또한 이것을 실행시킬떄 비밀번호가 나오는데 __반드시알고있어야한다.__ 

다음에 mysql  클라이언트에서
```mysql
ALTER USER 'root'@'localhost'IDENTIFIED BY 'root-password';
```
페스워드를 바꿀수있다.

mysql 클라이언트 실행
```
mysql -u root -p
```
-p는 페스워드를 입력한다는 의미인데 -p이후에 페스워드를 입력하지 않을경우
cli가 페스워드를 질문하게된다.
```ubuntu
/usr/local/mysql> bin/mysql -u root -p
```



sql 명령어는 대소문자를 구분하지않는다.