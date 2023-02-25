# CH12. 지네릭스, 열거형, 애너테이션

## 지네릭스(Generics)  
지네릭스는 JDK1.5에서 처음 도입되었다.  

<br>

### 지네릭스란  
지네릭스는 `다양한 타입의 객체를 다루는 메서드나 컬렉션 클래스에 컴파일 시의 타입 체크(compile-time type check)를 해주는 기능`이다. 
객체의 타입을 컴파일 시에 체크하기 때문에 객체의 타입 안정성을 높이고 형변환의 번거로움이 줄어든다.  

예를 들어, ArrayList와 같은 컬렉션 클래스는 다양한 종류의 객체를 담을 수 있긴 하지만 보통 한 종류의 객체를 담는 경우가 더 많다. 
그런데도 꺼낼 때 마다 타입체크를 하고 형변환을 하는 것은 아무래도 불편할 수밖에 없다. 
게다가 원하지 않는 종류의 객체가 포함되는 것을 막을 방법이 없다는 것도 문제다. 이러한 문제들을 지네릭스가 해결해준다.  

> 지네릭스의 장점  
> 1. 타입 안정성을 제공한다.
> 2. 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해진다.  

<br>

### 지네릭 클래스의 선언  
클래스를 지네릭 클래스로 변경하면 클래스 옆에 `<T>`를 붙이고 'Object'를 모두 `T`로 바꾸면 된다.  
```java
class Box<T> {   // 지네릭 타입 T를 선언  
    T item;   
    void setItem(T item){ this.item = item; }
    T getItem(){ return item; }
}
```

<br>

`T`를 '타입 변수(type variable)'라고 하며, 'Type'의 첫글자에서  따온 것이다. 
타입 변수는 T가 아닌 다른 것을 사용해도 된다.  
ArrayList<E>와 Map<K,V>와 같이 무조건 'T'를 사용하기보다는 상황에 맞게 의미있는 문자를 선택해서 사용하는 것이 좋다.  

**이들은 기호의 종류만 다를 뿐 `임의의 참조형 타입`을 의미한다는 것은 모두 같다.**   
지네릭이 도입되기 이전의 코드와 호환을 위해, 지네릭 클래스인데도 예전의 방식으로 객체를 생성하는 것이 허용된다. 
다만, 지네릭 타입을 지정하지 않아서 안전하지 않다는 경고가 발생한다.  
```java
Box b1 = new Box();         // OK. T는 Object로 간주된다. 
b1.setItem(new Object());   // 경고. unchecked or unsafe operation

Box<Object> b2 = new Box<Object>();         // OK. T 대신 Object 타입을 지정
b2.setItem(new Object());   // 경고 발생 안함. 타입을 지정하지 않은 것이 아니라 알고 적은 것이기 때문.        

Box<String> b3 = new Box<String>(); // 타입 T 대신, 실제 타입을 지정
b3.setItem(new Object());   // 에러. String 타입 이외의 타입은 지정 불가.
b3.setItem("ABC");          // OK. String 타입이므로 가능
```

<br>

#### 지네릭스 용어  
`Class Box<T> {}`  
```text
Box<T>    지네릭 클래스. 'T의 Box' 또는 'T Box'라고 읽는다.  
T         타입 변수 또는 타입 매개변수.(T는 타입 문자)   
Box       원시 타입(raw type)   
```
타입 변수 또는 타입 매개변수는 메서드의 매개변수와 유사한 면이 있다. 그래서 타입 매개변수에 타입을 지정하는 것을 `지네릭 타입 호출`이라고 하고, 
지정된 타입 'String'을 `매개변수화된 타입`이라고 한다.  

Box<String>과 Box<Integer>는 지네릭 클래스 Box<T>에 서로 다른 타입을 대입하여 호출한 것일 뿐, 
이 둘이 별개의 클래스를 의미하는 것은 아니다. 
이는 마치 매개변수의 값이 다른 메서드 호출, 즉 add(3,5)와 add(2,4)가 서로 다른 메서드를 호출하는 것이 아닌 것과 같다.  

