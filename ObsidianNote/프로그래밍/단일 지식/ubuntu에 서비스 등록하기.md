우분투에서는 쉽게 서비스를 등록해서 실행시킬수있따.

jar 파일 예
bash

해당 위치에 서비스 파일을 생성한다
```bash
sudo vi /etc/systemd/system/myapp.service
```

서비스 파일 작성
```text
[Unit]
Description=Java Application Service
After=syslog.target network.target

[Service] 
Type=simple
SuccessExitStatus=143
Restart=on-failure
RestartSec=10s
User=ubuntu
Group=ubuntu
WorkingDirectory=/home/ubuntu/
Environment=JAVA_HOME=/path/to/your/java/home
ExecStart=/path/to/java -jar /path/to/your/app.jar
ExecStop=/bin/kill -15 $MAINPID

[Install]
WantedBy=multi-user.target
```
예가 있긴한데  이건 검색해서 찾아보고 작성하면 좋다.

필수적인거
ExecStart=/path/to/java -jar /path/to/your/app.jar
이게 내가 실행 할필요없이 서비스에서 자동으로 실행시켲는 명령을 작성하는 부분인데
그냥 실행할떄 쓰는 명령어를 쓰면 된다.
보통
ExecStaart=/usr/bin/{프로그램이름}  -속성 \[파일 이름]
이런식으로 작성한다.

deamon파일을 reload한다 아마 시스탬 서비스 파일 을 재설정한다는 말이다
```bash
sudo systemctl daemon-reload
```

system에 실행할수있게 킴
```bash
sudo systemctl enable myapp.service
```

이 이후로는 systemctl 명령어 사용하면 된다.
```bash
sudo systemctl start myapp.service
sudo systemctl status myapp.service
```