# 가비지 컬렉션

## 가비지 컬렉션이란?

가비지 컬렉션(GC, Garbage Collection)은 Heap에서 더 이상 사용하지 않는 객체를 JVM이 자동으로 찾아 메모리를 회수하는 기능이다.

C나 C++처럼 개발자가 직접 메모리를 해제하는 언어에서는 해제를 빠뜨리면 메모리 누수가 생기고, 
이미 해제한 메모리에 접근하면 프로그램이 비정상적으로 동작한다.
자바는 이 작업을 GC가 대신하기 때문에 개발자가 메모리 해제 시점을 직접 관리하지 않아도 된다.

하지만 메모리 해제 시점을 정확하게 알수없어서 제어가 힘들고 GC가 동작할때 다른 동작을 멈추기 때문에 오버헤드가 발생한다(실시간성이 중요한 곳에서 따로 GC튜닝 작업을 요하게됨)

## GC의 동작 원리

GC는 객체가 실제로 사용 중인지 확인하기 위해 GC Root를 기준으로 객체의 참조 관계를 확인한다.

- GC Root: 객체 참조를 탐색하기 시작하는 지점
- Reachable: GC Root에서 접근할 수 있는 객체
- Unreachable: GC Root에서 접근할 수 없는 객체

Unreachable 상태의 객체는 더 이상 사용하지 않는 것으로 판단해 메모리에서 제거한다.

객체끼리 서로 참조하고 있어도 GC Root에서 접근할 수 없다면 GC 대상이 된다.

## Heap 영역과 GC

Heap은 객체의 생존 기간에 따라 크게 Young Generation과 Old Generation으로 나뉜다.

### Young Generation

새로 생성된 객체가 저장되는 영역

- **Eden:** 객체가 처음 생성되는 영역
- **Survivor:** GC에서 살아남은 객체가 이동하는 영역
- **Minor GC:** Young Generation에서 발생하는 GC

대부분의 객체는 생성된 후 금방 사용이 끝나기 때문에 Young Generation에서는 GC가 자주 발생한다.

### Old Generation

Young Generation의 GC에서 여러 번 살아남은 객체가 이동하는 영역

- 오래 사용하는 객체가 저장됨
- Young Generation보다 크기가 큼
- GC가 발생하면 비교적 오래 걸림

Young Generation의 객체가 Old Generation으로 이동하는 것을 승격(Promotion)이라고 한다.

## GC의 기본 처리 과정

GC의 처리 과정은 크게 세 단계로 이해할 수 있다.

1. **Mark:** 사용 중인 객체를 표시한다.
2. **Sweep:** 사용하지 않는 객체를 제거한다.
3. **Compact:** 살아 있는 객체를 한쪽으로 모아 빈 공간을 정리한다.

Compact 과정은 흩어진 빈 공간을 하나로 모아 새로운 객체가 들어갈 공간을 확보한다.

## STW

STW(Stop The World)는 GC 작업을 위해 애플리케이션의 실행을 잠시 멈추는 것이다.

GC가 오래 걸리면 애플리케이션의 응답도 늦어질 수 있다. 따라서 GC는 메모리를 자동으로 관리해 주지만 성능 저하를 일으킬 수도 있다.

최근의 GC는 애플리케이션과 동시에 일부 작업을 수행해 STW 시간을 줄인다.

## GC 종류

대표적인 GC는 다음과 같다.

| GC | 특징 |
| --- | --- |
| Serial GC | 하나의 스레드로 GC를 수행함 |
| Parallel GC | 여러 스레드로 GC를 수행해 처리량을 높임 |
| G1 GC | Heap을 여러 영역으로 나누어 필요한 영역부터 정리함 |
| ZGC | STW 시간을 짧게 유지하는 것을 목표로 함 |

Java 9부터는 G1 GC가 기본 GC로 사용된다.

## GC와 메모리 누수

GC가 있어도 메모리 누수가 발생할 수 있다.

GC는 참조가 남아 있는 객체를 사용 중인 객체로 판단한다. 따라서 실제로는 사용하지 않더라도 코드에서 계속 참조하고 있다면 제거하지 못한다.

```java
private static final List<Member> members = new ArrayList<>();

public void add(Member member) {
    members.add(member);
}
```

## 추가 내용

Q. GC가 있는데도 OutOfMemoryError가 발생할 수 있을까?
- 사용하지 않는 객체의 참조가 계속 남아 있을 수 있음
- 생성하는 객체의 양이 Heap 크기보다 많을 수 있음
- GC가 메모리를 무한하게 만들어 주는 것은 아님

Q. System.gc()를 호출하면 바로 GC가 실행될까?
- JVM에 GC 실행을 요청하는 것일 뿐 즉시 실행을 보장하지 않음
- Full GC를 유발해 성능이 저하될 수 있으므로 일반적으로 직접 호출하지 않음


