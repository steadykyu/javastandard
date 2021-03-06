# 1.컬렉션 프레임웍
+ 데이터 군(group)을 저장하는 클래스들을 표준화한 설계(단일화된 구조 - architecture)
+ 1.2 이후로 다양한 종류의 컬렉션 클래스가 추가되고, 모든 컬렉션 클래스를 표준화된 방식으로 다룰 수 있도록 체계화 되었다.

## 1.1 컬렉션 framework의 핵심 interface
+ 각 컬렉션을 다루는데 필요한 기능을 가진 3개의 인터페이를 정의하였다.
+ 그중 List, Set interface의 공통부분을 다시 뽑아 Collection interface를 정의하였고, 전혀 다른형태의 Map interface를 정의하였다.
> 핵심인터페이스들의 특징
```
List : 순서가 있는 데이터의 집합, 데이터의 중복을 허용한다.
Set : 순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다.
Map : key와 value의 쌍으로 이루어진 데이터의 집합
      순서는 유지되지 않으며, 키는 중복을 허용하지 않고, 값은 중복을 허용한다.
```
+ 기존의 인터페이스 Vector나 Hashtable 대신 표준화시켜놓은, 새로 추가된 ArrayList, Hashmap을 사용하자.

### Collection 인터페이스
+ List와 Set의 조상인 Collection interface에는 두 interface의 공통의 메서드들을 정의하고 있다.
+ 책에서 자주 사용하는 메서드를 확인해보자.

### List 인터페이스
+ **중복을 허용**하면서, **저장순서가 유지**되는 컬렉션을 구현하는데 사용된다.
+ 책에서 자주 사용하는 메서드를 확인해보자.

### Set 인터페이스
+ **중복을 허용하지 않고**, **저장순서가 유지되지 않는** 컬렉션 클래스를 구현하는데 사용
+ 책에서 자주 사용하는 메서드를 확인해보자.

### Map인터페이스
+ key와 value을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는데 사용
+ 책에서 자주 사용하는 메서드를 확인해보자.

### Map.Entry 인터페이스
+ Map 인터페이스의 내부인터페이스 이다.

## 1.2 ArrayList
+ List 인터페이스를 구현하였기 때문에, 데이터의 저장순서가 유지되고 중복을 허용한다는 특징이딨다.
+ Object배열을 이용해서 순차적으로 저장한다.
+ 더 이상 저장하는 공간이 없으면 큰 배열을 새로 생성해서 기존의 배열에 저장된 내용을 복사한 다음 새로운 배열로 저장한다.
+ 그러므로 너무 많은 복사가 일어나는 작업은 효율을 떨어트리므로, 초기에 적절한 용량의 인스턴스를 만드는 것이 좋다.
+ 책에서 자주 사용하는 메서드를 확인해보자.
+ size와 capacity
```
객체 배열과 함께 아래 두가지가 필드로 정의되어있다.
size : 실제 객체값이 존재하는 요소의 개수
capacity : 객체 배열의 크기(허용공간)
```
+ ArrayList의 for문과 remove(i)를 함께 쓴다면, List.size - 1부터 사용해야한다.
+ remove는 index객체를 지우면서, 객체값들이 이동하도록(당겨지도록) 만들기 때문에, 뒤에서 부터 제거해야한다.
+ api documentation java 를 검색하여 공식 API문서들을 볼수 있다.

## 1.3 LinkedList
### LinkedList의 단점
1. 크기를 변경할 수 없다.
2. 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.

### LinkedList
+ 불연속적으로 존재하는 데이터를 서로 연결한 형태로 구성하고 있다.
+ 추가, 제거시 참고 하고 있는 상태만 알맞게 변경해주면 되기때문에 실행속도가 빠르다.
+ 이동방향이 단방향인걸 보완하기 위해 더블 링크드 리스트가 존재한다.(책에서 그림참고)
+ LinkedList클래스는 이름과 달리 더블 LinkedList로 구현되어있다.
+ 메소드는 책으로 확인(ArrayList와 크게 다르지 않다. 몇개가 추가되있다.)

