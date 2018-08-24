# POJO (Plain Old Java Object)
자바를 계속해서 공부하다보면 당최 알 수 없는 처음보는 어려운 말들이 많이 등장합니다. POJO도 그 중 하나이지요. __POJO는__ Plain Old Java Object의 약자로 __직역하자면 '자바의 오래된 순수 객체' 정도?__ 이름만으로 미루어보면 오래전부터 사용해온 무언가를 지칭하는 말 같은데, 대체 무엇일까요?

POJO는 2000년도에 처음 등장한 용어입니다. POJO를 쉽게 말하자면 일종의 Bean인데, 여기서 말하는 빈은 EJB의 빈을 뜻하는 것이 아닙니다. __대표적으로__ 순수하게 getter, setter 메소드로 이루어진 __값 객체 (Value Object) 가 POJO에 해당__ 합니다. (엔티티, Entity 와는 다른 개념). 흔히 아래 코드와 같은 형태를 가지는 클래스이죠.

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

이클립스나 IntelliJ같은 IDE에서 자동생성해주는 값을 담는 빈 클래스입니다. 이것을 두고 우리는 POJO 라고 부릅니다. 그럼 왜 이를 두고 전부터 사용해오던 '빈(Bean)'이라는 용어 대신 굳이 POJO라고 새로이 명명했을까요?

그 이유는 바로 __빈 이라는 용어로 위의 값 객체를 정의하기에는 Java Bean, EJB의 Bean 등과 구분이 모호하고 또한 Bean이라는 용어로 정의되는 다른 많은 개념들과의 혼동을 피하고 확실히 분리시키기 위해 POJO라고 새로이 지칭한 것__ 이라고 볼 수 있습니다.

## 그럼 클래스나 인터페이스의 상속을 통해 그를 구현한 클래스는 POJO가 될 수 없나요?
__*아닙니다.*__ 상속을 통해 구현한 클래스도 라이브러리나 프레임워크에 종속되어 강제받지 않는 클래스라면 POJO라고 이야기할 수 있습니다. 이의 반례로 대표적으로 자바의 서블릿 (`Servlet`) 클래스가 있습니다. 서블릿 클래스를 작성할 때에는 반드시 `HttpServlet` 을 상속받아야합니다. 쉽게말해 서블릿 클래스를 작성하기 위해서는 `HttpServlet` 을 상속받을지 말지에 대해 선택권이 없으니 종속적이며 강제당하므로 이는 POJO에 해당하지 않습니다. __POJO는 이러한 제약이 없는, 일반적인 객체를 뜻하는 것이니까요.__ 
