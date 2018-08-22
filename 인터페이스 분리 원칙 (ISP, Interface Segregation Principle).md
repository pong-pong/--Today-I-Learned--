# 인터페이스 분리 원칙 (ISP, Interface Segregation Principle)
__인터페이스 분리 원칙은 '클라이언트(객체를 사용하는 소비자. e.g 메서드, 클래스 등)가 자신이 이용하지 않는 메서드에 의존, 즉 존재하지 않아야한다' 는 프로그래밍 원칙__ 입니다. 기능이 많은 큰 덩어리의 인터페이스를 구현하는 대신 이를 구체적이로 작은 단위들로 분리시켜 사용함으로써 클라이언트들이 꼭 필요한 메서드들만 이용할 수 있게 하는게 좋습니다. ISP를 잘 지킨다면 시스템의 내부 의존성을 약화시켜(낮은 결합도) __손쉬운 리팩토링, 수정 및 재배포, 높은 재사용성을 갖는 코드를 작성할 수 있습니다.__ 

``` Java
public interface IPhone {
    
    public void touchDisplay();
    
    public void pressHomeKey();
    
    public void threeDemensionalTouch();
    
}
```

위의 코드는 아이폰의 기능을 추상화시킨 인터페이스입니다. 화면을 터치하고 홈 키를 누르고, 3D터치를 실행시키는 기능을 묶어 이를 통해 아래 코드처럼 아이폰 7, 아이폰 8 등 많은 아이폰을 찍어낼 수 있습니다.
 
``` Java
public class IPhone7 implements IPhone {
  
    @Override
    public void touchDisplay() {
        System.out.println("iphone 7 display touched");
    }
    
    @Override
    public void pressHomeKey() {
        System.out.println("iphone 7 home key pressed");
    }
    
    @Override
    public void threeDemensionalTouch() {
        System.out.println("iphone 7 3d touch activated");        
    }
    
}
```

그럼 이번에는 최근에 나온 아이폰X와 구형 모델인 6도 한 번 구현해 볼까요?

``` Java
public class IPhoneX implements IPhone {
  
    @Override
    public void touchDisplay() {
        System.out.println("iphone X display touched");
    }
    
    @Override
    public void pressHomeKey() {
        // 
    }
    
    @Override
    public void threeDemensionalTouch() {
        System.out.println("iphone X 3d touch activated");        
    }
    
}

public class IPhone6 implements IPhone {
  
    @Override
    public void touchDisplay() {
        System.out.println("iphone 6 display touched");
    }
    
    @Override
    public void pressHomeKey() {
        System.out.println("iphone 6 home key pressed");
    }
    
    @Override
    public void threeDemensionalTouch() {
        //       
    }
    
}
```

아이폰X는 기존 모델과 달리 홈 키가 없습니다. 3D터치는 6s모델 이후부터 사용할 수 있는 기능이구요. 이렇게 되면 아이폰X와 아이폰6s는 쓰지도 않는 홈 키와 3D터치 메소드를 가지고 있어야합니다. 달리말하면 자신이 이용하지 않는 메서드에 의존하게 된다는 것이지요. 이 경우 ISP를 위반한 경우라고 이야기할 수 있는데요, 이를 해결하려면 그냥 각각의 기능을 독립적인 인터페이스로 만들어 상속받아 구현하면 간단하게 해결됩니다. 

``` Java
public interface IPhone {
    public void touchDisplay();
}

public interface HomeKey {
    public void pressHomeKey();
}

public interface ThreeDemensionalTouch {
    public void threeDemensionalTouch();
}

public class IPhoneX implements IPhone, ThreeDemensionalTouch {
    
    @Override
    public void touchDisplay() {
        System.out.println("iphone X display touched");
    }
    
    @Override
    public void threeDemensionalTouch() {
        System.out.println("iphone X 3d touch activated");
    }
    
}

public class IPhone6 implements IPhone, HomeKey {
    
    @Override
    pubic void touchDisplay() {
        System.out.println("iphone 6 display touched");
    }
    
    @Override
    public void pressHomeKey() {
        System.out.println("iphone 6 home key pressed");
    }
    
}
```

이런식으로 홈 키와 3D터치를 각각의 독립적인 인터페이스로 만들어 필요한 경우에 선택적으로 상속받아 구현할 수 있도록 하는 것입니다. 이렇게 되면 이용할 필요가 없는 메서드에 의존하지 않게 되므로 내부 의존성을 약화시켜 아까 말했던 그에 따른 여러가지 이점들을 가지는 코드를 작성할 수 있게 됩니다.

## 정리
__인터페이스 분리 원칙의 기본은 큰 덩어리의 인터페이스를 구현하는 대신 이를 구체적으로 쪼개서 구현함으로써 클라이언트들이 꼭 필요한 메서드만 이용할 수 있도록 하는 것__ 이지만 _너무 원자단위로 쪼개다 보면 오히려 더러운 코드가 되기 십상입니다._ 따라서 반복적인 학습을 통해 ISP를 적용시켜보면서 적당한 선을 찾아낼 수 있도록 연습해보면 좋을 것 같습니다.