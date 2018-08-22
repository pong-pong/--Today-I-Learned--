# 단일 책임 원칙 (SRP, Single Responsibility Principle)
단일 책임 원칙은 '모든 클래스(모듈, 함수 등 포함)는 하나의 책임만 가지며, 클래스는 그 책임을 완전히 캡슐화 해야한다'는 프로그래밍 원칙입니다. 동일한 이유로 변경되는 것들을 함께 모아두고, 서로 다른 이유로 변경되는 것들은 분리시킨다는 말입니다. 만약 클래스가 하나의 책임이 아닌 두 개 이상의 책임을 가지게 된다면, 해당 클래스를 변경할 때 변경하려는 이유의 가짓수도 그만큼 늘어나게 된다는 말인데, 이렇게 되면 이후 상대적으로 유지보수가 힘들어질 수 있습니다. 쉽게 생각하자면 외부 요인에 의해 내부 로직이 변경된다면 해당 클래스는 SRP를 위반한 경우라고 이야기할 수 있습니다. 

``` JAVA
public class BookDAO() {
    
    private DataSource dataSource;
    
    public BookDAO() {
        this.dataSource = new SimpleDriverDataSource();
        this.dataSource.setDriverClass(com.mysql.jdbc.Driver.class);
        this.dataSource.setUrl("jdbc:mysql://localhost/test");
        this.dataSource.setUsername("root");
        this.dataSource.setPassword("0000");
    }
    
    public add(Book book) throws SQLException {
        Connection c = this.dataSource.getConnection);
        ...
        c.close();
    }
    
    public delete(String code) throws SQLException {
        Connection c = this.dataSource.getConnection);
        ...
        c.close();
    }
    
}
```

위의 코드는 `DataSource` 를 생성하고, 커넥션을 이용해 DB에 쿼리를 날리는 메소드가 집합되어있는 클래스입니다. 이 경우 하나의 클래스에 `DataSource` 를 생성하는 책임 하나와 커넥션을 이용해 DB에 쿼리를 날리는 책임 하나씩 총 두개가 뭉쳐져있습니다. 이 경우 생성 과정에서 변동이 일어나거나 쿼리를 날리는 과정에서 변동이 일어나거나 하는 두 가지 경우에 클래스에 변동이 생길 수 있으므로 SRP를 위반한 사례에 해당합니다. `BookDAO` 클래스가 SRP를 따르게 하려면 의존성 주입 (DI, Dependency Injection)을 이용해서 외부에서 주입시켜 사용하면 됩니다.

``` JAVA
public class BookDAO() {
    
    private DataSource dataSource;
    
    public BookDAO(DataSource dataSource) {
        this.dataSource = dataSource;
    }
    
    public add(Book book) throws SQLException {
        Connection c = this.dataSource.getConnection);
        ...
        c.close();
    }
    
    public delete(String code) throws SQLException {
        Connection c = this.dataSource.getConnection);
        ...
        c.close();
    }
  
}
```

이런식으로 생성자를 이용해 생성 로직을 외부로 분리시키면 BookDAO 클래스는 커넥션을 이용해 DB에 쿼리를 날리는 책임 하나만을 가진 클래스가 되므로 변경의 이유도 하나만 가지게 됩니다. 

## 정리
__단일 책임 원칙은 이렇게 각각의 클래스나 모듈, 함수에 책임을 하나만 갖게 함으로써 응집도를 높이고 결합도를 낮추어 상대적으로 유지보수가 용이한 코드를 작성하는데에 도움을 줍니다.__ 오늘 예제 코드는 __의존성(의존관계) 주입 (DI, Dependency Injection)과 전략 패턴 (Strategy Pattern)에 기반한 코드로써 같이 보면 패턴을 이해하고 사용하는데에 도움을 줄 수 있으니 함께 읽어보는것을 강력히 추천드립니다.__