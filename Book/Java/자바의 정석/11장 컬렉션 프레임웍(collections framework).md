# CH11. 컬렉션 프레임웍 
## 컬렉션 프레임웍(Collections Framework)  
컬렉션 프레임웍이란, `데이터 군을 저장하는 클래스들을 표준화한 설계`를 뜻한다. 
컬렉션(Collection)은 다수의 데이터, 즉 데이터 그룹을, 프레임웍은 표준화된 프로그래밍 방식을 의미한다.  

JDK1.2부터 컬렉션 프레임웍이 등장하면서 다양한 종류의 컬렉션 클래스가 추가되고 모든 컬렉션 클래스를 표준화된 방식으로 다룰 수 있도록 체계화되었다.  
컬렉션 프레임웍은 컬렉션(다수의 데이터)을 다루는 데 필요한 다양하고 풍부한 클래스들을 제공하기 때문에 프로그래머의 짐을 상당히 덜어 주고 있으며, 또한 인터페이스와 다형성을 이용한 객체지향적 설계를 통해 표준화되어 있기 떄문에 사용법을 익히기에도 편리하고 재사용성이 높은 코드를 작성할 수 있다는 장점이 있다.   
<br>

## 컬렉션 프레임웍의 핵심 인터페이스  
컬렉션 프레임웍에서는 컬렉션데이터 그룹을 크게 3가지 타입이 존재한다고 인식하고 각 컬렉션을 다루는데 필요한 기능을 가진 3개의 인터페이스 `List`, `Set`, `Map`를 정의했다. 
인터페이스 List와 Set을 구현한 컬렉션 클래스들은 서로 많은 공통부분이 있어서, 공통된 부분을 다시 뽑아 Collection 인터페이스를 정의할 수 있었지만 Map 인터페이스는 이들과는 전혀 다른 형태로 컬렉션을 다루기 때문에 같은 상속계층도에 포함되지 못했다.  

