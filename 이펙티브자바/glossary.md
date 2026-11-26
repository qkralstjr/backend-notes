## 용어정리

| 용어 | 뜻 | 나온 곳 |
| --- | --- | --- |
| 박싱 클래스(boxed class) | 기본 타입(`int`, `boolean` 등)을 객체로 감싼 클래스. `Integer`, `Boolean` 등. 래퍼 클래스(wrapper class)라고도 한다 | [아이템 1](객체생성과파괴/static-factory-method.md) |
| 플라이웨이트 패턴(flyweight pattern) | 같은 객체를 매번 만들지 않고, 한 번 만든 객체를 공유해서 쓰는 패턴 | [아이템 1](객체생성과파괴/static-factory-method.md) |
| 인스턴스 통제 클래스(instance-controlled class) | 언제 어떤 인스턴스를 살아 있게 할지 클래스가 직접 통제하는 클래스 | [아이템 1](객체생성과파괴/static-factory-method.md) |
| 동반 클래스(companion class) | 인터페이스용 정적 메서드를 모아 두는 인스턴스화 불가 클래스. 예: `Collection` → `Collections` | [아이템 1](객체생성과파괴/static-factory-method.md) |
| 서비스 제공자 프레임워크(service provider framework) | 구현체를 클라이언트에 연결하는 일을 프레임워크가 맡아, 클라이언트가 구현체를 몰라도 되게 하는 구조. 예: JDBC | [아이템 1](객체생성과파괴/static-factory-method.md) |
