# Chapter1 자바 8을 눈여겨봐야 하는 이유

### 이 장의 내용

- 자바가 거듭 변화하는 이유
- 컴퓨팅 환경의 변화 : 멀티코어와 대용량 데이터집합(빅데이터) 처리
- 시대적 변화 요구 : 기존의 명령형을 탈피한 함수형 스타일의 새로운 아키텍처
- 자바 8의 새로운 기능 소개 : 람다, 스트림, 디폴트 메서드

### 자바 8을 구성하는 핵심사항 : 간결한 코드와 멀티코어 프로세서의 간단한 활용

### 자바 8에서 제공하는 새로운 기술

- 스트림 API
- 메서드에 코드를 전달하는 기법
- 인터페이스의 디폴트 메서드

---

## 1.1 왜 아직도 자바는 변화하는가?

많은 프로그래밍 언어 중 완벽한 프로그래밍 언어를 찾고자 함

### 1.1.1 프로그래밍 언어 생태계에서 자바의 위치

### 1.1.2. 스트림 처리

- 스트림 : 한 번에 한 개씩 만들어지는 연속적인 데이터 항목들의 모임
- java.util.stream 패키지, Stream<T> : T 형식으로 구성된 일련의 항목
- 스트림 API가 조립라인처럼 어떤 항목을 연속으로 제공하는 어떤 기능이라고 생각!
- 스트림 API의 핵심 : 고수준으로 추상화, 공짜로 병렬성을 얻는다!

### 1.1.3 동작 파라미터화로 메서드에 코드 전달하기

- 코드 일부를 API로 전달하는 기능
- 동작 파라미터화(behavior parameterization) : 메서드를 다른 메서드의 인수로 넘겨주는 기능

### 1.1.4 병렬성과 공유 가변 데이터

- 병렬성을 공짜로 얻을 수 있다.
- 대신 스트림 메서드로 전달하는 코드의 동작 방식을 조금 바꿔야 한다.
- 안전하게 실행될 수 있어야 한다. → 공유된 가변 데이터(shared mutable data)에 접근하지 X
- 위 함수를 순수(pure) 함수, 부작용 없는(side-effect-free) 함수, 상태 없는(stateless) 함수라 부른다.
- synchronized

### 1.1.5 자바가 진화해야 하는 이유

- 언어는 하드웨어나 프로그래머 기대의 변화에 부응할 수 있도록 변화해야 한다는 것이다

---

## 1.2 자바 함수

- 함수(function) = 메서드(method) = 정적 메서드(static method)
- 자바의 함수는 수학적인 함수처럼 사용되며 부작용을 일으키지 않은 함수를 의미
- 프로그래밍 언어의 핵심은 값을 바꾸는 것 → 일급(first class) 값이라 부름(전통적, 역사적으로)
- 일급시민 : int, double 형식, 객체의 레퍼런스
- 이급 시민 : 전달할 수 없는 구조체(메서드, 클래스 등)

### 1.2.1 메서드와 람다를 일급 시민으로

- 메서드 레퍼런스(method reference) → " :: "
- 기존 : 객체 레퍼런스(object reference) : new로 객체 레퍼런스를 생성함

```java
//기존
File[] hiddenFiles = new File(".").listFiles(new FileFilter(){
	public boolean accept(File file){
		return file.isHidden();
	}
});

//자바 8 스타일
File[] hiddenFiles = new File(".").listFiles(File::isHidden);
```

- 람다 : 익명 함수
(기명named) 메서드를 일급값으로 취급할 뿐만 아니라 람다(또는 익명anonymous 함수)를 포함하여 함수도 값으로 취급할 수 있다.
- 프리디케이트(predicate) : 수학에서 인수로 값을 받아 true나 false를 반환하는 함수

### 1.2.3 메서드 전달에서 람다로

- 한 두번만 사용할 메서드를 매번 정의하는 것은 귀찮은 일이다.
- 한 번만 사용할 메서드는 따로 정의를 구현할 필요가 없다.
- static <T> Collection<T> filter(Collection<T> c, Predicate<T> p);
 ex) filterApples(inventory, (Apple a) → a.getWeight() > 150);
⇒ filter(inventory, (Apple a) → a.getWeight() > 150);
- 하지만 병렬성이라는 중요성 때문에 위와 같은 설계는 포기
- filter와 비슷한 동작을 수행하는 연산집합을 포함하는 새로운 스트림 API

## 1.3 스트림

- for-each와 같은 외부 반복(external iteration)에서
- 스트림 API를 이용하여 내부 반복(internal iteration)으로 변화

### 1.3.1 멀티스레딩은 어렵다.

- 자바 8은 스트림 API로 '컬렉션을 처리하면서 발생하는 모호함과 반복적인 코드 문제' 그리고 '멀티코어 활용 어려움'이라는 두 가지 문제를 모두 해결했다.
- 필터링(filtering), 추출(extracting), 그룹화(grouping)

## 1.4 디폴트 메서드

- 자바 8에서는 더 쉽게 변화할 수 있는 인터페이스를 만들 수 있도록 디폴트 메서드를 추가하였다.
- 구현 클래스에서 구현하지 않아도 되는 메서드를 인터페이스가 포함할 수 있는 기능!
- 메서드 바디는 클래스 구현이 아니라 인터페이스의 일부로 포함된다.

다이아몬드 상속 문제(diamond inheritance problems)?

## 1.5 함수형 프로그램에서 가져온 다른 유용한 아이디어

## 1.6 요약

- 언어 생태계의 모든 언어는 변화해서 살아남거나 그대로 머물면서 사라지게 되는 상황에 놓인다. 지금은 자바의 위치가 견고하지만 코볼과 같은 언어의 선례를 떠올리면 자바가 영원히 지배적인 위치를 유지할 수 있는 것은 아닐 수 있다.
- 자바 8은 프로그램을 더 효과적이고 간결하게 구현할 수 있는 새로운 개념과 기능을 제공한다.
- 기존의 자바 프로그래밍 기법으로는 멀티코어 프로세서를 온전히 활용하기 어렵다.
- 함수는 일급값이다. 메서드를 어떻게 함수형값으로 넘겨주는지, 익명 함수(람다)를 어떻게 구현하는지 기억하자.
- 자바 8의 스트림 개념 중 일부는 컬랙션에서 가져온 것이다. 스트림과 컬렉션을 적절하게 활용하면 스트림의 인수를 병렬로 처리할 수 있으며 더 가독성이 좋은 코드를 구현할 수 있다.
- 인터페이스에서 디폴트 메서드를 이용하면 메서드 바디를 제공할 수 있으므로 인터페이스를 구현하는 다른 클래스에서 해당 메서드를 구현하지 않아도 된다.
- 함수형 프로그래밍에서 null 처리 방법과 패턴 매칭 활용 등 흥미로운 기법을 발견할 수 있었다.