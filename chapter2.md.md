### 2017. 2. 15
# 의미 있는 이름
---

### 의도를 분명히 밝혀라.
변수나 함수, 클래스의 이름은 다음의 질문에 모두 답할 수 있어야 한다.
> 변수(혹은 함수나 클래스)의 존재 이유는? 수행 기능은? 사용 방법은?  

주석이 필요하다면 의도를 드러내지 못한 것이다.

### 그릇된 정보를 피하라.
계정을 담는 컨테이너의 이름이 ``` accountList``` 인데, 실제 ```List```가 아니라면? 혼란을 가져온다. 차라리 ```accountGroup```, 혹은 ```bunchOfAccounts```와 같이 명명하라.

서로 흡사한 이름을 사용하지 않는다. ```XYZControllerForEfficientHandlingOfStrings``` 와 ```XYZControllerForEfficientStorageOfStrings```를 동시에 상용한다면?

최악의 변수명? 소문자```L```, 대문자```O```을 동시에 사용할 때. 

### 의미 있게 구분하라.