<br>

#### 지네릭스의 제한  
지네릭스는 인스턴스별로 다르게 동작하도록 하기 위해 만들어진 기능이다. 
그래서 지네릭 클래스 Box의 객체를 생성할 때, 객체별로 다른 타입을 지정하는 것은 적절하다.  
그러나 모든 객체에 대해 동일하게 동작해야하는 static 멤버에 타입 변수 T를 사용할 수 없다. 
T는 인스턴스변수로 간주되기 때문이다. 앞에서 배웠듯이 static 멤버는 인스턴스변수를 참조할 수 없다.   
static 멤버는 타입 변수에 지정된 타입, 즉 대입된 타입의 종류에 관계없이 동일한 것이어야 한다.  

그리고 지네릭 타입의 배열을 생성하는 것도 허용되지 않는다. 
지네릭 배열 타입의 참조변수를 선언하는 것은 가능하지만, 'new T[10]'과 같이 배열을 생성하는 것은 안된다는 뜻이다. 
```java
T[] itemArr;    // OK. T타입의 배열을 위한 참조변수
T[] tmpArr new T[3];    // 에러. 지네릭 배열 생성불가  
```

지네릭 배열을 생성할 수 없는 이유는 new 연산자 때문이다. 
이 연산자는 컴파일 시점에 타입 T가 무엇인지 정확히 알아야 한다. 그런데 지네릭 클래스를 컴파일하는 시점에서 T가 정확히 어떤 타입이 될지 전혀 알 수 없다.  
더하여, instanceof 연산자도 new 연산자와 같은 이유로 T를 피연산자로 사용할 수 없다.  

꼭 지네릭 배열을 생성해야할 필요가 있을 때는, new 연산자 대신 'Reflection API'의 newInstance()와 같이 
동적으로 객체를 생성하는 메서드로 배열을 생성하거나, Object 배열을 생성해서 복사한 다음에 'T[]'로 형변환하는 방법 등을 사용한다.  

<br><br>

### 지네릭 클래스의 객체 생성과 사용  
지네릭 클래스의 객체를 생성할 때는 참조변수와 생성자에 대입된 타입(매개변수화된 타입)이 일치해야 한다. 
일치하지 않으면 에러가 발생한다.  
두 타입이 상속관계에 있어도 마찬가지이다.  
단, 두 지네릭 클래스의 타입이 상속관계에 있고, 대입된 타입이 같은 것은 괜찮다.  
```java
Box<Apple> appleBox = new Box<Apple>();     // OK.
Box<Apple> appleBox = new Box<Grape>();     // 에러. 참조변수와 생성자에 대입된 타입이 불일치

Box<Fruit> appleBox = new Box<Apple>();     // 에러. Apple이 Fruit의 자손이더라도 참조변수와 생성자에 대입된 타입이 불일치하기 때문

Box<Apple> appleBox = new FruitBox<Apple>();    // OK. 다형성. FruitBox는 Box의 자손이다.
```

<br>

JDK1.7부터는 추정이 가능한 경우 타입을 생략할 수 있게 되었다. 
참조변수의 타입으로 생성자의 타입을 유추할 수 있기 때문에 생성자에 반복해서 타입을 지정해주지 않아도 되는 것이다.  
```java
Box<Apple> appleBox = new Box<>();      // OK. JDK1.7부터 생략 가능
```

<br>

생성된 Box<T>의 객체에 'void add(T item)'으로 객체를 추가할 때, 대입된 타입과 다른 타입의 객체는 추가할 수 없다.  
그러나 타입 T의 자손들은 이 메서드의 매개변수가 될 수 있다.  
```java
Box<Apple> appleBox = new Box<Apple>();
appleBox.add(new Apple());  // OK.
appleBox.add(new Grape());  // 에러. Box<Apple>에는 Apple 객체만 추가 가능
appleBox.add(new GreenApple()); // OK. 다형성. GreenApple은 Apple의 자손이다. 
```

