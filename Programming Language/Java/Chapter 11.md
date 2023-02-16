# 1. 컬렉션 프레임워크(Collections Framework)
- 컬렉션(여러 객체)을 다루기 위한 표준화된 프로그래밍 방식
- 컬렉션(여러 객체(데이터)를 모아 놓은 것) + 프레임워크(표준화, 정형화된 프로그래밍 방식)
- 컬렉션을 쉽고 편리하게 다룰 수 있는 다양한 클래스를 제공
- java.util 패키지에 포함
- 컬렉션 클래스(collection class)
    - 여러 객체를 저장할 수 있는 클래스
    - Vector, ArrayList, HashSet
    - 컬렉션 클래스는 저장 공간이 부족 시 스스로 공간을 확장

## 1-1. 컬렉션 프레임워크의 핵심 인터페이스
|List|Set|Map|
|------|------|------|
|순서가 있는 데이터의 집합|순서를 유지하지 않는 데이터의 집합|순서를 유지하지 않으며 키(key)와 값(value)의 쌍(pair)으로 이루어진 데이터의 집합|
|데이터의 중복 허용|데이터의 중복 허용 X|키는 중복 허용 X, 값은 중복 허용|
|구현 클래스 : ArrayList, LinkedList, Stack, Vector 등|구현 클래스 : HashSet, TreeSet 등|구현 클래스 : HashMap, TreeMap, Hashtable, Properties 등|

## 1-2. Collection 인터페이스의 메소드
|메소드|설명|
|------|------|
|boolean add(Object o)|지정된 객체(o)를 Collection에 추가|
|boolean addAll(Collection c)|지정된 Collection (c)의 객체들을 Collection에 추가|
|boolean contains(Object o)|지정된 객체(o)가 Collection에 포함 되어 있는지 확인|
|boolean containsAll(Collection c)|지정된 Collection (c)의 객체들이 Collection에 포함 되어 있는지 확인|
|boolean remove(Object o)|지정된 객체를 삭제|
|boolean removeAll(Collection c)|지정된 Collection에 포함된 객체들을 삭제|
|void clear()|Collection의 모든 객체를 삭제|
|boolean isEmpty()|Collection이 비어있는지 확인|
|int size()|Collection에 저장된 객체의 개수를 반환|

<br>

# 2. List 인터페이스
- 저장 순서가 유지되고 중복을 허용하는 컬렉션을 구현할 때 사용
- List 관련 메소드

|메소드|설명|
|------|------|
|void add(int index, Object element)|지정된 위치(index)에 객체(element) 또는 컬렉션에 포함된 객체들을 추가|
|boolean addAll(int index, Collection c)|위와 동일|
|Object get(int index)|지정된 위치(index)에 있는 객체를 반환|
|int indexOf(Object o)|지정된 객체의 위치(index)를 반환(List의 첫 번째 요소부터 순방향으로 탐색)|
|int lastIndexOf(Object o)|지정된 객체의 위치(index)를 반환(List의 마지막 요소부터 역방향으로 탐색)|
|Object remove(int index)|지정된 위치(index)에 있는 객체를 삭제하고 삭제된 객체를 반환|
|Object set(int index, Object element)|지정된 위치(index)에 객체(element)를 저장|
|void sort(Comparator c)|지정된 비교자(comparator)로 List를 정렬|
|List subList(int fromIndex, int toIndex)|지정된 범위(fromIndex부터 toIndex)에 있는 객체를 반환(일부를 추출하여 새로운 list 생성)|

## 2-1. ArrayList
```java
ArrayList<Integer> arrList = new ArrayList<Integer>();

// add() 메소드를 이용한 요소의 저장
arrList.add(40);
arrList.add(20);
arrList.add(30);
arrList.add(10);

// for 문과 get() 메소드를 이용한 요소의 출력
for (int i = 0; i < arrList.size(); i++) {
    System.out.print(arrList.get(i) + " ");
}

// remove() 메소드를 이용한 요소의 제거
arrList.remove(1);

// Enhanced for 문과 get() 메소드를 이용한 요소의 출력
for (int e : arrList) {
    System.out.print(e + " ");
}

// Collections.sort() 메소드를 이용한 요소의 정렬
Collections.sort(arrList);
 
// iterator() 메소드와 get() 메소드를 이용한 요소의 출력
Iterator<Integer> iter = arrList.iterator();
while (iter.hasNext()) {
    System.out.print(iter.next() + " ");
}

// set() 메소드를 이용한 요소의 변경
arrList.set(0, 20);
for (int e : arrList) {
    System.out.print(e + " ");
}

// size() 메소드를 이용한 요소의 총 개수
System.out.println("리스트의 크기 : " + arrList.size());
```
```
<!-- 출력 -->
40 20 30 10 

40 30 10

10 30 40 

20 30 40

리스트의 크기 : 3
```
- 내부적으로 배열을 이용하여 요소를 저장
- 배열을 이용하므로 인덱스를 통해 배열 요소에 빠르게 접근 가능
    - 그러나 배열은 크기를 변경할 수 없는 인스턴스이므로 크기를 늘리려면 새로운 배열을 생성하고 기존의 요소들을 옮기는 등 복잡한 과정을 수행
    - 크기를 조절하는 작업은 자동으로 수행되지만 요소의 추가 및 삭제 작업에 걸리는 시간이 매우 길어지는 단점이 존재
