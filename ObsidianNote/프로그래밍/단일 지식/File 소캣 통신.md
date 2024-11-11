#socket #통신 #connection 

이번에 파일 데이터를 보넬때 필요한 요소들을
- 파일 형식
- 파일 크기
- 파일 데이터
등이있다.

일반적으로 파일크기를  8바이트(long) 으로 전송하고 해당 데이터를 읽은후에
파일데이터를 읽기 시작한다.

파일 데이터는 일반적으로 버퍼의 크기를 정해서 파일 크기를 전부 읽을 떄 까지
반복해서 버퍼의 크기 만큼 읽는다.

```Java
try (FileOutputStream fos = new FileOutputStream(file);  
     FileChannel fileChannel = fos.getChannel();){  
    int bytesRead;  
    long totalBytesRead = 0;  
    while (totalBytesRead < buffer.expectedLength) {  
        buffer.messageBuffer.clear();  
        bytesRead = clientChannel.read(buffer.messageBuffer);  
        if(bytesRead == -1) {  
            System.out.println("connection closed prematurely");  
            break;        }  
        buffer.messageBuffer.flip();  
        while (buffer.messageBuffer.hasRemaining()){  
            fileChannel.write(buffer.messageBuffer);  
        }  
        totalBytesRead += bytesRead;  
    }  
}
```
이런식으로  데이터를 전체 길이를 읽을때까지
주어지 버퍼를 clear하고 read한 다음에
해당 데이터를 flip해서 쓰기 모드로 바꾸고
file에 작성한후에
데이터를 다받아왔는지 확인하고 반복하는 작업을 한다.

따라서 파일 소켓 통신에서 가장 주요한점은
파일 읽는 버퍼를 만들고
파일을 다 읽을떄 까지
해당 버퍼를 사용해서 파일을 읽고 쓰는거다.