<br><br>

### 제한된 지네릭 클래스  
지네릭 타입에 `extends`를 사용하면, 특정 타입의 자손들만 대입할 수 있게 제한할 수 있다.  
```java
class FruitBox<T extends Fruit> {
    ...
}
```

<br>

만일 클래스가 아니라 인터페이스를 구현해야 한다는 제약이 필요하다면, 이때도 `extends`를 사용한다. 
`implements`를 사용하지 않는다는 점에 주의하자.  

클래스 Fruit의 자손이면서 Eatable 인터페이스도 구현해야한다면 아래와 같이 `&` 기호로 연결한다.  
이제 FruitBox에는 Fruit의 자손이면서 Eatable을 구현한 클래스만 타입 매개변수 T에 대입될 수 있다.  
```java
class FruitBox<T extends Fruit & Eatable> {
    ...
}
```

<br>
<br>

### 와일드 카드  
지네릭 클래스라 하더라도 static 메서드에는 타입 매개변수 T를 매개변수에 사용할 수 없으므로 특정 타입을 T 대신 지정해줘야 한다.  
이렇게 지네릭 타입 대신 특정 타입으로 매개변수를 지정할 경우, 다른 타입의 객체는 static 메서드의 매개변수가 될 수 없다.  
```java
static Juice makeJuice(FruitBox<T> box){ ... }          // static 메서드에는 타입 매개변수 T를 매개변수에 사용할 수 없다.  
static Juice makeJuice(FruitBox<Fruit> box){ ... }      // 따라서, static 메서드에 지네릭스를 사용할 경우, 타입 매개변수 T 대신 특정 타입을 지정해줘야 한다.
```

<br>

```java
static Juice makeJuice(FruitBox<Fruit> box){ ... }
static Juice makeJuice(FruitBox<Apple> box){ ... }
```
그러나 위의 예시처럼 지네릭 타입이 다르게 오버로딩하면, 컴파일 에러가 발생한다. **지네릭 타입이 다른 것만으로는 오버로딩이 성립하지 않기 때문이다.**  
지네릭 타입은 컴파일러가 컴파일할 때만 사용하고 제거해버린다. 그래서 위의 두 메서드는 오버로딩이 아니라 `메서드 중복 정의`이다.  

이럴 때 사용하기 위해 고안된 것이 바로 `와일드 카드`이다.  
와일드카드는 기호 '?'로 표현하는데, 와일드 카드는 어떠한 타입도 될 수 있다.  
'?'만으로는 Object 타입과 다를 게 없으므로, 다음과 같이 'extends'와 'super'로 상한과 하한을 제한할 수 있다.  
```text
<? extends T> 와일드 카드의 상한 제한. T와 그 자손들만 가능
<? super T>   와일드 카드의 하한 제한. T와 그 조상들만 가능
<?>           제한 없음. 모든 타입이 가능. <? extends Object>와 동일
```

<br>

와일드 카드를 사용해서 makeJuice()의 매개변수 타입을 FruitBox<Fruit>에서 FruitBox<? extends Fruit>으로 바꾸면 
다음과 같이 된다.  
```java
static Juice makeJuice(FruitBox<? extends Fruit> Box){
        ...
        }
```

<br><br>

### 지네릭 메서드  
메서드의 선언부에 지네릭 타입이 선언된 메서드를 지네릭 메서드라 한다.  
지네릭 타입의 선언 위치는 반환 타입 바로 앞이다.  

