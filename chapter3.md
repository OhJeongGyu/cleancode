### 2017. 2. 20
# 함수
---
### 작게 만들어라!
각 함수가 이야기 하나를 표현하도록! 10줄 이하가 되도록...

### 블록과 들여쓰기
```if```, ```else```, ```while``` 등에서 함수를 호출한다. 이러한 제어자에 붙는 블럭 내에서 함수를 호출하는데, 호출 함수의 이름을 적절히 지어 코드를 읽기 쉽게 만들자. 또한, 함수에서 들여쓰기 수준을 1단~2단을 넘어가도록 하지 말도록 하자.

### 한 가지만 해라!
> 함수는 한 가지를 해야 한다. 그 한가지를 잘 해야 한다. 그 한 가지만을 해야 한다. 

함수를 만드는 이유는 큰 개념을 다음 추상화 수준에서 여러 단계로 나눠 수행하기 위한 것이다. 
함수가 '**한 가지**'만 하는지 판단하는 방법은 의미 있는 이름으로 다른 함수를 추출할 수 있다면 그 함수는 여러 작업을 하는 것이다!

### 함수 당 추상화 수준은 하나로!
```getHtml()```와 ```PathParser.render(pagepath)```, 그리고 ```.append("\n")```의 추상화 수준은 모두 다르다. ```getHtml()```은 추상화 수준이 높고, ```PathParser.render(pagepath)```의 추상화 수준은 중간이며 ```.append("\n")```은 추상화 수준이 낮다. 이러한 다양한 추상화 수준의 코드를 함께 쓴다면 읽는 사람은 헷갈린다. 

***위에서 아래로 코드 읽기: 내려가기 규칙***
위에서 아래로 읽으면 함수의 추상화 수준이 한 번에 한 단계씩 낮아진다. 
다른 말로 
>TO ~~하려면, @@한다.

TO 문단은 현재 추상화 수준을 설명.

### Switch 문
각 Switch 문을 저차원 클래스에 숨기고 절대로 반복하지 않는 방법이 있다. **다형성**을 이용하는 것!

```
public Money calculatePay(Employee e) throws InvalidImployeesType{
    switch(e.type) {
        case COMMISSIONED :
            return calculateCommisionedPay(e);
        case HOURLY :
            return calculateHourlyPay(e);
        case SALARIED :
            return calcuateSalariedPay(e);
        dafault :
            throw new InvalidEmployeeType(e.type);
    }
}
```
**문제점**
1.함수가 길다. 새 직원 유형 추가되면 더 길어진다..
2.한 가지 작업만 수행하지 않는다.
3.SRP(Single Responsibility Principle)를 위반한다.
4.OCP(Open Closed Principles)를 위반한다. 직원 유형 추가할 때 마다 코드 변경

**And** 가장 심각한 문제는 위 함수와 동일한 구조의 함수가 무한정 존재한다.

```isPayday(Employee e, Date date);```,
```deliverPay(Employee e, Money pat);```
와 같은 함수이다. 동일한 구조를 반복할 수 밖에 없다. 하나의 Switch만을 허락한다!
**팩토리**를 사용해 switch문에서 적절한 Employee 파생 클래스의 인스턴스를 생성한다. 