### 정리
+ ArrayLIst 
```
읽기 시간이 빠르나 순차적이지 않은 데이터를 추가/삭제 할때는 느리다.

비효율적인 메모리를 사용한다.(여유로운 용량을 위해)
```
+ LinkedList 
```
읽는 시간은 느리나, 데이터의 추가 삭제가 빠르다. 데이터가 많을 수록 접근성이 떨어진다.
```
+ 처음에 데이터 저장은 ArrayList를 사용한 다음, 작업은 LinkedList로 옮겨서 작업하면 좋은 효율을 가진다.
```
ArrayList al = new ArrayList(100000);
LinkedList ll = new LinkedList(al);
// 대부분의 컬렉션 클래스들은 서로 변환이 가능한 생성자를 제공한다.
```
## 1.4 Stack과 Queue
+ stack : LIFO(Last in First Out) 구조 - 마지막에 저장한 데이터를 가장 먼저 꺼낸다.
+ Queue : FIFO(First in First Out) 구조 - 처음에 저장한 데이터를 가장 먼저 꺼낸다.
+ 그러므로 stack 에는 ArrayList같은 배열기반이 어울리고
+ Queue에는 ArrayList를 사용하면 복사하는 비효율이 발생하므로, LinkedList로 구현한다.
+ stack은 클래스로 구현하여 제공하지만, 큐는 인터페이스로 정의되어 있어 구현체 클래스들을 사용해야한다.
### 스택과 큐의 활용
+ 스택의 활용 예 - 수식계산, 수식괄호검사, 웹브라우저의 뒤로/앞으로
+ 큐의 활용  예  - 최근사용문서목록, 인쇄작업대기목록, 버퍼
+ (코드참고)

### PriorityQueue
+ Queue 인터페이스의 구현체중 하나로 저장한 순서에 상관ㅇ벗이 우선순위가 높은것부터 꺼내는 특징이 있다.

### Deque
+ Queue를 조상으로하는 인터페이스이며, ArrayDeque와 LinkedList를 구현체로 가진다.
+ 한쪽 끝으로만 추가/삭제 가 가능한 Queue와 달리, 양쪽 끝에 추가/삭제가 가능하다.(책 그림 참고)

## 1.5 Iterator, ListIterator, Enumeration
+ 모두다 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스이다.
+ Enumeration는 구버전이라 이제 사용을 하지 않고, ListIterator는 Iterator보다 향상된 기능을 가지고 있다.
+ iterator()는 Collection 인터페이스에 정의 된 메서드이므로, 자손인 List와 Set에서도 사용이 가능하다.
+ 각 컬렉션들의 특징에 알맞게 작성되어있다.(순서, 중복상태)
+ Map 인터페이스 자체에는 사용할수 없으나, keySet()등으로 얻은 Set형태로는 iterator()를 호출 할 수 있다.
+ 코드참고
+ 책으로 메서드 꼭 참고하기

### ListIterator
+ Iterator에 양방향 조회기능을 추가했다.(이전방향으로 접근기능을 추가함)
+ 단 List를 구현한 경우에만 사용가능하다.
+ 책으로 메서드 꼭 참고하기

## 1.6 Arrays
+ 배열을 다루는데 유용한 메서드가 정의되어있다.
+ 같은 기능의 메서드가 매개변수로 배열의 타입만 다르게 오버로딩되있다.

### 배열의 복사
+ copyOf(), copyOfRange()
+ 배열전체, 배열의 일부를 복사해서 새로운 배열을 만들어 반환한다.

### 배열채우기
+ fill() : 배열의 모든 요소를 지정된 값으로 채운다.
+ setAll() : 배열을 채우는데 매개변수로 함수형 인터페이스를 가진다.

### 배열의 정렬과 검색
+ sort() : 배열을 정렬한다.
+ binarySearch() : 반드시 정렬상태인 배열에 사용하여야, 올바르게 값에 맞는 index를 반환해준다.
                 : 검색결과 일치하는 요소가 여러개 있다면, 어떤 index 가 나오는지는 알수 없다.
                 
                
