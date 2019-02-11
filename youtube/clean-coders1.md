# 백명석 클린 코더스 강의 1. 소개 및 OOP
## why clean code
## why OOP 
* 절차지향
    * 모든 프로시저가 데이터를 공유
* 객체지향
    * 프로시저를 실행하는데 필요한 만큼의 데이터만 가짐
    * 데이터와 그 데이터를 사용하는
## Object/Role/Responsibility
- 객체를 볼때 data로 보는것이 아니라 기능으로 봐야한다.
    - WriteArticleService (O)
    - ArticleService (X)
- 무엇으로 정의해야 한다 
    - RequestParser
- 어떻게로 정의하지 말아야한다.
    - JsonRequestParser
- 역할은 관롼된 책임의 집합 역할 하나에 책임이 여러개 있을 수 있다.
    - 역할 e.g) 글쓰기 사용자 댓글쓰기 사용자
## 객체지향 설계 과정
1. 기능을 제공할 객체 후보 선별 
    - 내부에서 필요한 데이터 선별
    - 클래스 다이어그램
2. 객체 간 메시지 흐름 연결
    - 커뮤니케이션 다이어그램
    - 시퀀스 다이어그램
1,2를 반복

## Encapsulation
- 내부적으로 어떻게 구현했는지를 감춰 내부의 변경 (데이터, 코드)이 client가 변경 되지 않도록
    -코드 내부의 변화

```java
public class ProceduralStopWatch {
    public long startTime;
    public long stopTime;
    public long startNanoTime;
    public long stopNanoTime;

    public long getElapsedTime() {
        return stopTime - startTime;
    }

    public long getElapsedNanoTime() {
        return stopNanoTime - startNanoTime;
    }
}
```

private 을 하고 getter setter로 하나 public이나...

객체 지향은 getter setter 가 없고 기능만 있다.

```java
public class StopWatch {
    private long startTime;
    private long stopTime;

    public void start() {
        startTime = System.nanoTime();
    }

    public void stop() {
        stopTime = System.nanoTime();
    }

    public Time getElapsedTime() {
        return new Time(stopTime - startTime);
    }
}
```

- Tell, Dont' Ask 
    - 객체가 자기가 가지고 있는 상태에 대해서 남에게 알려주는 경우가 많은데
    - e.g if(member.getExpiredDate().getTime() < System.currentTimeMillis) (X)
        if(member.isExpired())

- Command Vs Query
    - Command(Tell)
        - 객체의 내부 상태를 변경
    - Query(Ask)

## Polymorphism
- 한 객체가 여러가지(poly) 모습/타입을(morph) 가질 수 있다
상속을 통해 다형성을 구현
- inheritance (구현 상속)
    - 수퍼 타입의 구현을 재사용
    - 고치기 어렵다.
- interface (인터페이스 상속)
    - 타입 정의만 상속
    - 상속은 객체에게 다형성을 제공
    - 로직에서 변경 가능 한 부분을 외부에서 구현 외부에서 바라볼때는 인터페이스를 통해 바라본다. 바로 바라보면 바꿀 수가 없다.
    - 부가적인 기능을 넣기 쉽다. 유연하다.
    - 인터페이스를 사용하는 코드는 재사용이 가능하다.
    - 인터페이스 구현체 A 새로운 구현체 B가 들어가도 해당 구현체를 사용하는 애는 계속 사용 가능하다.
- 구현체에 변경이 일어나도 내가 작성한 코드는 변경이 안일어나는 것이 재사용.

## 타입 추상화
- Programming to interface
    - 구체적인 class를 보지 말고 interface를 보고 개발하라
- 추상화와 개발자의 습성
    - 상세한 구현을 하다보면 상위 수준의 설계를 놓치기 쉬운데 중간 중간 무엇을 하고 있는지 그림을 그려보는 것도 좋다. (말이 되는지, 이렇게 하는 것이 맞는지)
    - 놓치기 전 추상화를 한번 더 생각해봐라
    