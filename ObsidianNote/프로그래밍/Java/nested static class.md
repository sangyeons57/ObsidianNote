#language/java 

java는 outter class는 static선언이 불가능 하지만 inner class에 static선언이 가능하다.
(outter class가 이미 static이여서 static을 한번 더 붙여서 에러가 난다는 말도 있다.)


inner none static class는 반드시 부모 인스턴스가 있는 경우에만 inner class의 인스턴스를 만들수 있다. 그러므로 반드시 new 로 생성해야만 한다.

inner static class는 독립적으로 생성이 가능하다.


static class에 인스턴스는 모두 동일하지 않다.
static variable이나 method는 동일하지만 class의 instance는 동일한 주소를 가지지 않는다.


왠만하면 무조건 inner class는 static class여야하는데
1. innerclass에 outterclass instance에 대해서 암묵적으로 연결되고 참조된다. 이로 인해 innerclass가 outterclas에 CLASSNAME.this로 접근이 가능하지만 문제가 발생한다.
2. 바로 메모리 누수 문제인데 외부 클레스가 필요없어진 경우 GC 대상으로 매모리에서 제거 해야하는데  innerclass가 참조 하고 있어서 
3. 메모리 누수 가 발생하게 된다.

그래서 static inner class를 하면 outter class를 참조 못하는대신 이러한 메모리 문제가 발생하지 않는다.