### 배열의 비교와 출력
+ equals() : 모든 요소를 비교해서 같으면 true, 다르면 false를 반환한다.
+ deepEquals() : 다차원 배열의 비교가 가능하다.
+ toString() : 일차원 배열의 모든 요소를 문자열로 편하게 출력해준다.
+ deepToString() : 다차원 배열의 요소를 출력해준다.
+ asList(Object... a) : 배열을 List에 담아서 반환한다.
```
매개변수 타입이 가변인수이기 때문에, 배열뿐만아니라 저장할 요소만 나열해도 된다.
이 리스트는 크기를 변경할수 없다. 변경하려면
List list = new ArrayList(Arrays.asList(1,2,3,4,5));
```

### ParallelXXX(), spliterator(), stream()
+ parallel로 시작하는 이름의 메서드들은 보다 빠른 결과를 얻기위해 여러 쓰레드가 나누어 작업을 처리한다.
+ spliterator() : 여러 쓰레드가 처리할수 있게 하나의 작업을 여러작업으로 나누는 Spliterator를 반환한다.
+ stream() : 컬렉션을 스트림으로 변환한다.

## 1.7 Comparator와 Comparable
+ Arrays.sort()를 호출하면, 사실 Character클래스(char의 wrapper)의 Comparable의 구현에 의해 정렬되는 것이다.
+ Comparator와 Comparable는 모두 인터페이스 이며, Comparable을 구현한 클래스들은 기본적으로 오름차순으로 정렬되도록 구현하고 있다.
```
public interface Comparator{
      int compare(Object o1, Object o2); // 제한자가 default이므로 사용하려면, 같은 패키지내에서 구현을 따로 해주어야함.
}
=============================================
public interface Comparable{
      public int compareTo(Object o);
}
```
+ Comparable : 기본 정렬기준을 구현하는데 사용.
+ Comparator : 기본 정렬기준 외에 다른 기준으로 정렬하고자 할때 사용
  * 상수의 형태나 구현한 클래스속 compare() 메소드에서 return값을 수정해주면 정렬 기준을 바꿀 수있다.
+ 코드참고
```java
class Ch11_19ComparatorEx {
    public static void main(String[] args) {
        String[] strArr = {"cat", "Dog", "lion", "tiger"};

        Arrays.sort(strArr);                          // String의 Comparable구현에 의한 정렬이 되는 중
        System.out.println("strArr=" + Arrays.toString(strArr));

        Arrays.sort(strArr, String.CASE_INSENSITIVE_ORDER); // 대소문자 구분안함
        System.out.println("strArr=" + Arrays.toString(strArr));

        Arrays.sort(strArr, new Descending()); // 역순 정렬
        System.out.println("strArr=" + Arrays.toString(strArr));
    }
}

class Descending implements Comparator {
    public int compare(Object o1, Object o2){
        if( o1 instanceof Comparable && o2 instanceof Comparable) {
            Comparable c1 = (Comparable)o1;
            Comparable c2 = (Comparable)o2;
            return c1.compareTo(c2) * -1 ; // -1을 곱해서 기본 정렬방식의 역으로 변경한다.
            // 또는 c2.compareTo(c1)와 같이 순서를 바꿔도 된다.

        }
        return -1;
    }
}
```
+ 참고 블로그 : https://st-lab.tistory.com/243
## 1.8 HashSet
+ Set인터페이스를 구현한 가장 대표적인 컬렉션이며, HashSet은 중복된 요소를 저장하지 않는다.
+ 만약 저장순서를 유지하고자 한다면 LinkedHashset을 사용해야한다.
+ 생성자와 메서드는 책을 참고하자
+ 중복된 요소에는 서로 같은 객체이여야 한다.(ex String 자료형 "1" 과 Integer 자료형의 1 은 다른 객체로 간주한다.)

### HashSet의 add메서드
+ add 메서드를 통해 새로운 요소를 추가하기 전에, 기존의 저장된 요소와 같은 것인지 판별한다.
+ 추가하려는 **요소의** equals()와 hashCode()를 호출하기때문에, 목적에 맞게 오버라이딩 해야한다.
``` 
Person2 p1 = new Person2("David", 10);
Person2 p2 = new Person2("David", 10);
```
+ 오버라이드 해주지 않으면, p1과 p2는 다른 주소값을 가지고 있으므로, HashSet에서 중복으로 처리하지 않는다.

