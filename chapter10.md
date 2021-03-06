### 2017. 2. 27
# 클래스
---
###클래스 체계
클래스를 정의하는 표준 자바 관례
1. ```static public```상수 필드가 맨 처음에 온다.
2. ```static private```변수 필드
3. ```private``` 필드
4. ```public``` 필드가 필요한 경우는 없다.
5. ```public``` 메서드
6. ```private``` 메서드는 자신을 호출하는 ```public``` 메서드 직후에 넣는다.

***캡슐화***
변수나 메서드를 공개하지 않는 것이 좋지만, 반드시 숨겨야 하는 법도 없다. ```protected```로 선언해 테스트 코드에서 접근이 가능하도록 하기도 한다. 

###클래스는 작아야 한다!
작명은 클래스 크기를 줄이는 첫 번째 관문이다. 클래스의 설명은 if, and, or, but을 사용하지 않고서 25단어 내외로 하자. 

***단일 책임 원칙 (Single Responsibility Principle)***
클래스나 모듈을 변경할 이유가 단 하나뿐이여야 한다는 원칙이다.
> 도구 상자를 어떻게 관리하고 싶은가? 작은 서랍을 많이 두고 기능과 이름이 명확한 컴포넌트를 나눠 넣고 싶은가? 아니면 큰 서랍을 몇 개를 두고 모두를 던져 넣고 싶은가?

***응집도***
클래스는 인스턴스 변수 수가 작아야 한다. 일반적으로 메서드가 변수를 더 많이 사용 할수록 메서드와 클래스의 응집도가 더 높다. 
> '함수를 작게, 매개변수 목록을 짧게' 라는 전략을 따르다 보면 때때로 몇몇 메서드만 사용하는 인스턴스 변수가 아주 많아진다. 이는 새로운 클래스로 쪼개야 한다는 신호다. 

***응집도를 유지하면 작은 클래스 여럿이 나온다.***
여러 개의 변수를 사용하는 메서드를 여러 개의 메서드로 나눌 때, 나뉜 메서드가 네 개의 변수를 사용한다면, 인수로 넘겨야 할까? ***No!*** 변수를 클래스 인스턴스 변수로 승격하라. 매개변수가 전혀 필요없다. 
이렇게 하면 클래스가 응집력을 잃는다. 당연스럽게 필드 몇몇개만 사용하는 메서드들이 생기게 된다. 그렇다면, 독자적인 클래스로 분리할 수 있게된다!

###변경하기 쉬운 클래스 
새 기능을 수정하거나 기존 기능을 변경할 때 건드릴 코드가 최소인 구조가 바람직하다.

***변경으로부터 격리***
클래스는 구체적인 클래스와 추상적인 클래스로 나뉜다. 구체적인 클래스는 상세한 기능을 구현한 클래스이고, 추상적인 클래스(추상 클래스, 인터페이스)는 개념만을 포함하며, 구현에 미치는 영향을 격리시킨다. 상세한 구현에 의존하는 코드는 테스트하기 어렵다. 

```Portfolio``` 클래스는 ```TokyoStockExchange``` API를 직접 호출하는 대신 ```StockExchange```라는 인터페이스를 생성한 후 메서드 하나를 선언한다. ```StockExchange```를 구현하여 ```TokyoStockExchange```를 작성하고, ```StockExchange```를 흉내내는 테스트용 클래스를 또한 작성할 수 있다. 

```
public class Portfolio {
    private StockExchange exchange;
    public PortFolio(StockExchange exchange) {
        this.exchange = exchange;
    }
}

public interface StockExchange {
    Money currentPrice(String symbol);
}
```




















