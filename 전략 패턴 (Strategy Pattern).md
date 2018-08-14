# 전략 패턴 (Strategy Pattern)
__전략 패턴은 디자인 패턴의 꽃__ 이라고 불릴 만큼 다양하게, 자주 사용되는 패턴입니다. [개방 폐쇄 원칙](https://github.com/pong-pong/--Today-I-Learned--/blob/master/%EA%B0%9C%EB%B0%A9%20%ED%8F%90%EC%87%84%20%EC%9B%90%EC%B9%99.md)에도 가장 잘 들어맞는 패턴이라고 볼 수 있습니다. 

전략 패턴은 자신의 기능 맥락(context)에서 __필요에 따라 변경이 필요한 알고리즘(전략)을 인터페이스를 통해 외부로 독립시키고, 이를 구현한 구체적인 알고리즘(전략) 클래스를 필요에 따라 바꾸어 사용할 수 있게 하는 디자인 패턴__ 입니다. 다시 말해 각 알고리즘을 캡슐화 하는 알고리즘 패밀리, 즉 인터페이스 정의해둔 다음, 런타임 해당 알고리즘을 구현한 구체적인 구현 객체를 선택하는 기법이라고 말할 수 있습니다.

## 예시

``` Java
@FunctionalInterface
public class BookPredicate {
    boolean test(Book book);
}

public class BookHeavyWeightPredicate implements BookPredicate {
    @Override
    boolean test(Book book) {
        return book.getWeight() > 750;
    }
}

public class BookLongTitleLengthPredicate implements BookPredicate {
    @Override
    boolean test(Book book {
        return book.getTitle().length() > 20;
    }
}
```

위의 `BookPredicate` 인터페이스는 자바에서 기본적으로 제공하는 `java.util.function.Predicate<T>` 인터페이스와 똑같이 동작하는 인터페이스입니다. 대신 대상 객체가 `Book`으 로 한정되어있습니다. 그리고 그 아래에 `BookPredicate` 를 상속받아 구현한 두 개의 구현 객체가 있습니다. `BookHeavyWeightPredicate` 는 무게가 무거운 책이 파라미터로 주어질 때 `true` 를 반환하고, `BookLongTitleLengthPredicate` 는 제목의 길이가 긴 책이 파라미터로 주어질 때 `true` 를 반환합니다. 

``` Java
public List<Book> filterBooks(List<Book> books, BookPredicate p) {
    List<Book> result = new ArrayList<>();
    for(Book b : books)
        if(p.test(b))
            result.add(b);
    return result;
}
```

위의 코드는 책 리스트와 `BookPredicate` 객체를 파라미터로 받아 조건에 따라 필터링 한 결과를 반환합니다. 만약 무게가 무거운 책을 필터링하고싶다면 `BookHeavyWeightPredicate` 객체를 파라미터로 제공하면 되고 제목의 길이가 긴 책을 필터링하고싶다면 `BookLongTitleLengthPredicate` 객체를 파라미터로 제공하면 됩니다. 

이 외의 다른 조건으로 필터링하고싶다면 예시와 같이 `BookPredicate` 를 __상속받아 직접 구현__ 하거나, __익명클래스 또는 람다__ 를 이용해서 필터링시킬 수 있습니다(예시와 같이 *필요한 조건이 생길 때마다 클래스를 구현하는것은 좋은 코드가 아닙니다!* 자바 8에서 새로 추가된 람다를 이용해보세요!).

이때 `BookPredicate` 인터페이스가 전략 패턴의 알고리즘 패밀리(인터페이스), `BookPredicate` 를 상속받아 구체적으로 구현한 `BookHeavyWeightPredicate` 와 `BookLongTitleLengthPredicate` 가 알고리즘(전략)에 해당합니다.

