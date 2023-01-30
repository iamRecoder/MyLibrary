# CH11. 컬렉션 프레임웍 
## 컬렉션 프레임웍(Collections Framework)  
컬렉션 프레임웍이란, `데이터 군을 저장하는 클래스들을 표준화한 설계`를 뜻한다. 
컬렉션(Collection)은 다수의 데이터, 즉 데이터 그룹을, 프레임웍은 표준화된 프로그래밍 방식을 의미한다.  

JDK1.2부터 컬렉션 프레임웍이 등장하면서 다양한 종류의 컬렉션 클래스가 추가되고 모든 컬렉션 클래스를 표준화된 방식으로 다룰 수 있도록 체계화되었다.  
컬렉션 프레임웍은 컬렉션(다수의 데이터)을 다루는 데 필요한 다양하고 풍부한 클래스들을 제공하기 때문에 프로그래머의 짐을 상당히 덜어 주고 있으며, 또한 인터페이스와 다형성을 이용한 객체지향적 설계를 통해 표준화되어 있기 떄문에 사용법을 익히기에도 편리하고 재사용성이 높은 코드를 작성할 수 있다는 장점이 있다.   

## 컬렉션 프레임웍의 핵심 인터페이스  
컬렉션 프레임웍에서는 컬렉션데이터 그룹을 크게 3가지 타입이 존재한다고 인식하고 각 컬렉션을 다루는데 필요한 기능을 가진 3개의 인터페이스 `List`, `Set`, `Map`를 정의했다. 
인터페이스 List와 Set을 구현한 컬렉션 클래스들은 서로 많은 공통부분이 있어서, 공통된 부분을 다시 뽑아 Collection 인터페이스를 정의할 수 있었지만 Map 인터페이스는 이들과는 전혀 다른 형태로 컬렉션을 다루기 때문에 같은 상속계층도에 포함되지 못했다.  

![image](https://user-images.githubusercontent.com/54930365/215392479-afebe185-f028-48da-9eb3-8b24bf373042.png)  
사진출처: https://keepmind.net/java-collection-framework-1/  

|인터페이스| 특징                                                                                                                        |
|:---:|:--------------------------------------------------------------------------------------------------------------------------|
|List| 순서가 있는 데이터의 집합. 데이터의 중복을 허용한다.<br>구현 클래스: ArrayList, LinkedList, Stack, Vector 등                                          |
|Set| 순서를 유지하지 않는 데이터의 집합. 데이터의 중복을 허용하지 않는다.<br>구현 클래스: HashSet, TreeSet 등                                                     |
|Map| 키와 값의 쌍으로 이루어진 데이터의 집합.<br>순서는 유지되지 않으며, 키는 중복을 허용하지 않고 값은 중복을 허용한다.<br>구현 클래스: HashMap, TreeMap, Hashtable, Properties 등 |

컬렉션의 프레임웍의 모든 컬렉션들은 List, Set, Map 중 하나를 구현하고 있으며, 구현한 인터페이스의 이름이 클래스의 이름에 포함되어있어서 이름만으로도 클래스의 특징을 쉽게 알 수 있다.  
그러나 Vector, Stack, Hashtable, Properties와 같은 클래스들은 컬렉션 프레임웍이 만들어지기 이전부터 존재하던 것이기 때문에 컬렉션 프레임웍의 명명법을 따르지 않는다.  
Vector나 HashTable과 같은 기존의 컬렉션 클래스들은 호환을 위해, 설계를 변경해서 남겨두었지만 가능하면 사용하지 않는 것이 좋다. 그 대신 새로 추가된 ArrayList와 HashMap을 사용하자.  

![image](https://user-images.githubusercontent.com/54930365/215382400-be969d4a-6c37-496a-8fc5-b68fba3195f9.png)
사진 출처: https://velog.io/@da__hey/Java-Java-Collections-List-Set-Map-%EA%B0%9C%EB%85%901

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
List 인터페이스는 중복을 허용하면서 저장순서가 유지되는 컬렉션을 구현하는데 사용된다.  
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
Set 인터페이스는 중복을 허용하지 않고 저장순서가 유지되지 않는 컬렉션 클래스를 구현하는데 사용된다. Set 인터페이스를 구현한 클래스로는 HashSet, TreeSet 등이 있다.  
![image](https://user-images.githubusercontent.com/54930365/215392410-39f3d9dc-14de-4f44-82fb-5705ac766fe0.png)

사진출처: https://keepmind.net/java-collection-framework-3/  

### Map 인터페이스   
Map 인터페이스는 키(key)와 값(value)을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는 데 사용된다. 
키는 중복될 수 없지만 값은 중복을 허용한다. 
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


## 컬렉션 프레임웍의 구성요소들
### ArrayList
ArrayList는 List 인터체이스를 구현하기 때문에 데이터의 저장순서가 유지되고 중복을 허용한다는 특징을 갖는다.  