지네릭 클래스에 정의된 타입 매개변수와 지네릭 메서드에 정의된 타입 매개변수는 전혀 별개의 것이다. 
같은 타입 문자 T를 사용해도 같은 것이 아니라는 것에 주의해야 한다.  
지네릭 메서드는 지네릭 클래스가 아닌 클래스에도 정의될 수 있다.  
```java
class FruitBox<T>{
    ...
    static <T> void sort(List<T> list, Comparator<? super T> c){
        ...
    }
}
```
위의 코드에서 지네릭 클래스 FruitBox에 선언된 타입 매개변수 T와 지네릭 메서드 sort()에 선언된 타입 매개변수 T는 타입 문자만 같을 뿐 서로 다른 것이다.  
그리고 sort()가 static 메서드라는 것에 주목하자. 
앞서 설명한 것처럼, static 멤버에는 타입 매개변수를 사용할 수 없지만, 이처럼 메서드에 지네릭 타입을 선언하고 사용하는 것은 가능하다.  

메서드에 선언된 지네릭 타입은 지역 변수를 선언한 것과 같다고 생각하면 이해하기 쉬운데, 이 타입 매개변수는 메서드 내에서만 지역적으로 사용될 것이므로 메서드가 static이건 아니건 상관이 없다.  

앞서 나왔던 makeJuice()를 지네릭 메서드로 바꾸면 다음과 같다.  
```java
// before
static Juice makeJuice(FruitBox<? extends Fruit> box){
    String tmp = "";
    for(Fruit f : box.getList()) tmp += f + " ";
    return new Juice(tmp);
}
```
```java
// after
static <T extends Fruit> Juice makeJuice(FruitBox<T> box){
    String tmp = "";
    for(Fruit f : box.getList()) tmp += f + " ";
    return new Juice(tmp);
}
```

이제 이 메서드를 호출할 때는 아래와 같이 타입 변수에 타입을 대입해야 한다.  
```java
FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
System.out.println(Juicer.<Fruit>makeJuice(fruitBox));
```
그러나 대부분의 경우 컴파일러가 타입을 추정할 수 있기 때문에 생략해도 된다.  

지네릭 메서드는 매개변수의 타입이 복잡할 때도 유용하다. 만일 아래와 같은 코드가 있다면 타입을 별도로 선언함으로써 코드를 간략히 할 수 있다.  
```java
public static void printAll(ArrayList<? extends Product> list, ArrayList<? extends Product> list2){
        ...
}
```
```java
public static <T extends Product> void printAll(ArrayList<T> list, ArrayList<T> list2){
        ...
}
```

<br><br>

### 지네릭 타입의 형변환  
지네릭 타입과 넌지네릭(non-generic) 타입간의 형변환은 항상 가능하다.  
하지만 대입된 타입과 다른 지네릭 타입간의 형변환은 불가능하다.  
```java
Box box = null;
Box<Object> objBox = null;
box = (Box)objBox;              // OK. 지네릭 타입 -> 원시 타입. 경고 발생
objBox = (Box<Object>)box;     // OK. 원시 타입 -> 지네릭 타입. 경고 발생

Box<Object> objBox = null;
Box<String> strBox = null;
objBox = (Box<Object>)strBox;   // 에러. Box<String> -> Box<Object>
strBox = (Box<String>)objBox;   // 에러. Box<Object> -> Box<String>

Box<? extends Object> wBox = new Box<String>();     // OK. 형변환 가능. 매개변수에 다형성이 적용되었다.
```

<br><br>

### 지네릭 타입의 제거 
컴파일러는 지네릭 타입을 이용해서 소스파일을 체크하고, 필요한 곳에 형변환을 넣어준다. 
그리고 지네릭 타입을 제거한다 
즉, 컴파일된 파일(*.class)에는 지네릭 타입에 대한 정보가 없는 것이다.  

이렇게 하는 주된 이유는 지네릭이 도입되기 이전의 소스 코드와의 호환성을 유지하기 위해서이다.  
JDK1.5부터 지네릭스가 도입되었지만, 아직도 원시 타입을 사용해서 코드를 작성하는 것을 허용한다. 
그러나 앞으로 가능하면 원시 타입을 사용하지 않도록 하자.  