- 기존의 Vector를 개선한 것이므로 구현 원리와 기능적인 측면에서 동일
- 자체적으로 동기화 처리가 불가능(<-> Vector)
- List 인터페이스를 구현하므로 저장 순서가 유지되고 중복을 허용
- 데이터의 저장 공간으로 배열을 사용
    - Object 타입의 객체 배열을 사용하므로 모든 종류의 객체를 저장 가능

## 2-2. LinkedList
```java
LinkedList<String> lnkList = new LinkedList<String>();

// add() 메소드를 이용한 요소의 저장
lnkList.add("넷");
lnkList.add("둘");
lnkList.add("셋");
lnkList.add("하나");

// for 문과 get() 메소드를 이용한 요소의 출력
for (int i = 0; i < lnkList.size(); i++) {
    System.out.print(lnkList.get(i) + " ");
}

// remove() 메소드를 이용한 요소의 제거
lnkList.remove(1);

// Enhanced for 문과 get() 메소드를 이용한 요소의 출력
for (String e : lnkList) {
    System.out.print(e + " ");
}

// set() 메소드를 이용한 요소의 변경
lnkList.set(2, "둘");

for (String e : lnkList) {
    System.out.print(e + " ");
}

// size() 메소드를 이용한 요소의 총 개수
System.out.println("리스트의 크기 : " + lnkList.size());
```
```
<!-- 출력 -->
넷 둘 셋 하나

넷 셋 하나

넷 셋 둘

리스트의 크기 : 3
```
- ArrayList 클래스가 배열을 이용하여 요소를 저장함으로써 발생하는 단점을 극복하기 위해 고안
- 내부적으로 연결 리스트(linked list)를 이용하여 요소를 저장
- 배열은 저장된 요소가 순차적으로 저장
    - 하지만 연결 리스트는 저장된 요소가 비순차적으로 분포되며 이러한 요소 사이를 링크(link)로 연결하여 구성
- 단일 연결 리스트(singly linked list)
    - 다음 요소를 가리키는 참조만을 가지는 단방향 연결 리스트
    - 요소의 저장과 삭제 작업이 다음 요소를 가리키는 참조만 변경하면 되므로 처리 속도가 우수
    - 현재 요소에서 이전 요소로 접근하는 것이 복잡
- 이중 연결 리스트(Doubly Linked List)
    - 단일 연결 리스트의 접근성을 향상한 리스트
    - 1개의 추가 참조변수를 통해 다음 요소에 대한 참조뿐만 아니라 이전 요소에 대한 참조 가능
    - Java의 LinkedList는 이중 연결 리스트로 구현

## 2-3. ArrayList vs LinkedList
- 순차적인 데이터 추가/삭제 : ArrayList 승
- 비순차적인 데이터 추가/삭제 : LinkedList 승
- 접근 시간(access time) : ArrayList 승

<br>

# 3. Set 인터페이스
-  저장 순서가 유지되지 않고 중복을 허용하지 않는 컬렉션을 구현할 때 사용
- Set 인터페이스에 정의된 메소드는 Collection 인터페이스의 메소드와 동일
- 집합과 관련된 메소드

|메소드|설명|
|------|------|
|boolean addAll(Collection c)|지정된 Collection(c)의 객체를 Collection에 추가(합집합)|
|boolean containsAll(Collection c)|지정된 Collection(c)의 객체가 Collection에 포함되어 있는지 확인(부분 집합)|
|boolean removeAll(Collection c)|지정된 Collection(c)에 포함된 객체 삭제(차집합)|
|boolean retainAll(Collection c)|지정된 Collection(c)에 포함된 객체만 남기고 나머지는 Collection에서 삭제(교집합)|

