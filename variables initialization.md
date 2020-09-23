## 클래스 변수 초기화
초기화 되지 않은 변수는 null pointer exception error를 발생시키기 때문에 초기화가 필요함 
#### ref
[1](https://doublesprogramming.tistory.com/73)

#### 변수 초기화 방법에는 3가지 방법이 있다
- 명시적 초기화 (Explicit initialization)
- 생성자 (Constructor)
- 초기화 블럭 (클래스 초기화 / 인스턴스 초기화) (Initializaiton block)
<br/>

### 1 명시적 초기화 (Explicit initialization)
변수 선언과 함깨 동시에 초기화 하는 것을 명시적 초기화라고 함
```java
class Car {
int door = 4;                   // 기본형 변수 초기화
Enging engine = new Engine();   // 참조형 변수 초기화
}
```

### 2 초기화 블럭 (Initializaiton block)
인스턴스 변수의 초기화는 주로 생성자를 사용한다
#### 인스턴스 초기화 블럭은 모든 생성자에게 공통으로 수행되어야 하는 코드임
```java
class InitBlock {
  
  static {  // 클래스 초기화 블럭
  
  }
  
  {         // 인스턴스 초기화 블럭
  
  }

}
```
예시
```java
// 인스턴스 초기화 블럭을 통해 중복 제거 -> 모든 생성자에게 적용됨
{
  count++;
  serialNo = count;
}

// 생성자
Car() {
  color = "white";
  gearType = "auto";
}

Car(String color, String gearType) {
  this.color = color;
  this.gearType = gearType;
}
```

초기화 순서는 class 초기화 -> 인스턴스 블럭 -> 생성자 순으로 초기화가 진행됨 <br/>
class 초기화는 메모리에 로딩될 때 한번만 초기화 됨 <br/>
인스턴스 블럭, 생성자는 인스턴스가 생성될 때 마다 초기화 <br/>

### 생성자 Method Overideing

객체 생성 시 인자값에 들어오는 변수에 따라 초기화되는 값이 다르다

```java

public class Account {

  private final String name;
  
  public Account(String name, double balance) {
  
    this.name = ~~;
  
  }

  public Account(String name) {
  
    this(name, 0.0) = ~~; // call Account(name, 0.0)
  
  }
}
```
