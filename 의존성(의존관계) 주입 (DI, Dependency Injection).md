# 의존성(의존관계) 주입 (DI, Dependency Injection)
의존성, 의존관계 주입은 우리에게 DI라는 용어로 더 많이 알려져 있습니다. __의존관계__ 라고 하니 뭔가 대단해보이지만, 쉽게 말하자면 __객체가 객체를 사용한다는 말과 같습니다.__ 따라서 __의존관계 주입이란 클라이언트 객체가 의존하고 있는 서비스 객체를 클라이언트 객체 내부에서 생성해서 사용하는 것이 아닌 외부, 즉 제 3자가 서비스 객체를 생성해 주입시켜서 사용하도록 하는 것__ 이라고 이야기할 수 있습니다. 여기서 서비스 객체는 사용당하는 객체, 클라이언트 객체는 서비스 객체를 사용하는 주체가 되는 객체를 뜻합니다. 용어도 그렇고, 한 줄만으로 끝내기에는 복잡하고 심오해보이니 간단한 예제코드를 통해서 살펴보겠습니다.

``` Java
public class Elevator {

    private Voice voice;

    public Elevator(Voice voice) {
        this.voice = voice;
    }

    public void pushOpenBtn() {
        this.voice.say("The door is opening");
    }

    public void pushCloseBtn() {
        this.voice.say("The door is closing");
    }

}
```

위의 코드는 엘리베이터의 열림과 닫힘 버튼을 눌렀을 때 음성 안내메세지가 출력되는 메소드를 가진 엘리베이터 클래스입니다. 생성자로 `Voice` 객체를 받아서 객체 내의 `say()` 함수를 사용합니다. 다음 코드는 `Voice` 인터페이스를 상속받아 `say()` 메소드를 구현한 `WomanVoice` 와 `ManVoice` 클래스입니다.

``` Java
public interface Voice {
    
    public void say(String sentence);
    
}

public class WomanVoice implements Voice {

    @Override
    public void say(String sentence) {
        ...
    }

}

public class ManVoice implements Voice {

    @Override
    public void say(String sentence) {
        ...
    }

}
```

위에서 제가 말한 바에 따르면 여기서 `Elevator` 는 `Voice` 가 제공하는 서비스를 사용하는 클라이언트, `Voice` 는 `Elevator` 에게 서비스를 제공하는 서버(웹 서버의 서버가 *아닌* 서비스를 제공하는 주체를 뜻하는 말) 역할을 합니다. 현재 `Elevator` 클래스는 `Voice` 객체를 클래스 내부에서 __직접 생성해서 사용하지 않고, '생성자'를 통해 누군가에 의해 '주입'받아 사용__ 하고 있습니다. 이게 바로 DI, 의존관계 주입입니다. 엄청 어려운 줄 알았는데, 생각보다 쉽죠? DI는 생성자 뿐 아니라 우리가 흔히 사용하는 `setter()`, 즉 __수정자를 통해서도 주입이 가능합니다.__ 생성 후 나중에 주입을 할 수 도 있다는 것이지요. 

위 코드의 경우 뿐만아니라 DI는 단순히 객체를 외부에서 생성해서 주입받아 사용하는 모든 경우를 이야기 할 수 있습니다. 값 객체도 생성자와 수정자를 통해 객체를 주입받아 사용하니, 아래 코드 또한 DI의 예제가 될 수 있겠군요.

``` Java
public class Book {
    private String title;
    private String author;
    private String publisher;
    private int cost;
    
    public Book(String title, String author, String publisher, int cost) {
        this.title = title;
        this.author = author;
        this.publisher = publisher;
        this.cost = cost;
    }
    
    public String getTitle() {
        return this.title;
    }
    
    public void setTitle(String title) {
        this.title = title;
    }
    
    // 이하생략
    ...
}
```

## 정리
DI의 개념 자체는 생각보다 대단하고 어렵지 않습니다. 그러나 DI를 바탕으로 하는, 여러가지 디자인 패턴과 같은 친구들이 많다보니 그 과정에서 다른 개념도 섞이고 또 섞여서 생각보다 더 어렵고 멀게만 느껴지는 것 같아요. 그러니 쉽게, 정확하게 기억하도록 합시다. __DI는 클라이언트 내부에서 서버를 생성하는 대신 외부에서 생성된 서버를 주입받아서 사용하는 것__ 입니다. 이번 내용을 정확히 이해했다면 앞으로 DI를 활용한 여러가지 친구들을 마주쳤을 때에도 당황하지 않고 공부해나갈 수 있을것이라 생각합니다. 

오늘 예제는 전략 패턴 (Strategy Pattern) 을 알고있다면 더 알차게 학습할 수 있습니다. `Voice` 라는 알고리즘 패밀리를 두고, 각각 `WomanVoice` 와 `ManVoice` 라는 알고리즘을 두어 `Elevator` 클래스가 알고리즘(전략)을 선택적으로 사용할 수 있게 한 경우라는것, 한 눈에 알아볼 수 있겠지요? 