지네릭 타입의 제거 과정은 꽤 복잡하다. 
기본적인 제거과정에 대해서만 살표볼 것이다.  
> 1. 지네릭 타입의 경계(bound)를 제거한다.
>    - 지네릭 타입이 <T extends Fruit>라면 T는 Fruit로 치환된다. \<T>인 경우는 T는 Object로 치환된다. 그리고 클래스 옆의 선언은 제거된다.
>      ```java
>      // before
>      class Box<T extends Fruit> {
>       void add(T t) { ... }
>      }
>      ```
>      ```java
>      // after
>      class Box {
>       void add(Fruit t) { ... }
>      }
>      ```
> 2. 지네릭 타입을 제거한 후에 타입이 일치하지 않으면, 형변환을 추가한다.
>   - List의 get()은 Object 타입을 반환하므로 형변환이 필요하다.
>   - 와일드 카드가 포함되어 있는 경우에는 적절한 타입으로 형변환이 추가된다.
>     ```java
>     // before
>     T get(int i) {
>       return list.get(i);
>     }
>     ```
>     ```java
>     // after
>     Fruit get(int i) {
>       return (Fruit)list.get(i);
>     }
>     ```

<br><br>

## 열거형(enums)  
열거형은 서로 관련된 상수를 편리하게 선언하기 위한 것으로 여러 상수를 정의할 때 사용하면 유용하다.  
원래 자바에는 열거형이 존재하지 않았으나 JDK1.5부터 새로 추가되었다.  
자바의 열거형은 열거형이 갖는 값뿐만 아니라 타입도 관리하기 때문에 보다 논리적인 오류를 줄일 수 있다.  
```java
class Card {
    enum Kind { CLOVER, HEART, DIAMOND, SPADE }
    enum Value { TWO, THREE, FOUR }
    
    final Kind kind;    // 타입이 int가 아닌 Kind임에 유의하자.
    final Value value;
}
```

기존의 많은 언어들, 예를 들어 C언어에서는 타입이 달라도 값이 같으면 조건식 결과가 참이었으나, 
자바의 열거형은 `타입에 안전한 열거형`이라서 실제 값이 같아도 타입이 다르면 컴파일 에러가 발생한다. 
이처럼 값뿐만 아니라 타입까지 체크하기 때문에 타입에 안전하다고 하는 것이다.  
```java
if (Card.Kind.CLOVER == Card.Value.Two)     // 컴파일 에러. 값은 같지만 타입이 다르다.
```

<br>

그리고 더 중요한 것은 상수의 값이 바뀌면, 해당 상수를 참조하는 모든 소스를 다시 컴파일해야 한다는 것이다.  
하지만 열거형 상수를 사용하면, 기존의 소스를 다시 컴파일하지 않아도 된다.  

<br><br>

### 열거형의 정의와 사용  
`enum 열거형이름 { 상수명1, 상수명2, ... }`  

열거형에 정의된 상수를 사용하는 방법은 '열거형이름.상수명'이다. 클래스의 static 변수를 참조하는 것과 동일하다.  

열거형 상수간의 비교에는 '=='를 사용할 수 있다. 
equals가 아닌 '=='로 비교가 가능한 만큼 빠른 성능을 제공한다.  
그러나 '<','>'와 같은 비교연산자는 사용할 수 없고 compareTo()는 사용가능하다.  
```java
if(dir == Direction.EAST){  
        ...
} else if(dir > Direction.WEST){    // 에러. 열거형 상수에 비교연산자 사용불가
        ...
} else if(dir.compareTo(Direction.WEST) > 0){   // compareTo()는 가능
        ...
}
```

<br>

switch문의 조건식에도 열거형을 사용할 수 있다.  
이 때 주의할 점은 case 문에 열거형의 이름은 적지 않고 상수의 이름만 적어야 한다는 제약이 있다. 
아마도 그렇게 하는 것이 오타도 줄일 수 있고 보기에 간결하기 때문이다.  
```java
switch(dir){
    case EAST: 
        break;
    case WEST:
        break;
}
```

