## String
- 불변성(immutable)
- 생성 시 String pool 이라는 공간에 생성, 생성되면 그 인스턴스의 메모리 공간은 절대 변하지 않음
- `+` 연산이나  `String.concat()` 을 이용해서 문자열 값에 변화를 줘도 메모리 공간 내의 값이 변하는게 아니라 새로운 메모리를 할당받아 새로운 String 클래스 객체를 할당받는 것 -> 연산마다 오버헤드가 발생함
- 위의 경우 GC에 의해 기존의 문자열이 제거되어야 함 (언제 제거될 지 모름)
- 대신, 조회의 경우 타 클래스보다 빠르게 읽을 수 있음


## StringBuilder, 
- 가변성(mutable)
- 문자열 연산이 자주 일어날 때 성능이 좋음
- StringBuilder는 동기화가 불가능
- 싱글스레드나 동기화가 필요없는 멀티 스레드 환경에서 유리

## StringBuffer
- 가변성(mutable)
- 문자열 연산이 자주 일어날 때 성능이 좋음
- 동기화가 가능함 -> Thread-safe함
- 멀티 스레드 환경에서 유리

![이미지](https://t1.daumcdn.net/cfile/tistory/99BE23375E2F133722)