## 3-1. HashSet
```java
HashSet<String> hs01 = new HashSet<String>();
HashSet<String> hs02 = new HashSet<String>();

// add() 메소드를 이용한 요소의 저장
hs01.add("홍길동");
hs01.add("이순신");
System.out.println(hs01.add("임꺽정"));
System.out.println(hs01.add("임꺽정")); // 중복된 요소의 저장

// Enhanced for 문과 get() 메소드를 이용한 요소의 출력
for (String e : hs01) {
    System.out.print(e + " ");
}

// add() 메소드를 이용한 요소의 저장
hs02.add("임꺽정");
hs02.add("홍길동");
hs02.add("이순신");

// iterator() 메소드를 이용한 요소의 출력
Iterator<String> iter02 = hs02.iterator();
while (iter02.hasNext()) {
    System.out.print(iter02.next() + " ");
}

// size() 메소드를 이용한 요소의 총 개수
System.out.println("집합의 크기 : " + hs02.size());
```
- 저장 순서를 유지하지 않으며 중복을 허용 X
- 해시 알고리즘(Hash Algorithm)을 사용하므로 검색 속도가 매우 우수
- 내부적으로 HashMap 인스턴스를 이용하여 요소를 저장
- Set 인터페이스를 구현하므로 요소를 순서에 상관없이 저장하고 중복된 값은 저장 X
    - 만약 요소의 저장 순서를 유지하려면 LinkedHashSet 클래스를 사용
- Set은 정렬이 불가능하므로 Set의 모든 요소를 List에 담아 Collections.sort()로 정렬

## 3-2. TreeSet
```java
TreeSet<Integer> ts = new TreeSet<Integer>();

// add() 메소드를 이용한 요소의 저장
ts.add(30);
ts.add(40);
ts.add(20);
ts.add(10);

// Enhanced for 문과 get() 메소드를 이용한 요소의 출력
for (int e : ts) {
    System.out.print(e + " ");
}

// remove() 메소드를 이용한 요소의 제거
ts.remove(40);

// iterator() 메소드를 이용한 요소의 출력
Iterator<Integer> iter = ts.iterator();
while (iter.hasNext()) {
    System.out.print(iter.next() + " ");
}

// size() 메소드를 이용한 요소의 총 개수
System.out.println("이진 검색 트리의 크기 : " + ts.size());

// subSet() 메소드를 이용한 부분 집합의 출력
System.out.println(ts.subSet(10, 20));
System.out.println(ts.subSet(10, true, 20, true));
```
- 이진 탐색 트리라는 자료 구조의 형태로 데이터를 저장하는 컬렉션 클래스
- 내부적으로 TreeMap을 이용하여 생성하며 범위 탐색과 정렬 우수
- 데이터가 정렬된 상태로 저장되는 이진 검색 트리(binary search tree)의 형태로 요소를 저장
- 이진 검색 트리는 데이터를 추가하거나 제거하는 등의 기본 동작 시간이 매우 우수
- NavigableSet 인터페이스를 기존의 이진 검색 트리의 성능을 향상시킨 레드-블랙 트리(Red-Black tree)로 구현
- Set 인터페이스를 구현하므로 요소를 순서에 상관없이 저장하고 중복된 값은 저장 X
- 데이터 추가, 삭제에 소요되는 시간이 HashSet보다 많이 소요

<br>

# 4. Map 인터페이스
- 저장 순서가 유지되지 않고 키는 중복을 허용하지 않으며 값은 중복을 허용하는 컬렉션을 구현할 때 사용
    - 저장 순서의 유지가 필요한 경우, Map 인터페이스의 구현체 중 LinkedHashMap 사용
- Entry : 키와 값의 한 쌍
    - Map에서 값(value)은 중복을 허용하므로 반환 타입이 Collection이며 키(key)는 중복을 허용하지 않으므로 반환 타입이 Set
- Map 인터페이스에 정의된 메소드

|메소드|설명|
|------|------|
|Object put(Object key, Object value)|key 객체에 value 객체를 연결(mapping)하여 Map에 저장|
|void putAll(Map t)|지정된 Map의 모든 key-value 쌍을 추가|
|boolean containsKey(Object key)|지정된 key 객체와 일치하는 Map의 key 객체가 있는지 확인|
|boolean containsValue(Object value)|지정된 value 객체와 일치하는 Map의 value 객체가 있는지 확인|
|Object get(Object key)|지정된 key 객체와 일치하는 value 객체를 찾아서 반환|
|Object remove(Object key)|지정된 key 객체와 일치하는 key-value 객체를 삭제|
|Set entrySet()|Map에 저장되어 있는 key-value 쌍을 Map.Entry 타입의 객체로 저장한 Set으로 반환|
|Set keySet()|Map에 저장된 모든 key 객체를 반환|
|Collection values()|Map에 저장된 모든 value 객체를 반환|

## 4-1. HashMap
- Map 인터페이스를 구현하였으며 데이터를 키와 값의 쌍(entry)으로 저장
- HashMap(동기화 X)은 HashTable(동기화 O)의 새로운 버전
- 해싱(hashing)을 사용하므로 많은 양의 데이터를 검색할 때 우수
- Entry라는 내부 클래스를 정의하고 다시 Entry 타입의 배열을 선언
- 저장 순서를 유지하려면 LinkedHashMap클래스를 사용

