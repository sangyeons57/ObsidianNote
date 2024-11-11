
우선 가상기기 Linux에 특정 포트를 열어놓는 작업
```ubuntu
sudo apt update
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```



#### ufw 명령어
ufw (Uncomplicated Firewall) 방화벽 관리도구 이다.
```ubuntu
방화벽 활성화 
방화벽 비활성화
sudo ufw eneable
sudo ufw disalbe

방화벽 상태
sudo ufw status
sudo ufw status verbose
```
##### 규칙설정
```ubuntu
들어오는 연결 모두 허용
sudo ufw default deny incoming
모두 거부
sudo ufw default allow outgoing
```
```ubuntu
#포트허용
sudo ufw allow [포트번호]

#SSH 포트 허용
sudo ufw allow 22

# HTTP 포트 허용
sudo ufw allow  80

#포트 거부
sudo ufw deny 23
```

특정 ip주소 허용
```ubuntu
sudo ufw allow from [ip 주소]

sudo ufw allow from [ipwnth] to any port [포트번호]
```

규칙 삭제
```ubuntu
sudo ufw delete allow [포트번호]
```


가상기기가 네트워크 설정이  NAT 인경우 안될수있다.
Brigde Adapter로 바꿔서 할수있다.
또한 이럴경우 ip주소가 바뀌고 약간에 시간이 걸린다.
