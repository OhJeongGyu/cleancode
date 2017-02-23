### 2017. 2. 23 
# 오류 처리
---
> 깨끗한 코드와 오류처리는 밀접한 관련이 있다. 여기저기 흩어진 오류 처리 코드 때문에 실제 코드가 하는 일을 파악하기 어려워진다.

###오류 코드보다 예외를 사용하라.
이전의 프로그래밍 언어들은 예외처리를 지원하지 않았다. 이러한 언어에서는 메서드에 플래그와 같은 오류코드를 설정하여 처리하였다. 이 방식은 코드를 매우 복잡하게 만든다. 최근 언어들은 예외를 처리할 수 있는 코드를 지원하기 때문에 더욱 깔끔한 코드를 작성할 수 있다.

###Try-Catch-Fianally 문부터 작성하라.
예외 처리 코드는 **범위**를 지정한다. ```try```블록 안에서 예외가 발생할 수 있는 코드가 위치한다. 이 블록은 트랜잭션과 같다. 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성하라. 그러면 자연스럽게 ```try```블록의 트랜잭션 범위부터 구현하게 되므로 범위 내에서 트랜잭션 본질을 유지하기 쉬워진다.

###미확인(unchecked)예외를 사용하라
**확인된(checked) 예외**??
메서드를 선언할 때 메서드가 반환할 예외를 모두 열거. 반환하는 예외는 메서드 유형의 일부였다. 메서드를 사용하는 방식이 메서드 선언과 일치하지 않으면 컴파일이 불가능했다.

**문제점**
확인된 예외가 갖는 장점도 있지만, 반드시 필요한 것은 아니다. C++, C#, 루비, 파이썬과 같은 언어에서 확인된 예외를 지원하지 않지만, 충분히 안정적인 프로그래밍이 가능하다. 확인된 예외를 지원하기 위한 ***비용***이 너무 크다. 

**비용**
확인된 예외를 OCP(Open Closed Principle)을 위반한다. 예외를 던졌는데 ```catch``` 블록이 세 단계 위에 있다면 그 사이 메서드가 모두 해당 예외를 정의해야한다. 만약 N개의 연쇄적으로 호출되는 함수들이 있다면 첫 번째 함수가 예외를 던질 때 N번째 함수까지 모두 예외를 처리하는 코드를 추가해야한다는 것이다. 이는 최하위 메서드에서 던지는 예외를 알아야하므로 캡슐화 또한 깨져버린다. 

**결론**
때로는 유용할 때도 있다. 아주 중요한 라이브러리는 모든 예외를 잡아야한다. 하지만 일반적인 어플리케이션은 의존성이라는 비용이 이익보다 크다.

###예외에 의미를 제공하라
자바의 예외는 콜스택을 제공한다. 하지만 콜스택만으로 실패 코드의 의미를 파악하기엔 부족하다. 오류메세지에 정보를 담자.

###호출자를 고려해 예외 클래스를 정의하라.
예외를 구분하는 방법은 다양하다. 
- 오류가 발생한 위치
    - 오류가 발생한 컴포넌트로 분류
- 오류의 유형
    - 디바이스 실패, 네트워크 실패, 프로그래밍 오류 등
    
But, 프로그래머가 가장 중요시해야 하는 것은 ***오류를 잡아내는 방법***이어야 한다.

예외를 하나하나 처리하는 코드를 작성한다면 형편없는 코드가 될 것이다.

```
public class LocalPort{
    private ACMEPort innerPort;
    public LocalPort(int portNumber){
    innerPort = portNumber;
    }
    
    public void open(){
        try{
            innerPort.open();
        }catch(DeviceResponseException e){
            throw new PortDeviceFailure(e);
        }catch(...Exceoption e){
            throw new PortDeviceFailure(e);
        }
        ...
    }
}

Localport port = new LocalPort(12);
try{
    port.open();
} catch (PortDeviceFailure e){
    e.~~~
}

```
위와 같이 예외를 처리하는 클래스를 만들어 감쌀 수 있다. 

###정상 흐름을 정의하라
***특수 사례 패턴(Special Case Pattern)***
예외를 클래스 또는 메서드 내에서 처리하도록 하는 코드를 캡슐화하여 클라이언트 코드에서 신경을 쓰지 않도록 하는 패턴.

###null을 반환하지 마라
메서드에서 null을 반환하고싶은 유혹이 든다면 예외를 던지거나 특수 사례 객체를 반환하라. 
```
List<Employee> employees = getEmployees();
if(employees!=null){
    if(Employee e : employees) {
        totalPay += e.getPay();
    }
}
 
```
다음과 같은 코드에서 ```getEmployees```는 null도 반환한다. null을 반환하지 않는다면    
```
List<Employee> employees = getEmployees();
if(Employee e : employees) {
    totalPay += e.getPay();
}
```
와 같은 코드를 작성할 수 있다. Java의 ```Collection``` 프레임워크에는 빈 ```List```를 제공하는 함수가 존재한다.

```
public List<Employee> getEmployees(){
    if( ...직원이 없다면){
        return Collections.emptyList(); 
    }
}
```

이처럼 null을 리턴하지 않는다면 코드도 깔끔해지고, ```NullPointerException```이 발생할 가능성이 줄어든다.

###null을 전달하지 마라.
null을 반환하는 메서드는 나쁘지만, null을 전달하는 메서드 또한 나쁘다. 메서드로 null을 전달하는 메서드 또한 피하자. 

***TODO***
annotation으로 null 전달 막기!?

###결론
> 깨끗한 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다. 오류처리를 프로그램 논리와 분리시키면 튼튼하고 깨끗한 코드를 작성할 수 있다. 







