![image](https://user-images.githubusercontent.com/54930365/215392479-afebe185-f028-48da-9eb3-8b24bf373042.png)  
사진출처: https://keepmind.net/java-collection-framework-1/  

|인터페이스| 특징                                                                                                                        |
|:---:|:--------------------------------------------------------------------------------------------------------------------------|
|**List**| 순서가 있는 데이터의 집합. 데이터의 중복을 허용한다.<br>구현 클래스: ArrayList, LinkedList, Stack, Vector 등                                          |
|**Set**| 순서를 유지하지 않는 데이터의 집합. 데이터의 중복을 허용하지 않는다.<br>구현 클래스: HashSet, TreeSet 등                                                     |
|**Map**| 키와 값의 쌍으로 이루어진 데이터의 집합.<br>순서는 유지되지 않으며, 키는 중복을 허용하지 않고 값은 중복을 허용한다.<br>구현 클래스: HashMap, TreeMap, Hashtable, Properties 등 |

컬렉션의 프레임웍의 모든 컬렉션들은 List, Set, Map 중 하나를 구현하고 있으며, 구현한 인터페이스의 이름이 클래스의 이름에 포함되어있어서 이름만으로도 클래스의 특징을 쉽게 알 수 있다.  
그러나 Vector, Stack, Hashtable, Properties와 같은 클래스들은 컬렉션 프레임웍이 만들어지기 이전부터 존재하던 것이기 때문에 컬렉션 프레임웍의 명명법을 따르지 않는다.  
Vector나 HashTable과 같은 기존의 컬렉션 클래스들은 호환을 위해, 설계를 변경해서 남겨두었지만 가능하면 사용하지 않는 것이 좋다. 그 대신 새로 추가된 ArrayList와 HashMap을 사용하자.  

![image](https://user-images.githubusercontent.com/54930365/215382400-be969d4a-6c37-496a-8fc5-b68fba3195f9.png)
사진 출처: https://velog.io/@da__hey/Java-Java-Collections-List-Set-Map-%EA%B0%9C%EB%85%901  
<br>

### Collection 인터페이스  
List와 Set의 조상인 Collection 인터페이스에는 다음과 같은 메서드들이 정의되어 있다.  
- boolean add(Object o): 지정된 객체(o) 또는 Collection(c)의 객체들을 Collection에 추가한다.
- boolean addAll(Collection c): 지정된 객체(o) 또는 Collection(c)의 객체들을 Collection에 추가한다.
- void clear(): Collection의 모든 객체를 삭제한다.
- boolean contains(Object o): 지정된 객체(o) 또는 Collection의 객페들이 Collection에 포함되어 있는지 확인한다.
- boolean containAll(Collection c): 동일한 collection인지 비교한다.
- int hashCode(): Collection의 hash code를 반환한다.
- boolean isEmpty(): Collection이 비어있는지 확인한다.
- Iterator iterator(): Collection의 iterator를 얻어서 반환한다.
- boolean remove(Object o): 지정된 객체를 삭제한다.
- boolean removeAll(Collection c): 지정된 Collection에 포함된 객체들을 삭제한다.
- boolean retainAll(Collection c): 지정된 Collection에 포함된 객체만을 남기고 다른 객체들은 Collection에서 삭제한다. 이 작업으로 인해 Collection의 변화가 있으면 true를 그렇지 않으면 false를 반환한다.
- int size(): Collection에 저장된 객체의 개수를 반환한다.
- Object[] toArray(): Collection에 저장된 개수를 반환한다.
- Object[] toArray(Object[] a): 지정된 배열에 Collection의 객체를 저장해서 반환한다.  

이 외에도 JDK1.8부터 추가된 '람다(Lambda)와 스트림(Stream)'에 관련된 메서드들이 더 있다. 
이 메서드들은 '14장 람다와 스트림'에서 설명할 것이다.  

### List 인터페이스
List 인터페이스는 **중복을 허용하면서 저장순서가 유지되는 컬렉션**을 구현하는데 사용된다.  
![image](https://user-images.githubusercontent.com/54930365/215392586-fb3341ad-2781-4b71-8763-0f7482d694aa.png)

사진출처: https://keepmind.net/java-collection-framework-1/

List 인터페이스에 정의된 메서드는 다음과 같다. Collection 인터페이스로부터 상속받은 것들은 제외하였다.  
- void add(int index, Object element): 지정된 위치(index)에 객체(element) 또는 컬렉션에 포함된 객체들을 추가한다.
- boolean addAll(int index, Collection c): 지정된 위치(index)에 객체(element) 또는 컬렉션에 포함된 객체들을 추가한다.
- Object get(int index): 지정된 위치(index)에 있는 객체를 반환한다.
- int indexOf(Object o): 지정된 객체의 위치(index)를 반환한다.<br>(List의 첫 번째 요소부터 순방향으로 찾는다.)
- int lastIndexOf(Object o): 지정된 객체의 위치(index)를 반환한다.<br>(List의 마지막 요소부터 역방향으로 찾는다.)
- ListIterator listIterator(): List의 객체에 접근할 수 있는 ListIterator를 반환한다.
- ListIterator listIterator(int index): List의 객체에 접근할 수 있는 ListIterator를 반환한다.
- Object remove(int index): 지정된 위치(index)에 있는 객체를 삭제하고 삭제된 객체를 반환한다.
- Object set(int index, Object element): 지정된 위치(index)에 객체(element)를 저장한다.
- void sort(Comparator c): 지정된 비교자(comparator)로 List를 정렬한다.
- List subList(int fromIndex, int toIndex): 지정된 범위(fromIndex부터 toIndex)에 있는 객체를 반환한다.  

### Set 인터페이스  
Set 인터페이스는 **중복을 허용하지 않고 저장순서가 유지되지 않는 컬렉션** 클래스를 구현하는데 사용된다. Set 인터페이스를 구현한 클래스로는 HashSet, TreeSet 등이 있다.  
![image](https://user-images.githubusercontent.com/54930365/215392410-39f3d9dc-14de-4f44-82fb-5705ac766fe0.png)

사진출처: https://keepmind.net/java-collection-framework-3/  

### Map 인터페이스   
Map 인터페이스는 **키(key)와 값(value)을 하나의 쌍으로 묶어서 저장하는 컬렉션** 클래스를 구현하는 데 사용된다. 
**키는 중복될 수 없지만 값은 중복을 허용한다.** 
기존에 저장된 데이터와 중복된 키와 값을 저장하면 기존의 값은 없어지고 마지막에 저장된 값이 남게 된다. 
Map 인터페이스를 구현한 클래스로는 Hashtable, HashMap, LinkedHashMap, SortedMap, TreeMap 등이 있다.  

![image](https://user-images.githubusercontent.com/54930365/215392197-0771d7fc-3acd-443e-aad4-6cd296b41c6f.png)  
사진출처: https://keepmind.net/java-collection-framework-4/  

Map 인터페이스에 정의된 메서드는 다음과 같다. Map은 List, Set 인터페이스와 달리 Collection을 상속하지 않는다.  
- void clear(): Map의 모든 객체를 삭제한다. 
- boolean containKey(Object key): 지정된 key 객체와 일치하는 Map의 key객체가 있는지 확인한다.
- boolean containValue(Object value): 지정된 value 객체와 일치하는 Map의 value객체가 있는지 확인한다.
- Set entrySet(): Map에 저장되어 있는 key-value 쌍을 MapEntry 타입의 객체로 저장한 Set으로 반환한다.  
- int hashCode(): 해시코드를 반환한다.
- boolean isEmpty(): Map이 비어있는지 확인한다. 
- Set keySet(): Map에 저장된 모든 key 객체를 반환한다.
- Object put(Object key, Object value): Map에 value 객체를 key 객체에 연결(mapping)하여 저장한다.  
- void putAll(Map t): 지정된 Map의 모든 key-value 쌍을 추가한다.  
- Object remove(Object key): 지정한 key 객체와 일치하는 key-value쌍의 개수를 반환한다.
- int size(): Map에 저장된 key-value쌍의 개수를 반환한다.
- Collection values(): Map에 저장된 모든 value 객체를 반환한다.  

values()에서는 반환타입이 Collection이고, keySet()에서는 반환타입이 Set인 것에 주목하자. 
Map 인터페이스는 값은 중복을 허용하기 때문에 Collection 타입으로 반환하고, 키는 중복을 허용하지 않기 때문에 Set 타입으로 반환한다.  

### Map.Entry 인터페이스  
Map.Entry 인터페이스는 Map 인터페이스의 내부 인터페이스이다. 
내부 클래스와 같이 인터페이스도 인터페이스 안에 인터페이스를 정의하는 내부 인터페이스를 정의하는 것이 가능하다.  
Map에 저장되는 key-value 쌍을 다루기 위해 내부적으로 Entry 인터페이스를 정의해 놓았다.  
Map.Enty 인터페이스에 정의된 인터페이스는 다음과 같다. (JDK8.0부터 추가된 메서드는 생략)  

- boolean equals(Object o): 동일한 Entry인지 비교한다.
- Object getKey(): Entry의 key 객체를 반환한다.
- Object getValue(): Entry의 value 객체를 반환한다.
- int hashCode(): Entry의 해시코드를 반환한다.
- Object setValue(Object value): Entry의 value개게를 지정된 객체로 바꾼다.

<br>
<br>

## 컬렉션 프레임웍의 구성요소들
### ArrayList
ArrayList는 List 인터체이스를 구현하기 때문에 **데이터의 저장순서가 유지되고 중복을 허용한다**는 특징을 갖는다.   
ArrayList는 기존의 Vector를 개선한 것으로 Vector와 구현원리와 기능적인 측면에서 동일하다고 할 수 있다. 가능하면 Vector보다는 ArrayList를 사용하는 것이 좋다.  
ArrayList는 Object 배열을 이용해서 데이터를 순차적으로 저장한다. **Object 배열에 객체가 순서대로 저장되며, 배열에 더 이상 저장할 공간이 없으면 보다 큰 새로운 배열을 생성해서 기존의 배열에 저장된 내용을 새로운 배열로 복사한 다음에 저장된다.**   
```java
public class ArrayList extends AbstractList implements List, RandomAccess, Cloneable, java.io.Serializable{
    ...
    transient Object[] elementData;     // Object 배열
    ...
}
```
<br>

ArrayList의 생성자와 메서드는 다음과 같다.  

- ArrayList(): 크기가 10인 ArrayList를 생성
- ArrayList(Collection c): 주어진 컬렉션이 저장된 ArrayList를 생성
- ArrayList(int initialCapacity): 지정된 초기용량을 갖는 ArrayList를 생성
- boolean add(Object o): ArrayList의 마지막에 객체를 추가. 성공하면 true.
- void add(int index, Object element): 지정된 위치에 객체를 저장
- boolean addAll(Collection c): 주어진 컬렉션의 모든 객체를 저장한다.
- boolean addAll(int index, Collection c): 지정된 위치부터 주어진 컬렉션의 모든 객체를 저장한다.
- void clear(): ArrayList를 완전히 비운다.
- Object clone(): ArrayList를 복제한다.
- boolean contains(Object o): 지정된 객체(o)가 ArrayList에 포함되어 있는지 확인
- void ensureCapacity(int minCapacity): ArrayList의 용량이 최소한 minCapacity가 되도록 한다.
- Object get(int index): 지정된 위치(index)에 저장된 객체를 반환한다.
- int indexOf(Object o): 지정된 객테가 저장된 위치를 찾아서 반환한다.
- boolean isEmpty(): ArrayList가 비어있는지 확인한다.
- Iterator iterator(): ArrayList의 Iterator 객체를 반환한다.
- int lastIndexOf(Object o): 객체가 저장된 위치를 끝으로부터 역방향으로 검색해서 반환한다.
- ListIterator listIterator(): ArrayList의 ListIterator를 반환한다.
- ListIterator listIterator(int inex): ArrayList의 지정된 위치부터 시작하는 ListIterator를 반환한다.
- Object remove(int index): 지정된 위치(index)에 있는 객체를 제거한다.
- boolean remove(Object o): 지정된 객체를 제거한다. 성공하면 true, 실패하면 false
- boolean removeAll(Collection c): 지정된 컬렉션에 저장된 것과 동일한 객체들을 ArrayList에서 제거한다.
- boolean retainAll(Collection c): ArrayList에 저장된 객체 중에서 주어진 컬렉션과 공통된 것들만을 남기고 나머지는 삭제한다.
- Object set(int index, Object element): 주어진 객체(element)를 지정된 위치(index)에 저장한다.
- int size(): ArrayList에 저장된 객체의 개수를 반환한다.
- void sort(Comparator c): 지정된 정렬기준(c)으로 ArrayList를 정렬한다.
- List subList(int fromIndex, int toIndex): fromIndex부터 toIndex 사이에 저장된 객체를 반환한다.
- Object[] toArray(): ArrayList에 저장된 모든 객체들을 객체 배열로 반환한다.
- Object[] toArray(Object[] a): ArrayList에 저장된 모든 객체들을 객체배열 a에 담아 반환한다.
- void trimToSize(): 용량을 크기에 맞게 줄인다. (빈 공간을 없앤다.)

ArrayList나 Vector 같이 배열을 이용한 자료구조는 데이터를 읽어오고 저장하는 데는 효율이 좋지만, 용량을 변경해야 할 때는 새로운 배열을 생성한 후 기존의 배열로부터 새로 생성된 배열로 데이터를 복사해야하기 때문에 상당히 효율이 떨어진다는 단점을 가지고 있다.  
그래서 처음에 인스턴스를 생성할 때, 저장할 데이터의 개수를 잘 고려하여 충분한 용량의 인스턴스를 생성하는 것이 좋다.  

remove 메서드의 실제 코드에서 확인했듯이, **배열에 객체를 순차적으로 저장할 때와 객체를 마지막에 저장된 것부터 삭제하면** 
**System.arraycopy()를 호출하지 않기 때문에 작업시간이 짧지만, 배열의 중간에 위치한 객체를 추가하거나 삭제하는 경우 System.arraycopy()를 호출해서 다른 데이터의 위치를 이동시켜 줘야 하기 때문에 다루는 데이터의 개수가 많을수록 작업시간이 오래 걸린다.**  

<br><br>

### LinkedList  
배열은 가장 기본적인 형태의 자료구조로 구조가 간단하며 사용하기 쉽고 데이터를 읽어오는데 걸리는 시간(접슨 시간)이 가장 빠르다는 장점을 가지고 있지만 다음과 같은 단점도 가지고 있다.  
```text
1. 크기를 변경할 수 없다.
- 크기를 변경할 수 없으므로 새로운 배열을 생성해서 데이터를 복사해야한다.
- 실행속도를 향상시키기 위해서는 충분히 큰 크기의 배열을 생성해야 하므로 메모리가 낭비된다.

2. 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.
- 차례대로 데이터를 추가하고 마지막에서부터 데이터를 사게하는 것은 빠르지만,
- 배열의 중간에 데이터를 추가하려면 빈자리를 만들기 위해 다른 데이터들을 복사해서 이동해야 한다.
```

이러한 배열의 단점을 보완하기 위해서 링크드 리스트라는 자료구조가 고안되었다. 
배열은 모든 데이터가 연속적으로 존재하지만 링크드 리스트는 불연속적으로 존재하는 데이터를 서로 연결한 형태로 구성되어 있다.  

링크드 리스트의 각 요소(node)들은 자신과 연결된 다음 요소에 대한 참조(주소값)와 데이터로 구성되어 있다.  
```java
class Node {
    Node    next;   // 다음 요소의 주소를 저장
    Object  obj;    // 데이터를 저장
}
```
  
<br>
링크드 리스트에서의 데이터 삭제는 간단하다. 
삭제하고자 하는 요소의 이전요소가 삭제하고자 하는 요소의 다음 요소를 참조하도록 변경하기만 하면 된다. 
단 하나의 참조만 변경하면 삭제가 이뤄지는 것이다. 배열처럼 데이터를 이동하기 위해 복사하는 과정이 없기 때문에 처리속도가 매우 빠르다.  

새로운 데이터를 추가할 때는 새로운 요소를 생성한 다음 추가하고자 하는 위치의 이전 요소의 참조를 새로운 요소에 대한 참조로 변경해주고, 새로운 요소가 그 다음 요소를 참조하도록 변경하기만 하면 되므로 처리속도가 매우 빠르다.  

링크드 리스트는 이동방향이 단방향이기 때문에 다음 요소에 대한 접근은 쉽지만 이전요소에 대한 접근은 어렴다. 
이 점을 보완한 것이 `더블 링크드 리스트(이중 연결리스트, doubly linked list)`이다.  
더블 링크드 리스트는 단순히 링크드 리스트에 참조변수를 하나 더 추가하여 다음 요소에 대한 참조뿐 아니라 이전 요소에 대한 참조가 가능하도록 했을 뿐, 그 외에는 링크드 리스트와 같다. 
더블 링크드 리스트는 링크드 리스트보다 각 요소에 대한 접근과 이동이 쉽기 때문에 링크드 리스트보다 더 많이 사용된다.  
```java
class Node {
    Node    next;       // 다음 요소의 주소를 저장
    Node    previous;   // 이전 요소의 주소를 저장
    Object  obj;        // 데이터를 저장
}
```

<br>
더블 링크드 리스트의 접근성을 보다 향상시킨 것이 `더블 써큘러 링크드 리스트(이중 원형 연결리스트, doubly circular linked list)`인데, 
단순히 더블 링크드 리스트의 첫 번째 요소와 마지막 요소를 서로 연결시킨 것이다. 
이렇게 하면, 마지막 요소의 다음 요소가 첫 번째 요소가 되고, 첫 번째 요소의 이전 요소가 마지막 요소가 된다.  

실제로 LinkedList 클래스는 이름과 달리 `더블 링크드 리스트`로 구현되어 있는데, 이는 링크드 리스트의 단점이 낮은 접근성을 높이기 위한 것이다.    

LinkedList의 생성자와 메서드는 다음과 같다.  
- LinkedList(): LinkedList 객체를 생성
- LinkedList(Collection c): 주어진 컬렉션을 포함하는 LinkedList 객체를 생성
- boolean add(Object o): 지정된 객체를 LinkedList의 끝에 추가. 저장에 성공하면 true, 실패하면 false
- void add(int index, Object element): 지정된 위치(index)에 객체(element)를 추가
- boolean addAll(Collection c): 주어진 컬렉션에 포함된 모든 요소를 LinkedList의 끝에 추가한다. 성공하면 true, 실패하면 false
- boolean addAll(int index, Collection c): 지정된 위치에 주어진 컬렉션에 포함된 모든 요소를 추가. 성공하면 true, 실패하면 false
- void clear(): LinkedList의 모든 요소를 삭제
- boolean contains(Object o): 지정된 객체가 LinkedList에 포함되어있는지 알려줌
- boolean containsAll(Collection c): 지정된 컬렉션의 모든 요소가 포함되었는지 알려줌
- Object get(int index): 지정된 위치의 객체를 반환
- int indexOf(Object o): 지정된 객체가 저장된 위치(앞에서 몇 번째)를 반환
- boolean isEmpty(): LinkedList가 비어있는지 알려준다. 비어있으면 true
- Iterator iterator(): Iterator를 반환한다.
- int lastIndexOf(Object o): 지정된 객체의 위치를 반환(끝부터 역순검색)
- ListIterator listIterator(): ListIterator를 반환
- ListIterator listIterator(int index): 지정된 위치부터 시작하는 ListIterator를 반환
- Object remove(int index): 지정된 위치의 객체를 LinkedList에서 제거
- boolean remove(Object o): 지정된 객체를 LinkedList에서 제거. 성공하면 true, 실패하면 false
- boolean removeAll(Collection c): 지정된 컬렉션의 요소와 일치하는 요소를 모두 삭제
- boolean retainAll(Collection c): 지정된 컬렉션의 요소와 일치하는 요소만 제외하고 모두 삭제
- Object set(int index, Object element): 지정된 위치의 객체를 주어진 객체로 바꿈
- int size(): LinkedList에 저장된 객체의 수를 반환
- List subList(int fromIndex, int toIndex): LinkedList의 일부를 List로 반환
- Object[] toArray(): LinkedList에 저장된 객체를 배열로 반환
- Object[] toArray(Object[] a): LinkedList에 저장된 객체를 주어진 배열에 저장하여 반환
- Object element(): LinkedList의 첫 번째 요소를 반환
- boolean offer(Object o): 지정된 객체를 LinkedList의 끝에 추가. 성공하면 true, 실패하면 false
- Object peek(): LinkedList의 첫 번째 요소를 반환
- boolean poll(): LinkedList의 첫 번째 요소를 반환. LinkedList에서는 제거된다.
- Object remove(): LinkedList의 첫 번째 요소를 제거
- void addFirst(Object o): LinkedList의 맨 앞에 객체를 추가
- void addLast(Object o): LinkedList의 맨 끝에 객체를 추가
- Iterator descendingIterator(): 역순으로 조회하기 위한 DescendingIterator를 반환
- Object getFirst(): LinkedList의 첫 번째 요소를 반환
- Object getLast(): LinkedList의 마지막 요소를 반환
- boolean offerFirst(Object o): LinkedList의 맨 앞에 객체를 추가. 성공하면 true
- boolean offerLast(Object o): LinkedList의 맨 끝에 객체를 추가. 성공하면 true
- Object peekFirst(): LinkedList의 첫 번째 요소를 반환
- Object peekLast(): LinkedList의 마지막 요소를 반환
- Object pollFirst(): LinkedList의 첫 번째 요소를 반환하면서 제거
- Object pollLast(): LinkedList의 마지막 요소를 반환하면서 제거
- Object pop(): removeFirst()와 동일. LinkedList의 첫 번째 요소를 제거
- void push(Object o): addFirst()와 동일. LinkedList의 맨 앞에 객체를 추가
- Object removeFirst(): LinkedList의 첫 번째 요소를 제거
- Object removeLast(): LinkedList의 마지막 요소를 제거
- boolean removeFirstOccurence(Object o): LinkedList에서 첫 번째로 일치하는 객체를 제거
- boolean removeLastOccurence(Object o): LinkedList에서 마지막으로 일치하는 객체를 제거

<br>

LinkedList 역시 List 인터페이스를 구현했기 떄문에 ArrayList와 내부구현방법만 다를 뿐 제공하는 메서드의 종류와 기능은 거의 같다.  
ArrayList와 LinkedList의 성능차이는 다음과 같다.  

1. **순차적으로 추가/삭제하는 경우에는 ArrayList가 LinkedList보다 빠르다.**  
   - 단, ArrayList의 크기가 충분하지 않으면, 새로운 크기의 ArrayList를 생성하고 데이터를 복사하는 일이 발생하게 되므로 순차적으로 데이터를 추가해도 ArrayList보다 LinkedList가 더 빠를 수 있다.  
   - 순차적으로 삭제한다는 것은 마지막 데이터부터 역순으로 삭제해나가는 것을 의미하며, ArrayList는 마지막 데이터부터 삭제할 경우 각 요소들의 재배치가 필요하지 않기 때문에 상당히 빠르다.(단지 마지막 요소의 값을 null로만 바꾸면 되니까)  
2. **중간 데이터를 추가/삭제하는 경우에는 LinkedList가 ArrayList보다 빠르다.**  
   - 중간 요소를 추가 또는 삭제하는 경우, LinkedList는 각 요소간의 연결만 변경해주면 되기 때문에 처리속도가 상당히 빠르다.  
3. **특정 위치의 값을 읽는 시간(접근시간)은 ArrayList가 LinkedList보다 빠르다.**  
   - 배열은 각 요소들이 연속적으로 메모리상에 존재하기 때문에 간단한 계산만으로 원하는 요소의 주소를 얻어서 저장된 데이터를 곧바로 읽어올 수 있지만, LinkedList는 불연속적으로 위치한 각 요소들이 서로 연결된 것이라 처음부터 n번째 데이터까지 차례대로 따라가야만 원하는 값을 얻을 수 있다.  

|컬렉션|읽기(접근시간)|추가/삭제| 비고                               |
|:---:|:---:|:---:|:---------------------------------|
|ArrayList|빠르다|느리다| 순차적인 추가/삭제는 더 빠름<br>비효율적인 메모리 사용 |
|LinkedList|느리다|빠르다| 데이터가 많을수록 접근성이 떨어짐               |

<br><br>

### Stack과 Queue  
스택은 마지막에 저장한 데이터를 가장 먼저 꺼내게 되는 LIFO(Last In First Out) 구조로 되어 있고, 
큐는 처음에 저장한 데이터를 가장 먼저 꺼내게 되는 FIFO(First In First Out) 구조로 되어 있다.  

순차적으로 데이터를 추가하고 삭제하는 스택은 ArrayList와 같은 배열기반의 컬렉션 클래스가 적합하다.  
큐는 ArrayList보다 데이터의 추가/삭제가 쉬운 LinkedList로 구현하는 것이 적합하다.  

Stack의 메서드는 다음과 같다.  
- boolean empty(): Stack이 비어있는지 알려준다.  
- Object peek(): Stack의 맨 위에 저장된 객체를 반환. pop()과 달리 Stack에서 객체를 꺼내지는 않음.(비어있을 때는 EmptyStackException 발생)  
- Object pop(): Stack의 맨 위에 저장되는 객체를 꺼낸다. (비어있을 때는 EmptyStackException 발생)  
- Object push(Object item): Stack에 객체(item)를 저장한다.
- int search(Object o): Stack에서 주어진 객체를 찾아서 그 위치를 반환. 못 찾으면 -1을 반환. (배열과 달리 위치는 0이 아닌 1부터 시작)

<br>

Queue의 메서드는 다음과 같다.  
- boolean add(Object o): 지정된 객체를 Queue에 추가한다. 성공하면 true를 반환. 저장공간이 부족하면 IllegalStateException 발생
- Object remove(): Queue에서 객체를 꺼내 반환. 비어있으면 NoSuchElementException 발생
- Object element(): 삭제없이 요소를 읽어온다. peek과 달리 Queue가 비어있을 때 NoSuchElementException 발생
- boolean offer(Object o): Queue에 객체를 저장. 성공하면 true, 실패하면 false를 반환
- Object poll(): Queue에서 객체를 꺼내서 반환. 비어있으면 null을 반환
- Object peek(): 삭제없이 요소를 읽어온다. Queue가 비어있으면 null을 반환

<br>

```java
public static void main(String[] args){
    Stack st = new Stack();
    Queue q = new LinkedList();   // Queue 인터페이스의 구현체인 LinkedList를 사용
    
    st.push("0"); st.push("1"); st.push("2");
    q.offer("0"); q.offer("1"); q.offer("2");
    
    System.out.println("= Stack =");
    while(!st.empty()){
        System.out.println(st.pop());
    }
    
    System.out.println("= Queue ");
    while(!q.isEmpty()){
        System.out.println(q.poll());
    }
}
```
```text
// 실행 결과
= Stack = 
2
1
0
= Queue =
0
1
2
```

<br>

자바에서는 스택을 Stack 클래스로 구현하여 제공하고 있지만 
큐는 Queue 인터페이스로 정의해 놓았을 뿐 별도의 클래스를 제공하고 있지 않다. 
대신 Queue 인터페이스를 구현한 클래스들이 있어서 이들 중의 하나를 선택해서 사용하면 된다.   

Queue를 구현한 클래스들은 대표적으로 ArrayDeque, LinkedList, PriorityQueue 등이 있다.  
각 클래스들은 Queue 인터페이스에 정의된 메서드를 모두 작성해 놓았으면, 대부분 거의 같은 기능을 한다. 

- PriorityQueue
  - Queue 인터페이스의 구현체 중의 하나로, 저장한 순서에 관계없이 우선순위가 높은 것부터 꺼내게 된다는 특징이 있다. 
  - null은 저장할 수 없다. null을 저장하면 NullPointerException이 발생한다.  
  - 저장공간으로 배열을 사용하며, 각 요소를 '힙(heap)'이라는 자료구조의 형태로 저장한다. 힙은 이진 트리의 한 종류로 가장 큰 값이나 가장 작은 값을 빠르게 찾을 수 있다는 특징이 있다.  
- Deque(Double-Ended Queue)  
  - Queue의 변형으로, 한 쪽 끝으로만 추가/삭제할 수 있는 Queue와 달리, Deque는 양쪽 끝에 추가/삭제가 가능하다.  
  - Deque의 조상은 Queue이며, 구현체로는 ArrayDeque와 LinkedList 등이 있다. 
  - Deque은 스택과 큐를 하나로 합쳐놓은 것과 같으며 스택으로 사용할 수도 있고, 큐로 사용할 수도 있다.  
    
      |Deque|Queue|Stack|
      |:---:|:---:|:---:|
      |offerLast()|offer()|push()|
      |pollLast()|-|pop()|
      |pollFirst()|poll()|-|
      |peekFirst()|peek()|-|
      |peekLast()|-|peek()|
    <br>
   <img width="1096" alt="스크린샷 2023-02-05 오후 8 37 27" src="https://user-images.githubusercontent.com/54930365/216816475-c626212c-faca-4ed3-806d-bc897e1af946.png">


<br>

```text
스택의 활용 예 - 수식 계산, 수식괄호검사, 워드프로세서의 undo/redo, 웹브라우저의 뒤로/앞으로  
큐의 활용 예 - 최근사용문서, 인쇄작업 대기목록, 버퍼(buffer)  
```

<br><br>  

### Iterator, ListIterator, Enumeration  
Iterator, Listiterator, Enumeration은 모두 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스이다.  
Enumeration은 Iterator의 구버전이며, ListIterator는 Iterator의 기능을 향상시킨 것이다.  

<br>

#### 1. Iterator  
컬렉션 프레임웍에서는 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화하였다. 
컬렉션에 저장된 각 요소에 접근하는 기능을 가진 Iterator 인터페이스를 정의하고, Collection 인터페이스에는 'Iterator(Iterator를 구현한 클래스의 인스턴스)'를 반환하는 iterator()를 정의하고 있다.  
컬렉션 클래스에 대해 iterator()를 호출하여 Iterator를 얻은 다음 반복문, 주로 while문을 사용해서 컬렉션 클래스의 요소들을 읽어올 수 있다.  

- boolean hasNext(): 읽어 올 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환한다.  
- Object next(): 다음 요소를 읽어 온다. next()를 호출하기 전에 hasNext()를 호출해서 읽어 올 요소가 있는지 확인하는 것이 안전하다.  
- void remove(): next()로 읽어온 요소를 삭제한다. next()를 호출한 다음에 remove()를 호출해야한다. (선택적 기능)  

Iterator를 이용해서 컬렉션의 요소를 읽어오는 방법을 표준화했기 떄문에 이처럼 코드의 재사용성을 높이는 것이 가능하다.  
참조변수의 타입을 Collection 타입으로 두는 것이 좋다. 그 이유는 만일 Collection 인터페이스를 구현한 다른 클래스로 바꿔야 한다면 선언문 하나만 변경하면 나머지 코드는 검토하지 않아도 되기 때문이다.  

Map 인터페이스를 구현한 컬렉션 클래스는 키와 값을 쌍으로 저장하고 있기 때문에 iterator()를 직접 호출할 수 없고, 그 대신 keySet()이나 entrySet()과 같은 메서드를 통해서 키와 값을 각각 따로 Set의 형태로 얻어 온 후에 다시 Iterator()를 호출해야 Iterator를 얻을 수 있다.  
```java
Map map = new HashMap();
        ...
Iterator it = map.entrySet().iterator();
```

<br>
List 클래스들은 저장순서를 유지하기 때문에 Iterator를 이용해서 읽어 온 결과 역시 저장 순서와 동일하지만 Set 클래스들은 각 요소간의 
순서가 유지되지 않기 때문에 Iterator를 이용해서 저장된 요소들을 읽어 와도 처음에 저장된 순서와 같지 않다.  

<br>

#### 2. ListIterator와 Enumeration  
Enumeration은 컬렉션 프레임워크가 만들어지기 이전에 사용하던 것으로 Iterator의 구버전이라고 생각하면 된다. 
이전 버전으로 작성된 소스와의 호환을 위해서 남겨두고 있을 뿐이므로 가능하면 Enumeration 대신 Iterator를 사용하자.  

ListIterator는 Iterator를 상속받아서 기능을 추가한 것으로, 컬렉션의 요소에 접근할 때 Iterator는 단방향으로만 이동할 수 있는 데 반해 
ListIterator는 양방향으로 이동이 가능하다. 
다만 ArrayList나 LinkedList와 같이 List 인터페이스를 구현한 컬렉션에서만 사용할 수 있다.   
- Enumeration: Iterator의 구버전
- ListIterator: Iterator에 양방향 조회기능 추가 (List를 구현한 경우만 사용 가능)  

<br>

Enumeration의 메서드는 다음과 같다.  
- boolean hasMoreElements(): 읽어 올 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환한다. Iterator의 hasNext()와 같다.  
- Object nextElement(): 다음 요소를 읽어 온다. nextElement()를 호출하기 전에 hasMoreElements()를 호출해서 읽어올 요소가 남아있는지 확인하는 것이 안전하다. Iterator의 next()와 같다.  


ListIterator의 메서드는 다음과 같다.  
- void add(Object o): 컬렉션에 새로운 객체를 추가한다. (선택적 기능)
- boolean hasNext(): 읽어올 다음 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환
- boolean hasPrevious(): 읽어올 이전 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환
- Object next(): 다음 요소를 읽어 온다. next()를 호출하기 전에 hasNext()를 호출해서 읽어올 요소가 있는지 확인하는 것이 안전하다.  
- Object previous(): 이전 요소를 읽어 온다. previous()를 호출하기 전에 hasPrevious()를 호출해서 읽어 올 요소가 있는지 확인하는 것이 안전하다.  
- int nextIndex(): 다음 요소의 index를 반환한다.
- int previousIndex)(): 이전 요소의 index를 반환한다.
- void remove(): next() 또는 previous()로 읽어 온 요소를 삭제한다. 반드시 next()나 previous()를 먼저 호출한 다음에 이 메서드를 호출해야한다. (선택적 기능)
- void set(Object o): next() 또는 previous()로 읽어 온 요소를 지정된 객체로 변경한다. 반드시 next()나 previous()를 먼저 호출한 다음에 이 메서드를 호출해야한다. (선택적 기능)

선택적 기능이라고 표시된 메서드들은 반드시 구현하지 않아도 된다. 그렇다하더라도 인터페이스로부터 상속받은 메서드는 추상메서드차 메서드의 몸통을 반드시 만들어 주어여 하므로 다음과 같이 처리한다.  
```java
// 선택적 기능인 remove() 메서드의 몸통 예시
public void remove() {
    throw new UnsupportedOperationException();
}
```
단순히 public void remove() {};와 같이 구현하는 것보다 이처럼 예외를 던져서 구현되지 않은 기능이라는 것을 메서드를 호출하는 쪽에 알리는 것이 좋다.  
remove 메서드를 지원하지 않는 Iterator는 UnsupportedOperationException을 발생시킨다. 
remove 메서드의 선언부에서 예외처리를 하지 않는 이유는 UnsupportedOperationException이 RuntimeException의 자손이기 때문이다.  

<br><br>

### Arrays  
Arrays 클래스에는 배열을 다루는데 유용한 메서드가 정의되어 있다. 
같은 기능의 메서드가 배열의 타입만 다르게 오버로딩되어 있어서 많아 보이지만, 실제로는 그리 많지 않다.  
<br>

#### toString()
모든 기본형 배열과 참조형 배열 별로 하나씩 toString()이 정의되어 있다.  
<br>

#### 배열의 복사 - copyOf(), copyOfRange()  
copyOf()는 배열 전체를, copyOfRange()는 배열의 일부를 복사해서 새로운 배열을 만들어 반환한다.   
```java
int arr = [0,1,2,3,4];
int arr2 = Arrays.copyOf(arr, arr.length);  // arr2 = [0,1,2,3,4]
int arr3 = Arrays.copyOf(arr, 3);           // arr3 = [0,1,2]
int arr4 = Arrays.copyOfRange(arr, 2, 4);   // arr4 = [2,3]
```
<br>

#### 배열 채우기 - fill(), setAll()
fill()은 배열의 모든 요소를 지정된 값으로 채운다. setAll()은 배열을 채우는데 사용할 함수형 인터페이스를 매개변수로 받는다. 
setAll()을 호출할 때는 함수형 인터페이스를 구현한 객체를 매개변수로 지정하던가 아니면 람다식을 지정해야한다.  
```java
int[] arr = new int[5];
Arrays.fill(arr, 9);    // arr = [9,9,9,9,9]
Arrays.setAll(arr, () -> (int) (Math.random()*5)+1);    // arr = [1,5,2,1,1]
```
<br>

#### 배열의 정렬과 검색 - sort(), binarySearch()  
sort()는 배열을 정렬할 때, 그리고 배열에 저장된 요소를 검색할 때는 binarySearch()를 사용한다. 
binarySearch()는 배열에서 지정된 값이 저장된 위치를 찾아서 반환하는데, 반드시 배열이 정렬된 상태이어야 올바른 결과를 얻는다. 
그리고 만일 검색한 값과 일치하는 요소들이 여러 개 있다면, 이 중에서 어떤 것의 위치가 반환될지는 알 수 없다.  
```java
int[] arr = {3, 2, 0, 1, 4};
int idx = Arrays.binarySearch(arr, 2);      // idx=-5, 잘못된 결과

Arrays.sort(arr);   // 배열 arr을 정렬한다. 
System.out.println(Arrays.toString(arr));   // [0, 1, 2, 3, 4]
int idx = Arrays.binarySearch(arr, 2);       // idx = 2, 올바른 결과
```
이진 검색은 배열의 검색할 범위를 반복적으로 절반씩 줄여가면서 검색하기 때문에 검색 속도가 상당히 빠르다. 
단, 배열이 정렬되어 있는 경우에만 사용할 수 있다는 단점이 있다.  
<br>

#### 배열의 비교와 출력 - equals(), toString()  
toString()은 배열의 모든 요소를 문자열로 편하게 출력할 수 있다. 
toString()은 일차원 배열에서만 사용할 수 있으므로, 다차원 배열에는 deepToString()을 사용해야 한다. 
deepToString()은 배열의 모든 요소를 재귀적으로 접근해서 문자열을 구성하므로 2차원뿐만 아니라 3차원 이상의 배열에도 동작한다.  

equals()는 두 배열에 저장된 모든 요소를 비교해서 같으면 true, 다르면 false를 반환한다. 
equals()도 일차원 배열에만 사용가능하므로, 다차원 배열의 비교에는 deepEquals()를 사용해야 한다.  
다차원 배열은 '배열의 배열'의 형태로 구성하기 때문에 equals()로 비교하면, 문자열을 비교하는 것이 아니라 '배열에 저장된 배열의 주소'를 기뵤하게 된다. 
서로 다른 배열은 항상 주소가 다르므로 false를 결과로 얻는다.  
<br>

#### 배열을 List로 변환 - asList(Object... a)  
asList()는 배열을 List에 담아서 반환한다. 
매개변수 타입이 가변인수라서 배열 생성 없이 저장할 요소들만 나열하는 것도 가능하다.  
한 가지 주의할 점은 asList()가 반환한 List의 크기를 변경할 수 없다는 것이다. 즉, 추가 또는 삭제가 불가능하다. 
저장된 내용은 변경가능하다. 만일 크기를 변경할 수 있는 List가 필요하다면 다음과 같이 하면 된다.  
```java
List list = new ArrayList(Arrays.asList(1,2,3,4,5));
```
<br>

#### parallelXXX(), spliterator(), stream()  
이 외에도 'parallel'로 시작하는 이름의 메서드들이 있는데, 이 메서드들은 보다 빠른 결과를 얻기 위해 여러 쓰레드가 작업을 나누어 처리하도록 한다. 
spliterator()는 여러 쓰레드가 처리할 수 있게 하나의 작업을 여러 작업으로 나누는 Spliterator를 반환하며, stream()은 컬렉션을 스트림으로 변환한다.  

<br><br>

### Comparator와 Comparable  
Arrays.sort()를 호출만 하면 컴퓨터가 알아서 배열을 정렬하는 것처럼 보이지만, 사실은 Character 클래스의 Comparpable의 구현에 의해 정렬되었던 것이다.  
Comparator와 Comparable은 모두 인터페이스로 컬렉션을 정렬하는데 필요한 메서드를 정의하고 있으며, Comparable을 구현하고 있는 클래스들은 같은 타입의 인스턴스끼리 서로 비교할 수 있는 클래스들, 주로 Integer와 같은 wrapper 클래스와 String, Data, File과 같은 것들이며 기본적으로 오름차순, 
즉 작은 값에서부터 큰 값의 순으로 정렬되도록 구현되어 있다. 
그래서 Comparable을 구현한 클래스는 정렬이 가능하다는 것을 의미한다.  

Comparator와 Comparable의 실제 소스는 다음과 같다.  
```java
public interface Comparator {
    int compare(Object o1, Object o2);
    boolean equals(Object obj);
}
public interface Comparable {
    public int compareTo(Object o);
}
```
compare()와 compareTo()는 선언형태와 이름이 약간 다를 뿐 두 객체를 비교한다는 같은 기능을 목적으로 고안된 것이다. 
compareTo()의 반환값은 int이지만 실제로는 비교하는 두 객체가 같으면 0, 비교하는 값보다 작으면 음수, 크면 양수를 반환하도록 구현해야 한다. 
이와 마찬가지로 compare()도 객체를 비교해서 음수, 0, 양수 중의 하나를 반환하도록 구현해야 한다.  

equals 메서드는 모든 클래스가 가지고 있는 공통적인 메서드이지만, Comparator를 구현하는 클래스는 오버라이딩이 필요할 수도 있다는 것을 알리기 위해서 정의한 것일 뿐, 그냥 compare(Object o1, Object o2)만 구현하면 된다.  

Comparable을 구현한 클래스들이 기본적으로 오름차순으로 정렬되어 있지만, 다른 기준에 의해서 정렬되도록 하고 싶을 때 Comparator를 구현해서 정렬기준을 제공할 수 있다.  
- **Comparable: 기본 정렬기준을 구현하는데 사용**
- **Comparator: 기본 정렬기준 외에 다른 기준으로 정렬하고자할 때 사용**  

```java
// Comparator와 Comparable 사용 예제
import java.util.*;

class ComparatorEx {
    public static void main(String[] args){
        String[] strArr = {"cat", "Dog", "lion", "tiger"};
        
        Arrays.sort(strArr);                // String의 Comparable 구현에 의한 정렬
        Arrays.sort(str, String.CASE_INSENSITIVE_ORDER);    // 대소문자 구분 안함
        Arrays.sort(str,new Descending());  // 역순 정렬
    }
}

class Descending implements Comparator {
    public int compare(Object o1, Object o2) {
        if (o1 instanceof Comparable && o2 instanceof Comparable) {
            Comparable c1 = (Comparable) o1;
            Comparable c2 = (Comparable) o2;
            return c1.compareTo(c2) * -1;   // -1을 곱해서 기본 정렬방식의 역으로 변경한다.
        }
        return -1;
    }    
}
```

전에 배운 것과 같이, Arrays.sort()는 배열을 정렬할 때, Comprator를 지정해주지 않으면 
저장하는 객체(주로 Comparable을 구현한 클래스의 객체)에 구현된 내용에 따라 결정된다.  

- **static void sort(Object[] a) : 객체 배열에 저장된 객체가 구현한 Comparable에 의한 정렬**
- **static void sort(Object[] a, Comparator c) : 지정한 Comparator에 의한 정렬** 

String의 Comprable 구현은 문자열이 사전 순으로 정렬되도록 작성되어 있다. 
문자열의 오름차순 정렬은 공백, 숫자, 대문자, 소문자의 순으로 정렬되는 것을 의미하다. 
정확히 얘기하면 문자의 유니코드의 순서가 작은 값에서부터 큰 값으로 정렬되는 것이다.  
그리고 대소문자를 구분하지 않고 비교하는 Comparator를 상수 형태인 `String.CASE_INSENSITIVE_ORDER`로 제공한다.  

String의 기본 정렬을 반대로 하는 것, 즉 문자열의 내림차순을 구현하는 것은 아주 간단하다. 
단지 String에 구현된 compareTo()의 결과에 -1을 곱하기만하면 된다.  
```java
class Descending implements Comparator {
    public int compare(Object o1, Object o2) {
        if (o1 instanceof Comparable && o2 instanceof Comparable) {
            Comparable c1 = (Comparable) o1;
            Comparable c2 = (Comparable) o2;
            return c1.compareTo(c2) * -1;   // -1을 곱해서 기본 정렬방식의 역으로 변경한다.
        }
        return -1;
    }    
}
```

<br><br>

### HashSet  
HashSet은 Set 인터페이스를 구현한 가장 대표적인 컬렉션이며, Set 인터페이스의 특징대로 **HashSet은 중복된 요소를 저장하지 않는다.**  
**HashSet은 저장순서를 유지하지 않으므로 저장순서를 유지하고자 한다면 LinkedHashSet을 사용해야한다.**   

HashSet은 내부적으로 HashMap을 이용해서 만들어졌으며, HashSet이란 이름은 해싱(hashing)을 사용해서 구현했기 때문에 붙여진 것이다.  

HashSet의 생성자 또는 메서드는 다음과 같다.  
- HashSet(): HashSet 객체를 생성한다.
- HashSet(Collection c): 주어진 컬렉션을 포함하는 HashSet 객체를 생성한다.  
- HashSet(int initialCapacity): 주어진 값을 초기용량으로 하는 HashSet 객체를 생성한다.
- HashSet(int initialCapacity, float loadFactor): 초기용량과 load factor를 지정하는 생성자.
- boolean add(Object o): 새로운 객체를 저장한다.
- boolean addAll(Collection c): 주어진 컬렉션에 저장된 모든 객체들을 추가한다. (합집합)  
- void clear(): 저장된 모든 객체를 삭제한다.
- Object clone(): HashSet을 복제해서 반환한다. (얕은 복사)  
- boolean contains(Object o): 지정된 객체를 포함하고 있는지 알려준다.  
- boolean containsAll(Collection c): 주어진 컬렉션에 저장된 모든 객체들을 포함하고 있는지 알려준다.  
- boolean isEmpty(): HashSet이 비어있는지 알려준다.  
- Iterator iterator(): Iterator를 반환한다.
- boolean remove(Object o): 지정된 객체를 HashSet에서 삭제한다. (성공하면 true, 실패하면 false)  
- boolean removeAll(Collection c): 주어진 컬렉션에 저장된 모든 객체와 동일한 것들을 HashSet에서 모두 삭제한다. (차집합) 
- boolean retainAll(Collection c): 주어진 컬렉션에 저장된 객체와 동일한 것만 남기고 삭제한다. (교집합)  
- int size(): 저장된 객체의 개수를 반환한다.
- Object[] toArray(): 저장된 객체들을 객체배열의 형태로 반환한다.
- Object[] toArray(Object[] a): 저장된 객체들을 주어진 객체배열(a)에 담는다.  

HashSet의 메서드를 호출하는 것만으로도 간단하게 합집합(addAll), 교집합(retainAll), 차집합(removeAll)을 구할 수 있다.   
<br>

HashSet에 클래스의 인스턴스를 새로운 요소로 추가할 경우, 두 인스턴스가 같은 것으로 인식하게 하려면 어떻게 해야 할까?  
HashSet의 add 메서드는 새로운 요소를 추가하기 전에 기존에 저장된 요소와 같은 것인지 판별하기 위해 추가하려는 요소의 equals()와 hashCode()를 호출하기 때문에 
equals()와 hashCode()를 목적에 맞게 오버라이딩해야 한다.  
```java
// equals()와 hashCode() 오버라이딩 예시
public boolean equals(Object o){
    if (obj instanceof Person2) {
        Person2 tmp = (Person2) obj;
        return name.equals(tmp.name) && age == tmp.age;
    }
}

public int hashCode(){
    return Objects.hash(name, age);     // JDK1.8부터 Objects 클래스의 hash 메서드가 추가되었다.
//    return (name+age).hashCode();     // JDK1.8이전 버전에서는 이처럼 String 클래스의 hashCode()를 활용하여 간단하게 처리할 수 있다.
}
```
오버라이딩을 통해 작성된 hashCode()는 다음의 세 가지 조건을 만족시켜야 한다.  
1. **실행 중인 애플리케이션 내의 동일한 객체에 대해서 여러 번 hashCode()를 호출해도 동일한 int 값을 반환해야한다. 하지만 실행시마다 동일한 int값을 반환할 필요는 없다.**
   1. String 클래스는 문자열의 내용으로 해시코드를 만들어 내기 때문에 내용이 같은 문자열에 대한 hashCode() 호출은 항상 동일한 해시코드를 반환한다. 반면에 Object 클래스는 객체의 주소로 해시코드를 만들어 내기 때문에 실행할 때마다 해시코드값이 달라질 수 있다.  
2. **equals 메서드를 이용한 비교에 의해서 true를 얻은 두 객체에 대해 각각 hashCode()를 호출해서 얻은 결과는 반드시 같아야 한다.**
3. **equals 메서드를 호출했을 때 false를 반환하는 두 객체는 hashCode() 호출에 대해 같은 int를 반환하는 경우가 있어도 괜찮지만, 해싱(hashing)을 사용하는 컬렉션의 성능을 향상시키기 위해서는 다른 int 값을 반환하는 것이 좋다.**  
   1. 서로 다른 객체에 대해서 해시코드값이 중복되는 경우가 많아질수록 해싱을 사용하는 Hashtable, HashMap과 같은 컬렉션의 검색속도가 떨어진다.  
   2. 두 객체에 대해 equals 메서드를 호출한 결과가 true이면, 두 객체의 해시코드는 반드시 같아야하지만, 두 객체의 해시코드가 같다고 해서 equals 메서드의 호출결과가 반드시 true여야 하는 것은 아니다.  

<br>

사용자정의 클래스를 작성할 때 equals 메서드를 오버라이딩해야 한다면 hashCode()도 클래스의 작성의도와 맞게 오버라이딩하는 것이 원칙이지만, 
경우에 따라 위의 예제처럼 간단히 구현하거나 생략해도 별 문제가 되지 않으므로 hashCode()를 구현하는데 너무 부담을 갖지 않아도 된다.  

<br><br>

### TreeSet  
TreeSet은 `이진 검색 트리(binary search tree)라는 자료구조의 형태로 데이터를 저장하는 컬렉션 클래스`이다.  
이진 검색 트리는 정렬, 검색, 범위검색에 높은 성능을 보이는 자료구조이며 
TreeSet은 이진 검색 트리의 성능을 향상시킨 **'레드-블랙 트리'**로 구현되어 있다.  
그리고 Set 인터페이스를 구현했으므로 중복된 **데이터의 저장을 허용하지 않으며 정렬된 위치에 저장하므로 저장순서를 유지하지도 않는다.**  

이진 검색 트리는 부모노드의 왼쪽에는 부모노드의 값보다 작은 자식노드를 오른쪽에는 큰 값의 자식노드를 저장하는 이진 트리이다.  
TreeSet에 저장되는 객체가 Comparable을 구현하던가 아니면 TreeSet에게 Comparator를 제공해서 두 객체를 비교할 방법을 알려줘야 한다. 
그렇지 않으면 TreeSet에 객체를 저장할 때 예외가 발생한다.  

왼쪽 마지막 값에서부터 오른쪽 값까지 값을 '왼쪽 노드 -> 부모 노드 -> 오른쪽 노드' 순으로 읽어오면 오름차순으로 정렬된 순서를 얻을 수 있다.  
TreeSet은 이처럼 정렬된 상태를 유지하기 때문에 단일 값 검색과 범위검색이 매우 빠르다.  

> #### 이진 검색 트리는 
> - 모든 노드는 **최대 두 개의 자식노드**를 가질 수 있다.
> - 왼쪽 자식노드의 값은 부모노드의 값보다 작고 오른쪽 자식노드의 값은 부모노드의 값보다 커야한다.
> - 노드의 추가/삭제에 시간이 걸린다. (순차적으로 저장하지이 아니라 저장위치를 찾아서 저장해야하고 삭제하는 경우 트리의 일부를 재구성해야하기 때문)
> - **검색(범위검색)과 정렬에 유리하다.**
> - **중복된 값을 저장하지 못한다.**

<br>

TreeSet의 생성자 또는 메서드는 다음과 같다.  
- TreeSet(): 기본 생성자
- TreeSet(Collection c): 주어진 컬렉션을 저장하는 TreeSet을 생성
- TreeSet(SortedSet c): 주어진 SortedSet을 구현한 컬렉션을 저장하는 TreeSet을 생성
- boolean add(Object o): 지정된 객체를 TreeSet에 추가
- boolean addAll(Collection c): 주어진 컬렉션의 객체들을 TreeSet에 추가
- Object ceiling(Object o): 지정된 객체와 같은 객체를 반환. 없으면 큰 값을 가진 객체 중 제일 가까운 값의 객체를 반환. 없으면 null
- void clear(): 저장된 모든 객체를 삭제한다.
- Comparator comparator(): TreeSet을 복제하여 반환한다.
- boolean contains(Object o): 지정된 객체가 포함되어 있는지 확인한다.
- boolean containsAll(Collection c): 지정된 Collection의 객체들이 포함되어 있는지 확인한다.
- NavigableSet descendingSet(): TreeSet에 저장된 요소들을 역순으로 정렬해서 반환한다.
- Object first(): 정렬된 순서에서 첫 번째 객체를 반환한다.
- Object floor(Object o): 지정된 객체와 같은 객체를 반환한다. 없으면 작은 값을 가진 객체 중 제일 가까운 값의 객체를 반환한다. 없으면 null  
- SortedSet headSet(Object toElement): 지정된 객체보다 작은 값의 객체들을 반환한다.
- NavegableSet headSet(Object toElement, boolean inclusive): 지정된 객체보다 작은 값의 객체들을 반환한다. inclusive가 true이면, 같은 값의 객체도 포함한다.
- Object higher(Object o): 지정된 객체보다 큰 값을 가진 객체 중 제일 가까운 값의 객체를 반환한다. 없으면 null
- boolean isEmpty(): TreeSet이 비어있는지 확인한다.
- Iterator iterator(): TreeSet의 Iterator를 반환한다.
- Object last(): 정렬된 순서에서 마지막 객체를 반환한다.
- Object lower(Object o): 지정된 객체보다 작은 값을 가진 객체 중 제일 가까운 값의 객체를 반환한다. 없으면 null
- Object pollFirst(): TreeSet의 첫번째 요소(제일 작은 값의 객체)를 반환한다.
- Object pollLast(): TreeSet의 마지막 번째 요소(제일 큰 값의 객체)를 반환한다.
- boolean remove(Object o): 지정된 객체를 삭제한다.
- boolean retainAll(Collection c): 주어진 컬렉션과 공통된 요소만을 남기고 삭제한다. (교집합)  
- int size(): 저장된 객체의 개수를 반환한다.
- Spliterator spliterator(): TreeSet의 spliterator를 반환한다.
- SortedSet subSet(Object fromElement, Object toElement): 범위검색(fromElement와 toElement 사이)의 결과를 반환한다. (끝 범위인 toElement는 범위에 포함되지 않음)  
- Navigable<E> subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive): 범위 검색의 결과를 반환한다. (fromInclusive가 true이면 시작값이 포함되고, toInclusive가 true이면 끝값이 포함된다.)  
- SortedSet tailSet(Object fromElement): 지정된 객체보다 큰 값의 객체들을 반환한다.
- Object[] toArray(): 저장된 객체를 객체배열로 반환한다.
- Object[] toArray(Object[] a): 저장된 객체를 주어진 객체배열에 저장하여 반환한다.  

<br>
문자열의 경우 정렬순서는 문자의 코드값이 되므로, 오름차순 정렬의 경우 코드값의 크기가 작은 순서에서 큰 순서, 
즉 공백, 숫자, 대문자, 소문자 순으로 정렬되고 내림차순의 경우 그 반대가 된다.  
그래서 가능하면 대문자 또는 소문자로 통일해서 저장하는 것이 좋으며 다른 방식으로 정렬해야하는 경우 Comparator를 이용하면 된다.  

<br><br>

### HashMap과 Hashtable  
Hashtable과 HashMap의 관계는 Vector와 ArrayList의 관계와 같아서 
Hashtable보다 새로운 버전인 HashMap을 사용할 것을 권한다.  

HashMap은 Map을 구현했으므로 **키와 값을 묶어서 하나의 데이터(entry)로 저장한다**는 특징을 갖는다. 
**그리고 해싱을 사용하기 때문에 많은 양의 데이터를 검색하는데 있어서 뛰어난 성능을 보인다.**   

HashMap은 키와 값을 각각 Object타입으로 저장한다. 
즉 (Object, Object)의 형태로 저장하기 때문에 어떠한 객체도 저장할 수 있지만 키는 주로 String을 대문자 또는 소문자로 통일해서 사용하곤 한다.  
- 키(key): 컬렉션 내의 키 중에서 유일해야 한다.
- 값(value): 키와 달리 데이터의 중복을 허용한다.  

<br>

HashMap의 생성자와 메서드는 다음과 같다.  
- HashMap(): HashMap 객체를 생성한다.
- HashMap(int initialCapacity): 지정된 값을 초기용량으로 하는 HashMap 객체를 생성한다.
- HashMap(int initialCapacity, float loadFactor): 지정된 초기용량과 load factor의 HashMap 객체를 생성한다.  
- HashMap(Map m): 지정된 map의 모든 요소를 포함하는 HashMap을 생성한다.
- void clear(): HashMap에 저장된 모든 객체를 제거한다.
- Object clone(): 현재 HashMap을 복제해서 반환한다.
- boolean containsKey(Object key): HashMap에 지정된 키가 포함되어있는지 알려준다. 
- boolean containsValue(Object value): HashMap에 지정된 값이 포함되어있는지 알려준다.  
- Set entrySet(): HashMap에 저장된 키와 값을 엔트리(키와 값의 결합) 형태로 Set에 저장해서 반환한다.  
- Object get(Object key): 지정된 키의 값을 반환한다. 못 찾으면 null 반환
- Object getOrDefault(Object key, Object defaultValue): 지정된 키의 값을 반환한다. 키를 못찾으면, 기본값으로 지정된 객체를 반환한다.  
- boolean isEmpty(): HashMap이 비어있는지 알려준다.
- Set keySet(): HashMap에 저장된 모든 키가 저장된 Set을 반환한다.
- Object put(Object key, Object value): 지정된 키와 값을 HashMap에 저장한다.
- void putAll(Map m): Map에 저장된 모든 요소를 HashMap에 저장한다.
- Object remove(Object key): HashMap에서 지정된 키로 저장된 값을 제거한다.
- Object replace(Object key, Object value): 지정된 키의 값을 지정된 객체로 대체한다.
- boolean replace(Object key, Object oldValue, Object newValue): 지정된 키와 객체가 모두 일치하는 경우에만 새로운 객체로 대체한다.
- int size(): HashMap에 저장된 요소의 개수를 반환한다
- Collection values(): HashMap에 저장된 모든 값을 컬렉션의 형태로 반환한다.  

<br>

**Hashtable은 키나 값으로 null을 허용하지 않지만, HashMap은 허용한다.**  
그래서 'map.put(null,null)'이나 'map.get(null)'과 같이 할 수 있다.  

HashMap은 데이터를 키와 값을 모두 Object 타입으로 저장하기 때문에 HashMap의 값으로 HashMap을 다시 저장할 수 있다. 
이렇게 함으로써 하나의 키에 다시 복수의 데이터를 저장할 수 있다.  

HashMap과 같이 **해싱을 구현한 컬렉션 클래스들은 저장순서를 유지하지 않는다.**    

<br>

#### 해싱과 해시함수  
해싱이란 `해시함수를 이용해서 데이터를 해시테이블에 저장하고 검색하는 기법`이다.  
해시함수는 데이터가 저장되어 있는 곳을 알려 주기 때문에 다량의 데이터 중에서도 원하는 데이터를 빠르게 찾을 수 있다.  
해싱을 구현한 컬렉션 클래스로는 HashSet, HashMap, Hashtable 등이 있다.  
Hashtable은 컬렉션 프레임웍이 도입되면서 HashMap으로 대체되었으나 이전 소스와의 호환성 문제로 남겨 두고 있다.   

해싱에서 사용하는 자료구조는 **배열과 링크드리스트의 조합**으로 되어있다.  
저장할 데이터의 키를 해시함수에 넣으면 배열의 한 요소를 얻게 되고, 다시 그 곳에 연결되어 있는 링크드리스트에 저장하게 된다.  
해싱 과정은 다음과 같다.  
1. 검색하고자 하는 값의 키로 해시함수를 호출한다.
2. 해시함수의 계산결과(해시코드)로 해당 값이 저장되어 있는 링크드 리스트를 찾는다.
3. 링크드리스트에서 검색한 키와 일치하는 데이터를 찾는다.  

빠른 검색 결과를 얻기 위해서는 하나의 링크드 리스트에 최소한의 데이터만 자정되도록 저장될 데이터의 크기를 고려해서
HashMap의 크기를 적절하게 지정해주어야 하고, 해시함수가 서로 다른 키에 대해서 중복된 해시코드의 반환을 최소화해야 한다.  

실제로는 HashMap과 같이 해싱을 구현한 클래스에서는 Object 클래스에 정의된 hashCode()를 해시함수로 사용한다. 
Object 클래스에 정의된 hashCode()는 객체의 주소를 이용하는 알고리즘으로 해시코드를 만들어 내기 때문에 모든 객체에 대해 hashCode()를 호출한 결과가 서로 유일한 훌륭한 방법이다.  
String 클래스의 경우 Object로부터 상속받은 hashCode()를 오버라이딩해서 문자열의 내용으로 해시코드를 만들어 낸다. 
그래서 서로 다른 String 인스턴스일지라도 같은 내용의 문자열을 가졌다면 hashCode()를 호출하면 같은 해시코드를 얻는다.  

HashSet에서 이미 설명했던 것과 같이 서로 다른 두 객체에 대해 equals()로 비교한 결과가 true인 동시에 hashCode()의 반환값이 같아야 같은 객체로 인식한다. 
HashMap에서도 같은 방법으로 객체를 구별하며, **이미 존재하는 키에 대한 값을 저장하면 기존의 값을 새로운 값으로 덮어쓴다.**   

그래서 새로운 클래스를 정의할 때 equals()를 재정의 오버라이딩해야 한다면 hashCode()도 같이 재정의해서 equals()의 결과가 true인 두 객체의 hashCode() 결과값이 항상 같도록 해줘야 한다. 
그렇지 않으면 HashMap과 같이 해싱을 구현한 컬렉션 클래스에서는 equals()의 호출 결과가 true이지만 해시코드가 다른 두 객체를 서로 다른 것으로 인식하고 따로 저장할 것이다.
(equals()로 비교한 결과가 false이고 해시코드가 같은 경우에도 같은 링크드 리스트(서랍)에 저장된 서로 다른 두 데이터가 된다.)  

<br><br>