<br>

#### 모든 열거형의 조상 - java.lang.Enum  
열거형 Direction에 저의된 모든 상수를 출력하려면, 다음과 같이 한다.  
```java
Direction[] dArr = Direction.values();
for(Direction d : dArr){
    System.out.println(d.name() + d.ordinal());
}
```
values()는 열거형의 모든 상수를 배열에 담아 반환한다. 
이 메서드는 모든 열거형이 가지고 있는 것으로 컴파일러가 자동으로 추가해 준다. 
그리고 ordinal()은 모든 열거형의 조상인 java.lang.Enum 클래스에 정의된 것으로, 열거형 상수가 정의된 순서(0부터 시작)를 정수로 반환한다.  

Enum 클래스에는 그 밖에도 다음과 같은 메서드가 정의되어 있다.  
- Class<E> getDeclaringClass(): 열거형 Class객체를 반환한다.
- String name(): 열거형 상수의 이름을 문자열로 반환한다.
- int ordinal(): 열거형 상수가 정의된 순서를 반환한다. (0부터 시작)
- T valueOf(Class<T> enumType, String name): 지정된 열거형에서 name과 일치하는 열거형 상수를 반환한다.  

<br>

values() 이외에도 컴파일러가 자동적으로 추가해주는 메서드가 하나 더 있다.  
```java
static E values()
static E valueOf(String name)
```
valueOf(String name) 메서드는 열거형 상수의 이름으로 문자열 상수에 대한 참조를 얻을 수 있게 해준다.  
```java
Direction d = Direction.valueOf("WEST");
System.out.println(d);      // WEST
```

<br><br>

### 열거형에 멤버 추가하기  
Enum 클래스에 정의된 ordinal()이 열거형 상수가 정의된 순서를 반환하지만, 이 값을 열거형 상수의 값으로 사용하지 않는 것이 좋다. 
이 값은 내부적인 용도로만 사용되기 위한 것이기 때문이다.   

열거형 상수의 값이 불연속적인 경우에는 다음과 같이 열거형 상수의 이름 옆에 원하는 값을 괄호()와 함께 적어주면 된다.  
`enum Direction { EAST(1), SOUTH(5), WEST(-1), NORTH(10) }`  

그리고 지정된 값을 저장할 수 있는 인스턴스 변수와 생성자를 새로 추가해주어야 한다.  
이 때 주의할 점은, 먼저 열거형 상수를 모두 정의한 다음에 다른 멤버들을 추가해야 한다는 것이다. 
그리고 열거형 상수의 마지막에도 ';'을 잊지 말아야 한다.  
```java
enum Direction {
    EAST(1), SOUTH(5), WEST(-1), NORTH(10);     // 끝에 ';'을 추가햐야 한다.
    
    private final int value;    // 정수를 저장할 필드(인스턴스 변수)를 추가
    Direction(int value) { this.value = value; }    // 생성자를 추가
    
    public int getValue() { return value; }
}
```

열거형 인스턴스 변수는 반드시 final이어야 한다는 제약은 없지만, value는 열거형 상수의 값을 저장하기 위한 것이므로 final을 붙였다. 
그리고 외부에서 값을 얻을 수 있게 getValue()도 추가했다.  

열거형 Direction에 새로운 생성자가 추가되었지만, 위와 같이 열거형의 객체를 생성할 수 없다. 
열거형의 생성자는 제어자가 묵시적으로 private이기 때문이다.  
`Directin d = new Direction(1);   // 에러. 열거형의 생성자는 외부에서 호출불가`  

필요하다면 하나의 열거형 상수에 여러 값을 지정할 수도 있다. 
다만 그에 맞게 인스턴스 변수와 생성자 등을 새로 추가해주어야 한다.  

<br>