### hashCode()
#### 오버라이딩 되는 hashCode()의 조건
+ 두 객체에 대해 equal메서드를 호출한 결과가 true이면 두 객체의 해시코드는 반드시 같아야한다.
+ 두 객체의 해시코드가 같다고 해서 equals메서드의 호출결과가 반드시 true인 것은 아니다.
+ 만약 equal메서드가 false가 나왔을때,hashcode() 가 같을 수도 있으나, 해싱을 사용하는 컬렉션의 성능을 향상시키기 위해서는 다르게하는 것이 좋다.(추후 배울 예정)

## 1.9 TreeSet
+ 이진검색 트리의 자료구조 형태로 데이터를 저장하는 컬렉션 클래스이다.

### 특징
+ 여러 개의 노드를 가지며 루트, 부모, 자식 노드로 계속 확장해간다.
+ 모든 노드는 최대 두개의 자식 노드를 가질 수 있다.
+ 왼쪽 자식노드의 값은 부모보다 작고, 오른쪽 자식노드의 값은 부모노드의 값보다 커야한다.
+ 노드의 추가 삭제에 시간이 걸린다.(순차적으로 저장하지 않고 이진검색을 통해 비교후 저장하므로)
+ 검색(범위검색)과 정렬에 유리하다.( 이미 정렬된 상태로 값이 저장되어있음)
+ 중복된 값을 저장하지 못한다.
**** 
+ 문자열의 정렬순서는 코드값의크기가 작은순서에서 큰순서(공백,숫자,대문자,소문자) 순으로 정렬된다.
+ Comparator 로 정렬조건을 설정할 수 있다.
### 생성자,메서드
+ 책 참고

## 1.10 HashMap과 HashTable
+ HashTable 보다는 호환성을 위해 새로운 버전인 HashMap을 쓰자.

### HashMap
+ Map을 구현했으므로 key와 value를 묶어서 하나의 데이터(entry 클래스)로 저장한다는 특징을 가진다.
```
public class HashMap extends AbstractMap implements Map, Cloneable, Serializable{
      transient Entry[] table;
      ...
      static class Entry implements Map.Entry{
      final Object key;
      Object value;
      ...
      }
}
```
+ (Object, Object)의 형태로 저장하여 어떠한 객체도 저장할 수있다.
+ value안에 HashMap을 다시 저장하여 하나의 키에 복수의 데이터를 넣을 수도 있다.
```
키(key) : 컬렉션 내의 키중에서 유일해야한다.
값(value) : 키와 달리 데이터의 중복을 허용한다.
```
+ 생성자, 메서드는 책참고
+ Set entrySet() 메서드는 유의해서 봐보기.
+ 변수 처럼 같은 key에 여러번 값을 넣어주면, 마지막 value을 가진상태로 조재한다.

### 해싱과 해시함수
+ 어려우니 책의 그림을 꼭 참고하자.
+ 해싱 : 해시함수를 이용해서 데이터를 hashtable에 저장하고 검색하는 기법을 말한다.
+ HashSet, HashMap, Hashtable등이 해싱을 구현한 컬렉션 클래스로 Hashtable은 호환성때문에 쓰지 않는다.

### 해싱의 자료구조 구성
+ 해싱에서 사용하는 자료구조는 배열과 링크드 리스트의 조합으로 되어있다.
+ 저장된 키를 해시함수에 넣으면 배열의 한요소를 얻게되고, 다시 그곳에 연결 되어있는 링크드 리스트에 저장하게 된다.(책의 그림 참고)
+ 이전에서 배운것처럼 배열은 검색에 유리한 자료구조이고, 링크드 리스트는 검색에는 좋지않은 자료구조이다.
+ 그러므로 하나의 배열요소에 많은 링크드 리스트가 존재하는 것 보다는 여러 배열요소들이 존재하고 하나의 데이터(링크드 리스트)가 존재하는 것이 더 빠른 결과를 얻을 수 있다.
+ 이를 위해서는 HashMap의 크기를 적절하게 지정해주어야하고, 해시함수가 서로 다른 키들에 대해서 중복된 해시코드의 반환을 **최소화**해야한다.
+ 이 과정에서 해시함수의 알고리즘이 중요하게 되는데, 가장 중복되지 않은 해시코드 반환을 위해 Object클래스의 hashcode()는 객체의 주소를 이용하는 알고리즘을 사용한다.
+ 모든 객체에 대하여 hashcode() 결과가 유일한 값을 가지는 훌륭한 방법이다.
***
+ HashSet과 마찬가지로 HashMap도 중복의 판단에 있어, equals()가 true이고 hashcode() 반환 값이 같아야 같은 객체로 인정한다.(중복으로 판단)
+ 그러므로 HashMap에 넣을 새로운 클래스를 정의할때 equals()를 재정의오버라이딩 하여 true값이 나오게 되는 경우에는 hashcode()도 같은 결과값을 가지도록 만들어주어야한다.
+ 그래야 원하는 중복의 기준 설정을 할수 있을 것이다.
+ 참고: equals() 로 비교한 결과가 false 이고 해시코드가 같은경우는 (한 배열요소에 존재하는)같은 링크드 리스트에 저장된 서로 다른 두 데이터가 된다.


