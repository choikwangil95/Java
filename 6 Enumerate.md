## Enumerate 열거형

서로 관련 있는 상수들을 모아 심볼릭한 명칭의 집합으로 정의한 것<br/>
Enum 클래스형을 기반으로 한 클래스형 선언<br/>
새로운 열거형을 선언하면, 내부적으로 Enum 클래스형 기반의 새로운 클래스형이 만들어짐<br/><br/>
### Reference
[Enum 정의](https://www.opentutorials.org/module/1226/8025)<br/>
[Enum 사용 이유](https://heepie.me/32)

### Code
```java
<Shoes.java>

enum Type {
    WALKING, RUNNING, TRACKING, HIKING
}
public class Shoes {
    public String name;
    public int size;
    public Type type;
     
    public static void main(String[] qrgs) {
        Shoes shoes = new Shoes();
         
        shoes.name = "나이키";
        shoes.size = 230;
        shoes.type = Type.RUNNING;
         
        System.out.plintln("신발 이름 = " + shoes.name);
        System.out.plintln("신발 사이즈 = " + shoes.size);
        System.out.plintln("신발 종류 = " + shoes.type);
    }
}

// 결과
// 신발 이름 = 나이키
// 신발 사이즈 = 230
// 신발 종류 = RUNNING
```

### Method
#### 1) values()
열거된 모든 원소를 배열에 담아 순서대로 반환
```java

enum Type {
    WALKING, RUNNING, TRACKING, HIKING
}

public class Shoes {
    public String name;
    public int size;
    public Type type;
     
    public static void main(String[] args) {
        for(Type type : Type.values()) {
            System.out.println(type);
        }
    }
}
// 결과
// WALKING
// UNNING
// TRACKING
// HIKING

```
#### 2) ordinal()
원소에 열거된 순서를 정수 값으로 반환
```java

enum Type {
    WALKING, RUNNING, TRACKING, HIKING
}

public class Shoes {
    public String name;
    public int size;
    public Type type;
     
    public static void main(String[] args) {
        Shoes shoes = new Shoes();
         
        shoes.name = "나이키";
        shoes.size = 230;
        shoes.type = Type.RUNNING;
         
        System.out.println(shoes.type.ordinal());
         
        Type tp = Type.HIKING;
         
        System.out.println(tp.ordinal());
    }
}

// 결과
// 1
// 3
```
#### 3) valueOf() 
매개변수로 주어진 String과 열거형에서 일치하는 이름을 갖는 원소를 반환
```java
enum Type {
    WALKING, RUNNING, TRACKING, HIKING
}

public class Shoes {
     
    public static void main(String[] args) {
        Type tp1 = Type.WALKING;
        Type tp2 = Type.valueOf("WALKING");
         
        System.out.println(tp1);
        System.out.println(tp2);
    }
}
// 결과
// WALKING
// WALKING

```
#### 4) 열거형 상수를 다른 값과 연결하기
```java
enum Type {
    // 상수("연결할 문자")
    WALKING("워킹화"), RUNNING("러닝화")
    , TRACKING("트래킹화"), HIKING("등산화")
     
    final private String name;
     
    private Type(Stirng name) { //enum에서 생성자 같은 역할
        this.name = name;
    }
    public String getName() { // 문자를 받아오는 함수
        return name;
    }
}

public class Shoes {
    public static void main(String[] args) {
        for(Type type : Type.values()){
            System.out.println(type.getName());
        }
    }
}
// 결과
// 워킹화
// 러닝화
// 트래킹화
// 등산화

```
