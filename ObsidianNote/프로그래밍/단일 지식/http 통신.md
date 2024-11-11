
#통신 #connection
###  Get과  Post 통신의 차이
get 통신은 파라미터 내용이 노춤됨
post 는 파리미터 내용이 노출이 안됨 ( 하지만 개발자 도구를 통해 알아낼수있음 따라서 암호화 해야함 )

Get은 idempotent  하고 Post 는 Non-idempotent 하다.
idempotent하다는것은 서버에 여러번 요청 하더라도 동일 한 응답이 나와야 한다는걸 의미합니다.( 즉 조회 등의 행위에만 Get을 사용해야한다는 의미이다.)

Get 통신은 캐시될수있다.
즉 js 나 css같은 파일은 같은 주소에 접속할떄 또 불러야 할필요없이 미리 일부분 저장해놓으면 아낄수있음으로 따로 저장해둔다는 말이다.
이걸로 인해 내용을 변경했는데 적용이 안되는 경우가 있다. 이 떄는 캐시를 지워주면 된다.

쿼리 스트링: url 뒤에 전달하는 파라미터 get통신에 사용됨
ex) ? id=1 & name=value
message body:  start line, headers, body 로 구성되는 post통신에 사용되는 방식
Post는 따라서 url에 표시되지않고 body에 포함되되서 통신한다.