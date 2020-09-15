## Static
static은 보통 변수나 메소드 앞에 static 키워드를 붙여서 사용하게 된다.

### 1 Static 변수
웹 사이트 방문시마다 조회수를 증가시키는 Counter 프로그램이 다음과 같이 있다고 가정 해 보자
```java
public class Counter  {
    int count = 0;
    Counter() {
        this.count++;
        System.out.println(this.count);
    }

    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();
    }
}
```
프로그램을 수행해 보면 다음과 같은 결과값이 나온다<br/>
```
1
1
```
c1, c2 객체 생성시 count 값을 1씩 증가하더라도 <br/>
c1과 c2의 count는 서로 다른 메모리를 가리키고 있기 때문에 원하던 결과(카운트가 증가된)가 나오지 않는 것이다. <br/>
객체변수는 항상 독립적인 값을 갖기 때문에 당연한 결과이다.<br/>
```java
public class Counter  {
    static int count = 0;
    Counter() {
        this.count++;
        System.out.println(this.count);
    }

    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();
    }
}
```
int count = 0 앞에 static 키워드를 붙였더니 count 값이 공유되어 다음과 같이 방문자수가 증가된 결과값이 나오게 되었다<br/>
```
1
2
```
보통 변수의 static 키워드는 프로그래밍 시 메모리의 효율보다는 두번째 처럼 공유하기 위한 용도로 훨씬 더 많이 사용하게 된다.
클래스 내의 변수에 static을 앞에 붙여주면, 해당 클래스로 생성된 객체들이 해당 변수의 값에 대해 메모리를 새로 할당하지 않고 <br/>
첫 객체 생성 이후 해당 변수 값을 공유하게 된다<br/>
#### static으로 선언된 변수는 선언된 클래스의 모든 인스턴스가 공유하게 됩니다.

### 2 Static method
```java
public class Counter  {
    static int count = 0;
    Counter() {
        this.count++;
    }

    public static int getCount() {
        return count; // static method는 static 변수에만 접근 가능, instance 변수에는 접근 불가능
    }

    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();

        System.out.println(Counter.getCount()); // 객체 생성없이 호출 가능
        System.out.println(c1.getCount()); // 객체에서 static method에 접근 불가능
        
    }
}
```
getCount() 라는 static 메소드가 추가되었다. <br/>
main 메소드에서 getCount() 메소드는 Counter.getCount() 와 같이 클래스를 통해 호출할 수 있게 된다.<br/>
staic메서드는 객체의 생성 없이 호출이 가능하고, 객체에서는 호출이 불가능하다. <br/>
또한 static 메서드 안에서는 인스턴스 변수 접근이 불가능하다. 


## 결론
### 1) 변수 -> 인스턴스에 공통적으로 사용해야하는 것에 static을 붙인다
인스턴스를 생성하면, 각 인스턴스들은 서로 다른 메모리들의 주소를 할당 받기 때문에 서로 다른 값을 유지한다. <br/>
하지만 인스턴드을이 공통적인 값이 유지되어야 하는 경우에는 static을 붙인다.
### 2) method -> 전역으로 자주 사용할 메서드를 static메서드로 만들어 사용한다.
프로젝트 내에서 공통적으로 사용해야할 메서드가 있으면 static 메서드로 만들어서 불필요한 코드의 수를 줄인다.<br/>
이때 인스턴스 변수가 메서드 내부에 필요한가에 대해서도 고려를 해야한다.


