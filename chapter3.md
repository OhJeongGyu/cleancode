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

```
public interface EmployeeFactory{ 
    public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
}

public cass EmployeeFactoryImpl implementes EmployeeFactory {
    public Employee(EmployeeRecord r) throws InvalidEmployeeType{
        switch(r.type){
            ...
        }
    }
}
```

다음과 같이 다형적 객체를 생성하고, 내부의 switch문은 다른 코드에 절대로 노출시키지 않는다.

### 서술적인 이름을 사용하라!

### 함수 인수
이상적인 함수 인수는 0개이다. 다음은 1개, 그다음은 2개, 3개는 가능하면 피하는 편이 좋다. 4개 이상은 특별한 이유가 필요하다.
```includeSetupPageInto(newPageContent)``` 보다 ```includeSetupPage()```가 이해하기 쉽다. ```includeSetupPageInto(newPageContent)``` 는 함수 이름과 인수 사이에 추상화의 수준이 다르다. 또한, StringBuffer를 알아야한다. 

**테스트 관점**
인수가 없으면 간단하지만, 두 개 부터 복잡해지고, 3개면 상당히 부담스러워진다.

***많이 쓰는 단항 형식***
함수에 인수가 1개인 경우는 두 가지 이다.
- 인수에 질문을 던지는 경우

```
boolean fileExists("MyFile");
```

- 인수를 뭔가로 변환해 결과를 반환하는 경우

```
InputStream fileOpen("MyFile")
```

String 파일 이름을 입력받아 InputStream 타입으로 반환한다.

> 함수 이름을 지을 때 언제나 분명히 구분하라!


아주 유용한 단항 함수 형식 **이벤트**
```
passwordAttemptFailedNtimes(int attempts)
```
함수 호출을 이벤트로 해석해 입력 인수로 시스템의 상태를 바꾼다. 

```
void transform(StringBuffer out);
```
보다는  
```
StringBuffer transform(StringBuffer pageText);
```
와 같이 사용하자. 


***플래그 인수***
플래스 인수는 추하다..함수가 **여러 가지** 일을 한다는 것을 공표한다....
```
render(boolean isSuite)
```
보다는 
```renderForSuite()```와 ```renderForSingleTest()```로 나누어 사용하자!

***이항 함수***
```writeField(name)```은 ```write(outputStream, name)```보다 이해하기 쉽다. 후자는 ```write(outputStream, name)```는 첫 번째 인수를 무시해야 한다는 사실을 시간이 좀 지난 후에 깨닫는다. 그리고 문제를 일으킨다. 
***Why?*** 어떤 코드든 절대로 무시를 하면 안된다. 

***삼항 함수***
신중하라...

***인수 객체***
여러 개의 인수가 필요하다면 독자적인 크ㄹ래스 변수로 선언하자. 변수를 묶어 넘기려면 이름이 필요하므로 결국 개념을 표현하게 된다!
```
Circle makeCircle(double x, double y, double radius);
Circle makeCircle(Point center, double radius);
```

***인수 목록***
인수가 가변적인 함수. 
```String.format("%s worked %.2f hours);``` 처럼 가변 인수를 전부 동등하게 취급하면 List형 인수 하나로 취급할 수 있다. 실제 선언부는 다음과 같다.
``` public String format(String format, Object... args) ```

***동사와 키워드***
함수의 의도나 인수의 순서와 의도를 제대로 표현하려면 좋은 함수 이름이 필수이다. 단항 함수는 함수와 인수가 동사/명사 쌍을 이뤄야 한다.
```write(name)```처럼 ```이름```과 ```쓴다```는 짝을 이루어야 한다. 좀 더 나은 이름은 ```writeField(name)```이다. 이름이 필드라는 사실이 분명해진다. 
함수 이름에 키워드를 추가하는 형식. 함수 이름에 인수 이름을 넣자.
```assertEquals```보다 ```assertExpectedEqualsActual(expected, actual)```로 변경하자. 인수의 순서를 기억 할 필요가 없다.

### 부수 효과를 일으키지 마라!
함수에서 한 가지를 하겠다고 약속하고 ***남몰래*** 다른 짓을 하게 하지 마라!

***출력 인수***
```appendFooter(s);```에서 ```s```는 출력일까 입력일까?
선언문에는 ```public void appendFooter(StringBuffer report)```라 선언되어 있다. 인수가 출력을 위한 인수이다. 객체지향 언어에서 이러한 출력을 위한 인수는 필요하지 않다. 
```this```를 사용하자. ```report.appendFooter```와 같이 호출하는 방식이 좋다.

### 명령과 조회를 분리하라
함수는 뭔가를 수행하거나 뭔가에 답하거나 둘 중 하나만 해야 한다. 

```public booleab set(String attribute, String value);```

이 함수를 호출하면, 

```if (set("username", "unclebob"))...```
와 같은 괴상한 코드가 만들어진다.









