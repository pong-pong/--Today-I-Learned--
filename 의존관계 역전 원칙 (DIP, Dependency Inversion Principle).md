## 의존관계 역전 원칙 (DIP, Dependency Inversion Principle)
__의존관계 역전 원칙은 '고수준 모듈은 저수준 모듈의 구현에 의존해서는 안되며, 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다'는 프로그래밍 원칙__ 입니다. 여기서 이야기하는 고수준 모듈은 의미있는 기능을 제공하는 모듈을 말하며 저수준 모듈은 고수준 모듈을 구현하기 위해 필요한 하위 기능의 실제 구현으로, 쉽게 말하자면 메소드 자체와 메소드의 구현정도로 이해하면 되겠습니다. 한 문장으로 설명하기에는 잘 이해가 가지 않을 수 있으니 아래 예제를 통해 살펴보겠습니다. 오늘 포스팅을 읽기 전 __의존성 주입 (DI, Dependency Injection)__ 에 대해서 한 번씩 읽어보고 오시면 두 개념을 골고루 이해하는데에 큰 도움이 됩니다.

``` Java
public interface Filter<T> {
    
    public List<T> filter(List<T> list);
    
}

public class Storage {
    
    private Filter filter = new AppleFilter();
    
    ...
    
    public List<Apple> filter(List<Apple> apples) {
        return filter.filter(apples);
    }
    
}

public class AppleFilter implements Filter<Apple> {
    
    @Override
    public List<Apple> filter(List<Apple> apple) {
        ...
        return result;
    }
    
}
```

위의 코드는 `Filter` 라는 인터페이스를 상속받아 `AppleFilter` 라는 클래스를 만들고 `Storage` 클래스에서 `AppleFilter` 를 사용하고있는 예제입니다. 동시에 DIP 원칙을 어기고 있는 코드입니다. 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안된다고 이야기했는데, 현재 코드를 살펴보면 `Storage`의 `filter` 메소드에서 `AppleFilter`의 `filter` 메소드의 구현에 직접적으로 의존하고 있다는것을 확인할 수 있습니다. 

만약 `Storage` 에서 `Apple` 이 아니라 `Orange` 를 필터링해야한다면 새로운 메소드를 만들거나 `OrangeFilter` 를 구현해 기존의 필터 메소드의 `AppleFilter` 를 갈아끼워 메소드를 수정해서 사용하면 됩니다. 이렇게 저수준 모듈에 따라서 고수준 모듈이 의존하고 있는 오브젝트가 달라지기때문에 DIP를 어기고있는 경우가 되는 것입니다.

``` Java
public class Storage {
    
    private Filter filter;

    ...
    
    public Storage(Filter filter) {
        this.filter = filter;
    }
    
    public setFilter(Filter filter( {
        this.filter = filter;
    }
    
    public <T> List<T> filter(List<T> list) {
        return filter.filter(list);
    }
    
}
```

위의 코드는 `Storage` 클래스를 수정한 코드입니다. `Storage` 클래스에서 직접 필터 구현 클래스를 생성하는게 아니라 어떤 필터 클래스를 사용하는지 모르도록 생성자, 또는 수정자를 통해 객체를 외부에서 주입받아 사용함(DI)으로써 어떤 리스트를 무슨 필터를 가지고 어떻게 필터링을 하는지 그 구현에 대해 `Storage` 클래스는 신경쓸 필요가 전혀 없게 되었습니다. 

즉, 고수준 모듈( `Filter` )이 저수준 모듈의 구현( `AppleFilter` 의 `filter` )에 의존하지 않고, 저수준 모듈( `AppleFilter`) 이 고수준 모듈( `Filter` 인터페이스)에서 정의한 추상 타입에 의존하고 있다는 말과 같습니다. 이는 첫 번째 예제 코드에서 `Storage` (고수준 모듈) 가 `Filter` 를 상속받은 클래스(저수준 모듈)에 의존했던 것과 다르게 상황이 역전되었다고 볼 수 있겠습니다.

## 정리
예제를 통해 살펴보았던 것과 같이 고수준 모듈이 저수준 모듈에 의존했던 상황이 역전되어서 저수준 모듈이 고수준 모듈에 의존하게 된다는 것을 보고 의존관계 역전 원칙이라고 부릅니다.  DIP는 나중에 개방 폐쇄 원칙과 리스코프 치환 원칙을 행하게 해주는 기반이 됩니다. 또한 DIP와 DI의 개념은 다른것이므로 혼동하지 말아야하며, DI는 DIP를 구현하는 방법 중 하나일 뿐이라는것을 기억하시길 바랍니다.