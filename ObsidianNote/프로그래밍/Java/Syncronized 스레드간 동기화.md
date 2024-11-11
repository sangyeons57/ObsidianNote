2가지 방법이 있다.

블럭수준 동기화
```JAVA
public class Counter {
    private int count = 0;
    private final Object lock = new Object();

    public void increment() {
        synchronized (lock) { // 특정 객체에 대해 동기화
            count++;
        }
    }

    public int getCount() {
        synchronized (lock) { // 특정 객체에 대해 동기화
            return count;
        }
    }
}

```

메서드 수준 동기화
```JAVA
public class Counter {
    private int count = 0;

    // synchronized 메서드
    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}

```

동기화를 할 경우

이 메서드의 사용이 앞 스레드가 끝날떄 까지 기다리게 된다.


---

자바의 모든 객체는 monitor를 가지고 있다.
monitor는  여러 스레드가 객체로 동시에 접근하는것을 막는 역할을 한다.
heap영역에 있는 객체는 모든 스레드에서 공유 가능하다. [[힙과 스택]]

따라서 스레드가 monitor를 가지면 monitor를 가지는 객체에게 lock을 걸수 있다.

메서드수준 동기화는 인스턴스가 moniter를 가져서 동기화시킨다.


동기화 되지 않은 메서드는 모니터를 사용하지 않는다. 즉 동기화 된 메서드만 객체의 모니터를 확인해서 코드를실행시킨다.
따라서 이 동기화를 해주기위해서 블럭단위 동기화는 객체를 사용하고(모든 객체에는 하나의 모니터가 있기 때문에)
메서드 단위는 현제 인스턴스 객체를 모니터로 사용하게 된다.

이떄 스택은 각자 블럭 네애서 그리고 스레드 내에서 각각 존제해서 문제가 발생하지않고
힙은 공동으로 사용하기 때문에 문제가 발생할수있어서 (이 부분때문에 동기화 작업을 한다.)





동기화에는 요런 문제도있다고 한다.
**데드락(Deadlock)**: 잘못된 동기화는 데드락을 유발할 수 있습니다. 이는 두 스레드가 서로 상대방이 소유한 잠금을 기다리며 영원히 멈추는 상태입니다.

이것들을 이용해서 여러스레드를 함꼐 다룰수도있다.
**wait(), notify(), notifyAll()**: 동기화된 블록 안에서 스레드 간의 협업을 가능하게 하는 메서드들입니다. `wait()`는 스레드를 일시 정지시키고, `notify()`는 대기 중인 스레드를 깨우며, `notifyAll()`은 모든 대기 중인 스레드를 깨웁니다.

