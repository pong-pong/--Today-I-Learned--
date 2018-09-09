## 자바의 synchronized 키워드 (동기화)
Java를 사용해 프로그래밍을 하다보면 멀티 스레딩 (multi-threading) 으로 인하여 동기화를 시켜야 하는 경우가 꽤 있습니다. 스레드 ( `Thread` ) 는 각 클래스의 자원에 접근할 수 있는데 (멤버변수가 Heap을 이용하기 때문), 여러 스레드가 공유자원에 접근하는 경우에는 동기화를 해주어야 나중에 값을 읽고 쓸 때 문제가 생기는 것을 방지할 수 있습니다. 이러한 상황에서 바로 `synchronized` 키워드를 사용하게 됩니다. 간단하게 이야기하면 `synchronized` 키워드를 사용함으로써 메소드나 객체에 대한 동시접근을 차단해 연산을 보호한다고 이야기할 수 있습니다. 예를 들어 은행에서 출금할 때를 생각하면 이해가 쉬울 것 같네요.

### synchronized 키워드의 위치
``` Java
// 함수에 사용
public synchronized void printValue(Object obj) {
    System.out.println(obj.toString());
}

// 객체의 변수에 사용 (block 사용)
private Object obj = new Object();
public void lockObject(Object obj) {
    synchronized(obj) {
        // ... do something
    }
}
```

위의 코드에서 나타나있듯이 `synchronized` 키워드는 메소드에 lock 을 걸거나 특정 객체에 lock 을 걸 때 두 가지 형태로 사용이 가능합니다. 

`synchronized` 메소드의 경우 한 스레드가 `synchronized` 메소드를 호출해서 수행하고 있을 때, 이 메소드가 종료될 때까지 다른 스레드가 이 메소드를 호출해서 수행할 수 없게 되고, `synchronized` 블록도 마찬가지로 블록의 시작부터 lock 을 걸어주고 이후에 블록 안의 모든 연산의 수행이 종료되면 lock 을 풀어 다른 스레드가 블록 수행중 지정 객체에 접근할 수 없도록 합니다. 강력한 기능을 키워드 하나로 이용할 수 있다는 장점이 있지만, 남용하면 성능상에 문제를 줄 수 있기 때문에 신중히 사용해야합니다.