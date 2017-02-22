### 2017. 2. 22 
# 객체와 자료구조
---
> 필드는 왜 ```private```로 선언하고, ```getter```와 ```setter```는 ```public```으로 공개할까?

### 자료 추상화
필드를 감추고 ```getter```와 ```setter```를 만든다고 진정한 의미의 클래스가 되지 않는다. 진정한 의미의 클래스는 추상 인터페이스를 제공하여 사용자가 구현을 모른 채 자료의 핵심을 조작할 수 있어야 한다. 
```
//구체적인 Vehicle 클래스
public interface Vehicle {
    double getFuelTankCapacityInGallons();
    double getGallansOfGasoline();
}

//추상적인 Vehicle 클래스
public interface Vehicle {
    double getPercentFuelRemaining();
}
```

자료를 세세하게 공개하는 것 보다 아래의 클래스처럼 추상적인 개념으로 표현하는 것이 좋다!

### 자료/객체 비대칭
***자료구조***? 필드만을 가지는 클래스. 필드에 대한 ```getter``` 혹은 ```setter```를 갖지 않는다.

> (자료구조를 사용하는) 절차적인 코드는 기존 자료 구조를 변경하지 않으면서 새 함수를 추가하기 쉽다. 반면, 객체 지향 코드는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽다.

반대도 참이다.

> 절차적인 코드는 새로운 자료 구조를 추가하기 어렵다. 그러려면 모든 함수를 고쳐야 한다. 객체 지향 코드는 새로운 함수를 추가하기 어렵다. 그러려면 모든 클래스를 고쳐야 한다.

> 때로는 단순한 자료구조와 절차적인 코드가 적합한 상황도 있다.

### 디미터 법칙
> '모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다'는 법칙.

클래스```C```의 메서드 ```f```는 다음과 같은 객체의 메서드만 호출해야 한다.
1. 클래스C
2. f가 생성한 객체
3. f 인수로 넘어온 객체
4. C 인스턴스 변수에 저장된 객체

```
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath(); 
```

위의 코드는 디미터 법칙을 위반한다. 한 클래스 내에서 위의 코드가 존재한다면, ```getOptions()```에서 반환된 객체의 메서드인 ```getScratchDir()```를 호출하기 때문이다. 또한, ```getAbsolutePath()```도 마찬가지이다.

***기차 충돌***
위와 같은 코드를 기차충돌이라 부른다. 위의 코드를 나누면 다음과 같다.
```
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```

만약 세 개의 필드가 객체라면 여전히 디미터 법칙을 위반한다. 하지만, 자료구조라면 객체가 아니기 때문에 디미터 법칙이 적용되지 않는다. 만약 자료구조였다면 애초에 다음과 같이 작성할 수 있다.
```
final String outputDir = ctxt.options.scratchDir.absoultePath;
```

***잡종 구조***
절반은 객체, 절반은 자료 구조인 잡종 구조도 있다. 이는 객체와 자료구조의 단점만 모아놓은 구조이다. 피하자

***구조체 감추기***