#### 열거형에 추상 메서드 추가하기  
열거형에 추상 메서드를 선언하면 각 열거형 상수가 이 추상 메서드를 반드시 구현해야 한다.   
아래의 예제에서는 각 열거형 상수가 fare()를 똑같은 내용으로 구현했지만, 다르게 구현될 수도 있게 하려고 추상 메서드로 선언한 것이다.  
```java
enum Transportation {
    BUS(100) {
        int fare(int distance) { return distance * BASIC_FARE; }
    },
    TRAIN(150) {
        int fare(int distance) { return distance * BASIC_FARE; }
    },
    AIRPLANE(300) {
        int fare(int distance) { return distance * BASIC_FARE; }
    };
    
    abstract int fare(int distance);    // 거리에 따른 요금을 계산하는 추상 메서드
    
    protected final int BASIC_FARE;     // protected로 해야 각 상수에서 접근가능
    
    Transportation(int basicFare){
        BASIC_FARE = basicFare;
    }
}
```

<br><br>

### 열거형의 이해  
열거형의 이해를 돕기 위해 마지막으로 열거형이 내부적으로 어떻게 구현되었는지에 대해 설명하고자 한다.  

만일 열거형 Direction이 다음과 같이 정의되어 있을 때, 
`enum Direction { EASR, SOUTH, WEST, NORTH }`  
사실은 열거형 상수 하나하나가 Direction 객체이다.  

위의 문장을 클래스로 정의한다면 다음과 같을 것이다.  
```java
class Direction {
    static final Direction EAST = new Direction("EAST");
    static final Direction SOUTH = new Direction("SOUTH");
    static final Direction WEST = new Direction("WEST");
    static final Direction NORTH = new Direction("NORTH");
    
    private String name;
    
    private Direction(String name){
        this.name = name;
    }
}
```
Direction 클래스의 static 상수 EAST, SOUTH, WEST, NORTH의 값은 객체의 주소이고, 
이 값은 바뀌지 않는 값이므로 '=='로 비교가 가능한 것이다.  

만약 열거형에 추상 메서드를 새로 추가하면, 클래스 앞에도 'abstract'를 붙여줘야 하고, 
각 static 상수들도 추상 메서드를 구현해주어야 한다.  
이제 왜 열거형에 추상 메서드를 추가하면 각 열거형 상수가 추상 메서드를 구현해야하는지 이해가 될 것이다.  


<br><br>

## 애너테이션(annotation)  
자바를 개발한 사람들은 소스코드와 문서를 하나의 파일로 관리하는 것이 낫다고 생각했다. 
그래서 소스코드의 주석에 소스코드에 대한 정보를 저장하고, 소스코드의 주석으로부터 HTML문서를 생성해내는 프로그램(javadoc.exe)을 만들어 사용했다.  
'/**'로 시작하는 주석 안에 소스코드에 대한 설명들이 있고 그 안에 '@'이 붙은 태그들이 눈에 띌 것이다. 
미리 정의된 태그들을 이용해서 주석 안에 정보를 저장하고, javadoc.exe라는 프로그램이 이 정보를 읽이서 문서를 작성하는데 사용한다.  

이 기능을 응용하여, **프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것**이 바로 `애너테이션`이다. 
**애너테이션은 주석처럼 프로그래밍 언어에 영향을 미치지 않으면서도 다른 프로그램에게 유용한 정보를 제공할 수 있다는 장점이 있다.**   

애너테이션은 JDK에서 기본적으로 제공하는 것과 다른 프로그램에서 제공하는 것들이 있다.  
JDK에서 제공하는 표준 애너테이션은 주로 컴파일러를 위한 것으로 컴파일러에게 유용한 정보를 제공한다. 
그리고 **새로운 애너테이션을 정의할 때 사용하는** `메타 애너테이션`을 제공한다.  