## 4-2. TreeMap
- 이진 탐색 트리의 구조로 키와 값의 쌍으로 이루어진 데이터를 저장
- HashMap보다 데이터 추가, 삭제에 시간이 더 소요
- 다수의 데이터에서 개별적인 검색은 TreeMap보다 HashMap이 더 우수
- Map이 필요할 때는 주로 HashMap을 사용하며, 범위 검색이나 정렬이 필요한 경우에 TreeMap을 사용

<br>

# 5. 스택과 큐(Stack & Queue)
## 5-1. 스택(Stack)
```JAVA
// Stack 생성
Stack<Integer> st = new Stack<Integer>();
// Stack 생성 2 - Deque 인터페이스를 구현한 ArrayDeque 클래스 사용
Deque<Integer> st = new ArrayDeque<Integer>();

// push() 메소드를 이용한 요소의 저장
st.push(4);
st.push(2);
st.push(3);
st.push(1);

// peek() 메소드를 이용한 요소의 반환
System.out.println(st.peek());
System.out.println(st);

// pop() 메소드를 이용한 요소의 반환 및 제거
System.out.println(st.pop());
System.out.println(st);
 
// search() 메소드를 이용한 요소의 위치 검색
System.out.println(st.search(4));
System.out.println(st.search(3));
```
|메소드|설명|
|------|------|
|boolean empty()|해당 스택이 비어 있으면 true, 비어 있지 않으면 false를 반환|
|E peek()|해당 스택의 제일 상단에 있는(제일 마지막으로 저장된) 요소를 반환|
|E pop()|해당 스택의 제일 상단에 있는(제일 마지막으로 저장된) 요소를 반환하고, 해당 요소를 스택에서 제거|
|E push(E item)|해당 스택의 제일 상단에 전달된 요소를 삽입|
|int search(Object o)|해당 스택에서 전달된 객체가 존재하는 위치의 인덱스를 반환|

이때 인덱스는 제일 상단에 있는(제일 마지막으로 저장된) 요소의 위치부터 0이 아닌 1부터 시작함.
- List 컬렉션 클래스의 Vector 클래스를 상속받아 전형적인 스택 메모리 구조의 클래스를 제공
- 선형 메모리 공간에 데이터를 저장하면서 후입선출(LIFO)의 시멘틱을 따르는 자료 구조
    - LIFO : 가장 나중에 저장된(push) 데이터가 가장 먼저 인출(pop)되는 구조

## 5-2. 큐(Queue)
```java
// Queue 생성
LinkedList<String> qu = new LinkedList<String>(); 
// Queue 생성 2 - Deque 인터페이스를 구현한 ArrayDeque 클래스 사용
Deque<String> qu = new ArrayDeque<String>();

// add() 메소드를 이용한 요소의 저장
qu.add("넷");
qu.add("둘");
qu.add("셋");
qu.add("하나");

// peek() 메소드를 이용한 요소의 반환
System.out.println(qu.peek());
System.out.println(qu);

// poll() 메소드를 이용한 요소의 반환 및 제거
System.out.println(qu.poll());
System.out.println(qu);

// remove() 메소드를 이용한 요소의 제거
qu.remove("하나");
System.out.println(qu);
```
|메소드|설명|
|------|------|
|boolean add(E e)|해당 큐의 맨 뒤에 전달된 요소를 삽입 / 만약 삽입에 성공하면 true를 반환하고, 큐에 여유 공간이 없어 삽입에 실패하면 IllegalStateException을 발생|
|E element()|해당 큐의 맨 앞에 있는(제일 먼저 저장된) 요소를 반환|
|boolean offer(E e)|해당 큐의 맨 뒤에 전달된 요소를 삽입|
|E peek()|해당 큐의 맨 앞에 있는(제일 먼저 저장된) 요소를 반환 / 만약 큐가 비어있으면 null을 반환|
|E poll()|해당 큐의 맨 앞에 있는(제일 먼저 저장된) 요소를 반환하고, 해당 요소를 큐에서 제거 / 만약 큐가 비어있으면 null을 반환|
|E remove()|해당 큐의 맨 앞에 있는(제일 먼저 저장된) 요소를 제거|
- 클래스로 구현된 스택과는 달리 자바에서 큐 메모리 구조는 별도의 인터페이스 형태로 제공
- Queue 인터페이스를 상속받는 하위 인터페이스
    - Deque<E>
    - BlockingDeque<E>
    - BlockingQueue<E>
    - TransferQueue<E>
- Deque 인터페이스를 구현한 LinkedList 클래스가 큐 메모리 구조를 구현할 때 많이 사용
- 큐 메모리 구조는 선형 메모리 공간에 데이터를 저장하면서 선입선출(FIFO)의 시멘틱을 따르는 자료 구조
    - FIFO : 가장 먼저 저장된(push) 데이터가 가장 먼저 인출(pop)되는 구조
