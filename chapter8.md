### 2017. 2. 23 
# 경계
---
> 외부 패키지, 혹은 오픈소스를 사용할 때 소프트웨어의 경계를 깔끔하게 처리해야 한다.

###외부 코드 사용하기
Java의 ```Map``` 클래스에는 어떠한 자료도 넣을 수 있다.

```
Map sensors = new HashMap();
sensors s = (Sensor) sensors.get(sensorId);
```
위의 코드는 지저분하지만, 작동은 한다. 내부에 저장된 자료를 얻기 위하여 쓸데없는 코드가 많이 필요하다.

```Generics```를 사용한 방법

```
Map<String, Sensor> sensors = new Hashmap<>();
...
Sensor s = sensors.get(sensorId);
...
```
 제네릭스를 사용하면 사용자에게 필요하지 않은 기능까지 제공한다. 즉, ```Map<string, Sensor>``` 인스턴스를 여기저기 넘긴다면, ```Map``` 인스턴스가 변할 경우, 수정해야 할 코드가 상당히 많아진다. 
제네릭스를 금지하는 코드도 많았다.

제네릭스를 사용하지 않고, 다음과 같이 작성할 수 있다.
```
public class Sensors {
  private Map sensors = new HashMap();
  public Sensor getById(String id) {
    return (Sensor) sensors.get(id);
  }
}
```

위 코드의 장점은 ```Sensors``` 클래스 내부에서 객체 유형을 관하는 것이다.

### 경계 살피고 익히기
외부 코드를 사용하는 것은 어렵다. 곧바로 나의 코드에 외부 코드를 붙이지 말고 내 코드의 테스트 케이스를 작성해 테스트하고, 외부 코드를 익힌 후 추가하라.

### log4j 익히기

### 학습 테스트는 공짜 이상이다.





































