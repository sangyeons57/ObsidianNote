#Java/Android #Programing/static 

Android에서 밖으로 나갔다가 들와서 onCreate가 실행되는경우
OnCreate가 실행되서 프로세스(프로그램) 이 세롭게 실행되었다고 착각 할수있지만

OnCreate는 프로세스가 실행되는것이 아니라 Activity가 실행되는 거다.

즉 static은 프로그램이 종료되기전까지 계속 값을 가지고 남아있다.

이로인해 싱글톤 같은 방식 을 활용하여 프로그래밍 할떄
Activity값은 Static으로 가지고 있으면 동일한 Activity를 참조하지 않아서 문제가 생길수있다.