## 1.11 TreeMap
+ 이진검색트리의 형태로 key와 value의 쌍으로 이루어진 데이터를 저장한다.
+ 기본정렬 방식은 key를 기준으로 오름차순 정렬 형태이다.
+ 검색에 관한 대부분의 경우에는 HashMap이 TreeMap보다 더 뛰어나므로 HashMap을 쓰는게 좋다.
+ 다만 범위검색이나 정렬이 필요한 경우에는 TreeMap을 사용하자.

## 1.12 Properties
+ Hashtable은 (Object, Object)로 저장하는데 비해, Property는 (String, String) 형태로 저장하는 단순화된 컬렉션 클래스이다.
+ 주로 애플리케이션의 환경설정과 관련된 property를 저장하는데 사용되며, 데이터를 파일로부터 읽고 쓰는 편리한 기능을 제공한다.
+ list(), load(), store() 여러 메소드의 사용을 책과 코드를 통해 살펴보자.

## 1.13 Collections
+(s 붙여야함!)
+ 배열과 관련된 메서드를 제공하는 Arrays 처럼, Collections는 컬렉션과 관련된 메서드를 제공한다.
+ Arrays에 존재하는 메서드들을 포함하여 추가적인 여러 메서드를 제공한다.

### 컬렉션의 동기화
+ 이전버전의 Vector, Hashtable은 자체적으로 동기화 처리가 되어있었지만, 새로추가된 ArrayList와 HashMap은 필요한 경우에만 java.util.**Collections**클래스의 동기화 메서드를 이용하여 
처리하도록 변경하였다.
+ 멀티쓰레드 프로그래밍에서 하나의 객체를 여러 쓰레드가 동시에 접근 할 수 있는데, 데이터의 일관성을 유지하기 위해서 공유되는 객체에 동기화(synchronization)가 필요하다.
> 모든 컬렉션이 가능한데, 예시로 하나의 메서드만 살펴보자.
```
static List synchronizedList(List list)

List syncList = Collections.synchronizedList(new ArrayList(...));
```

### 변경 불가 컬렉션 만들기
+ 컬렉션의 저장된 데이터를 보호하기위해 읽기 전용으로 만들수 있다.
```
static List unmodifiableList(List list)
```

### 싱글톤 컬렉션 만들기
+ 단 하나의 객체만을 저장하는 컬렉션을 만들고 싶을 경우
```
static List singletonList(List list)
```

### 한 종류의 객체만 저장하는 컬렉션 만들기
+ 컬렉션은 모든 종류의 객체를 저장할수 있는데 이는 장점이면서 단점인 부분이다.
+ 한 종류의 객체를 저장하며, 컬렉션에 지정된 종류의 객체만 저장하도록 제한 싶을때는 아래의 메서드를 사용한다.
```
static List checkedList(List list, Class type)

List checklist = Collections.checkedList(list, String.class);
```
+ 사실 지네릭스를 사용하면 컬렉션에 저장할 요소타입을 쉽게 제한 할수 있다.
+ 지네릭스를 호환하지 않는 java버전에서 checked를 사용하자.

## 1.14 컬렉션 클래스 정리 & 요약
+ 책의 그림과 표를 확인하자.


