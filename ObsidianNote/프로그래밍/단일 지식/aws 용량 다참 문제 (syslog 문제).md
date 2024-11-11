서버에 계속 로그를 작성하다보면 제한 용량이 넘어가는 경우가있다.

이경우에 winscp(ssh 통신)가 전송이 안된느 문제가 발생할수있다.

이떄 system로그를 비울필요가있다.

이런식으로 파일 내용만 비울수있다.
```
sudo sh -c 'cat /dev/null > /var/log/syslog'
```

파일 용량보는법은
```
sudo du -hs *
sudo du -h --max-depth=1
```

이런식으로 할수있다.