### 표준 애너테이션
- @Override: 컴파일러에게 오버라이딩하는 메서드라는 것을 알린다.
- @Deprecated: 앞으로 사용하지 않을 것을 권장하는 대상에 붙인다.
- @SuppressWarnings: 컴파일러의 특정 경고메시지가 나타나지 않게 해준다.
- @SafeVarags: 지네릭스 타입의 가변인자에 사용한다. (JDK1.7)
- @FunctionalInterface: 함수형 인터페이스라는 것을 알린다. (JDK1.8)
- @Native: native 메서드에서 참조되는 상수 앞에 붙인다. (JDK1.8)

여기서부터는 메타애너테이션이다.  
- @Target: 애너테이션이 적용가능한 대상을 지정하는데 사용한다.
- @Documented: 에너테이션 정보가 javadoc으로 작성된 문서에 포함되게 한다.
- @Inherited: 애너테이션 자손 클래스에 상속되도록 한다.
- @Retention: 애너테이션이 유지되는 범위를 지정하는데 사용한다.
- @Repeatable: 애너테이션을 반복해서 적용할 수 있게 한다. (JDK1.8)  

<br><br>

#### @Override
메서드 앞에만 붙일 수 있는 애너테이션으로, 조상의 메서드를 오버라이딩하는 것이라는 걸 컴파일러에게 알려주는 역할을 한다.  
메서드 앞에 '@Override' 애너테이션을 붙이면, 컴파일러가 같은 이름의 메서드가 조상에 있는지 확인하고 없으면, 에러메시지를 출력한다.  

<br>

#### @Deprecated
더 이상 사용되지 않는 필드나 메서드에 '@Deprecated'를 붙인다.  
이 애너테이션이 붙은 대상은 다른 것으로 대체되었으니 더 이상 사용하지 않을 것을 권한다는 의미이다.  
'@Deprecated'가 붙은 대상을 사용하지 않도록 권할 뿐 강제성은 없다.  

<br>

#### FunctionalInterface
함수형 인터페이스를 선언할 때, 이 애너테이션을 붙이면 컴파일러가 '함수형 인터페이스'를 올바르게 선언했는지 확인하고, 잘못된 경우 에러를 발생시킨다.  
필수는 아니지만, 붙이면 실수를 방지할 수 있으므로 함수형 인터페이스를 선언할 때 이 애너테이션을 반드시 붙이도록 하자.  

<br>

#### @SuppressWarnings
컴파일러가 보여주는 경고메시지가 나타나지 않게 억제해준다.  
물론 컴파일러의 경고메시지는 무시하고 넘어갈 수도 있지만, 모두 확인하고 해결해서 컴파일 후에 어떠한 메시지도 나타나지 않게 해야 한다.  

그러나 경우에 따라서는 경고가 발생할 것을 알면서도 묵인해야 할 때가 있다.  
이 경고를 그대로 나두면 컴파일할 때마다 메시지가 나타난다.  
이 때는 묵인해야하는 경고가 발생하는 대상에 만드시 '@SuppressWarning'을 붙여서 컴파일 후에 어떤 경고 메시지도 나타나지 않게 해야한다.  

'@SuppressWarning'로 억제할 수 있는 경고 메시지의 종류는 여러 가지가 있는데, JDK의 버전이 올라가면서 앞으로도 계속 추가될 것이다. 
이 중에서 주로 사용되는 것은 "deprecation", "unchecked", "rawtypes", "varargs" 정도이다.  

"deprecation"은 앞서 살펴본것과 같이 '@Deprecated'가 붙은 대상을 사용해서 발생하는 경고를, "unchecked"는 지네릭스로 타입을 지정하지 않았을 때 발생하는 경고를, "rawtypes"는 지네릭스를 사용하지 않아서 발생하는 경고를, "varargs"는 가변인자의 타입이 지네릭 타입일 때 발생하는 경고를 억제할 때 사용한다.   
억제하려는 경고 메시지를 애너테이션의 뒤에 괄호()안에 문자열로 지정하면 된다.  

만일 둘 이상의 경고를 동시에 억제하려면 다음과 같이 괄호{}를 추가로 사용하면 된다.  
`@SuppressWarnings({"deprecation", "unchecked", "varargs"})`  



