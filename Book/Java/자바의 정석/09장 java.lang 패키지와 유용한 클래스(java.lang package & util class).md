## java.lang 패키지  
java.lang 패키지는 자바 프로그래밍에 가장 기본이 되는 클래스들을 포함하고 있다. 
그렇기 때문에 java.lang 패키지의 클래스들은 import문 없이도 사용할 수 있게 되어 있다.  
그 동안 String 클래스나 System 클래스를 import문 없이 사용할 수 있었던 이유가 바로 java.lang 패키지에 속한 클래스들이기 때문이었던 것이다.  
다음은 java.lang 패키지의 여러 클래스들 중에서도 자주 사용된느 클래스들이다.  

### Object 클래스
Object 클래스는 모든 클래스의 최고 조상이기 때문에 Object 클래스의 멤버들은 모든 클래스에서 바로 사용할 수 있다.  
- **Object clone()**: 객체 자신의 복사본을 반환한다. Object 클래스에 정의된 clone()은 단순히 인스턴스변수의 값만 복사하기 때문에 참조타입의 인스턴스 변수가 있는 클래스는 완전한 인스턴스 복제가 이루어지지 않는다.   
    - 💡 Cloneable 인터페이스를 implements하고 clone() 메서드를 구현해야만 사용 가능! 그 이유는 인스턴스의 데이터를 보호하기 위해서이다. Cloneable 인터페이스가 구현되어 있다는 것은 클래스 작성자가 복제를 허용한다는 의미이다.  
    - 배열 뿐 아니라 java.util 패키지의 Vector, ArrayList, LinkedList, HashSet, TreeSet, HashMap, TreeMap, Calendar, Date와 같은 클래스들이 이와 같은 방식으로 복제가 가능하다.  
    - 얕은 복사와 깊은 복사  
        - 얕은 복사
            - 원본과 복사본이 같은 객체를 공유하므로 원본을 변경하면 복사본도 영향을 받는다.
            - 객체에 저장된 값을 그대로 복사할 뿐, 객체가 참조하고 있는 객체까지 복사하지는 않는다.  
            - clone()을 사용한 경우
        - 깊은 복사
            - 원본과 복사본이 서로 다른 객체를 참조하기 때문에 원본의 변경이 복사본에 영향을 미치지 않는다.  
            - 원본이 참조하고 있는 객체까지 복제하는 것이다.
- **boolean equals(Object obj)**: 객체 자신과 객체 obj가 같은 객체인지 알려준다. 두 참조변수에 저장된 값(주소값)이 같은지를 판단한다. 
- **String toString()**: 객체 자신의 정보를 문자열로 반환한다.  
    - System.out.println(인스턴스명) == System.out.println(인스턴스명.toString())  

<br> 

## String 클래스  
자바에서는 문자열을 위한 String 클래스를 제공한다. String 클래스는 문자열을 저장하고 이를 다루는데 필요한 메서드를 함께 제공한다.  

### 변경 불가능한(immutable) 클래스
String 클래스에는 문자열을 저장하기 위해서 문자형 배열 참조변수(char[]) value를 인스턴스 변수로 정의해놓고 있다. 
인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스 변수(value)에 문자형 배열(char[])로 저장되는 것이다.  
한번 생성된 String 인스턴스가 갖고 있는 문자열은 읽어 올 수만 있고, 변경할 수는 없다.  
문자열간의 결합이나 추출 등의 작업을 할 때마다 새로운 문자열을 가진 String 인스턴스가 생성되어 메모리공간을 차지하게 된다. 
따라서, 이러한 문자열을 다루는 작업이 많이 필요한 경우에는 String 클래스 대신 StringBuffer 클래스를 사용하는 것이 좋다. 
StringBuffer인스턴스에 저장된 문자열은 변경이 가능하기 때문에 하나의 StringBuffer인스턴스만으로도 문자열을 다루는 것이 가능하기 때문이다.  

### 문자열 비교
문자열을 만들 때는 두 가지 방법, `문자열 리터럴을 지정하는 방법`과 `String 클래스의 생성자를 사용해서 만드는 방법`이 있다.  
String클래스의 생성자를 이용한 경우에는 new 연산자에 의해서 메모리 할당이 이루어지기 때문에 항상 새로운 String 인스턴스가 생성된다. 
그러나 문자열 리터럴은 이미 존재하는 것을 재사용하는 것이다.  
두 문자열의 내용을 비교하는 equals() 메서드와 String인스턴스의 주소를 비교하는 등가비교연산자 '=='를 통해 위의 사실을 입증할 수 있다.   
```text
String str1 = "abc";
String str2 = "abc";
str1 == str2 ? true
str1.equals(str2) ? true

String str3 = new String("abc");
String str4 = new String("abc");
str3 == str4 ? false
str3.equals(str4) ? true
```
<br>

### 문자열 리터럴  
자바 소스파일에 포함된 모든 문자열 리터럴은 컴파이 시에 클래스 파일에 저장된다. 
이때 같은 내용의 문자열 리터럴은 한번만 저장된다. 문자열 리터럴도 String 인스턴스이고, 한번 생성하면 내용을 변경할 수 없으니 하나의 인스턴스를 공유하면 되기 때문이다.  
클래스 파일에는 소스파일에 포함된 모든 리터럴의 목록이 있다. 해당 클래스 파일이 클래스 로더에 의해 메모리에 올라갈 때, 이 리터럴의 목록에 있는 리터럴들이 JVM내에 있는 `상수 저장소(constant pool)`에 저장된다. 
`상수 저장소(constant pool)`에 문자열 리터럴이 자동적으로 생성되어 저장되는 것이다.  

### 빈 문자열(empty string)  
길이가 0인 배열이 존재할 수 있다. 
char형 배열은 길이가 0인 배열을 생성할 수 있고, 이 배열을 내부적으로 가지고 있는 문자열이 바로 빈 문자열이다.  
`String s="";`와 같이 길이가 0인 문자열을 생성하면, s가 참조하고 있는 String 인스턴스는 내부에 'new char[0]'과 같이 길이가 0인 char형 배열을 저장하고 있는 것이다.  
그러나 `char=''`는 불가능하다. char형 변수에는 반드시 하나의 문자를 지정해야 한다.  

따라서, 일반적으로 변수를 선언할 때, 각 타입의 기본값으로 초기화 하지만 String은 참조형 타입의 기본값인 null보다는 빈 문자열로, char 형은 기본값인 `\u0000`대신 공백으로 초기화하는 것이 보통이다.  
```java
// 초기화
String s = "";  // 빈 문자열로 초기화
char c = ' ';   // 공백으로 초기화  
```
<br>  

### String 클래스의 생성자와 메서드  
이는 자주 사용되는 String 클래스의 생성자와 메서드이다.  
- String(String s): 주어진 문자열을 갖는 String 인스턴스를 생성한다.
- String(char[] value): 주어진 문자열을 갖는 String 인스턴스를 생성한다.
- String(StringBuffer buf): StringBuffer인스턴스가 갖고 있는 문자열과 같은 내용의 String 인스턴스를 생성한다.  
- char charAt(int index): 지정된 위치에 있는 문자를 알려준다. index는 0부터 시작
- int compareTo(String str): 문자열과 사전순서로 비교한다. 같으면 0을, 사전순으로 이전이면 음수(-1)를, 이후면 양수(1)를 반환한다.
- String concat(String str): 문자열(str)을 뒤에 덧붙인다.
- boolean contains(CharSequence s): 지정된 문자열(s)이 포함되었는지 검사한다.
- boolean endsWith(String suffix): 지정된 문자열(suffix)로 끝나는지 검사한다.
- boolean equals(Object obj): obj가 String이 아니거나 문자열이 다르면 false를 반환한다.
- boolean equalsIgnoreCase(String str): 문자열과 String 인스턴스의 문자열을 대소문자 구분없이 비교한다.
- int indexOf(int ch): 주어진 문자(ch)가 문자열에 존재하는지 확인하여 위치(index)를 알려준다. 못 찾으면 -1을 반환한다.
- int indexOf(int ch, int pos): 주어진 문자(ch)가 문자열에 존재하는지 지정된 위치(pos)부터 확인하여 위치(index)를 알려준다.
- int indexOf(String str): 주어진 문자열이 존재하는지 확인하여 그 위치를 알려준다.
- int lastIndexOf(int ch): 지정된 문자 또는 문자코드를 문자열의 오른쪽 끝에서부터 찾아서 위치(index)를 알려준다.
- int lastIndexOf(String str): 지정된 문자열을 인스턴스의 문자열 끝에서부터 찾아서 위치를 알려준다.
- int length(): 문자열의 길이를 알려준다.
- String replace(char old, char new): 문자열 중 old를 모두 새로운 문자 new로 바꾼 문자열을 반환한다.
- String replace(CharSequence old, CharSequence new): 문자열 중 old를 모두 새로운 문자열 new로 바꾼 문자열을 반환한다.
- String replaceAll(String regex, String replacement): 문자열 중에서 지정된 문자열(regex)와 일치하는 것을 새로운 문자열(replacement)로 모두 변경한다.
  - replaceAll()은 첫번째 인자값으로 정규식을 받는다. 정규식을 이용해서 불특정 문자열을 변환할 수 있다.
    ```java
    String str = "aaabbbccccabcddddabcdeeee";
    String result1 = str.replace("abc", "왕");
    String result2 = str.replaceAll("[abc]", "왕");

    System.out.println("replace result->"+ result1);        // replace result->aaabbbcccc왕dddd왕deeee
    System.out.println("replaceAll result->"+ result2);     // replaceAll result->왕왕왕왕왕왕왕왕왕왕왕왕왕dddd왕왕왕deeee
    ```
    [예제 출처 - https://djusti.tistory.com/8](https://djusti.tistory.com/8)
- String replaceFirst(String regex, String replacement): 문자열 중에서 지정된 문자열(regex)와 일치하는 것 중, 첫 번째 것만 새로운 문자열(replacement)로 변경한다.
- String[] split(String regex): 문자열을 지정된 분리자(regex)로 나누어 문자열 배열에 담아 반환한다.
- String[] split(String regex, int limit): 문자열을 지정된 분리자(regex)로 나누어 문자열배열에 담아 반환한다. 단, 문자열 전체를 지정된 수(limit)로 자른다.
- boolean startsWith(String prefix): 주어진 문자열(prefix)로 시작하는지 검사한다.
- String substring(int begin): 주어진 시작위치(begin)부터의 문자열을 얻는다. 이 때, 시작위치의 문자는 범위에 포함된다.
- String substring(int begin, int end): 주어진 시작위치(begin)부터 끝 위치(end) 범위에 포함된 문자열을 얻는다. 이 때, 시작위치의 문자는 범위에 포함되지만, 끝 위치의 문자는 포함되지 않는다.
- String toLowerCase(): String 인스턴스에 저장되어있는 모든 문자열을 소문자로 변환하여 반환한다.
- String toString(): String 인스턴스에 저장되어 있는 문자열을 반환한다.
- String toUpperCase(): String 인스턴스에 저장되어있는 모든 문자열을 대문자로 변환하여 반환한다.
- String trim(): 문자열의 왼쪽 끝과 오른쪽 끝에 있는 공백을 없앤 결과를 반환한다. 이 때 문자열 중간에 있는 공백은 제거되지 않는다.
- static String valueOf(Object o): 지정된 값을 문자열로 변환하여 반환한다. 참조변수의 경우, toString()을 호출한 결과를 반환한다.
    - static String valueOf(boolean b)
    - static String valueOf(char c)
    - static String valueOf(int i)
    - static String valueOf(long l)
    - static String valueOf(float f)
    - static String valueOf(double d)  


### join()과 StringJoiner  

join()은 여러 문자열 사이에 구분자를 넣어서 결합한다.  
java.util.StringJoiner 클래스를 사용해서 문자열을 결합할 수도 있는데, 사용하는 방법은 간단하다.  
- String.join(’구분자’,붙이고 싶은 문자열): 여러 문자열 사이에 구분자를 넣어서 결합한다.
- java.util.StringJoiner 클래스: new StringJoiner(delimiter,prefix,suffix)와 같은 형식으로 지정 가능하다.
```java
String animals = "dog,cat,bear";
String[] arr = animals.split(",");

System.out.println(String.join("-", arr));  // dog-cat-bear

StringJoiner sj = new StringJoiner("/","[","]");
for(String s: arr){
    sj.add(s);
}

System.out.println(sj.toString);    // [dog/cat/bear]
```
<br>  

### String.format()
format()은 형식화된 문자열을 만들어내는 간단한 방법이다.
String.format(”서식 문자열”, 인자…) 형식으로 printf()하고 사용법이 완전히 똑같다.  

### 기본형 값을 String으로 변환  
기본형을 문자열로 변경하는 가장 간단한 방법은 빈 문자열""을 더해주는 것이다.  
이 외에도 valueOf()를 사용하는 방법도 있다. 
성능은 valueOf()가 더 좋다.  

### String을 기본형으로 변환  
String을 기본형으로 변환하는 방법은 valueOf()를 쓰거나 parseInt()를 사용하면 된다.  
원래 valueOf()의 반환타입은 int가 아니라 Integer인데, 곧 배울 오토박싱(auto-Boxing)에 의해 Integer가 int로 자동 변환된다.  
메서드의 이름을 통일하기 위해 valueOf()가 나중에 추가되었다. valueOf(String s)는 내부에서 그저 parseInt(String s)를 호출할 뿐이므로, 두 메서드는 반환 타입만 다른 같은 메서드이다.  
```text
parseInt(String s)의 리턴타입은 Primitive Type이다. valueOf(String s)의 리턴타입은 Wrapper class이다. 
```

parseInt()나 parseFloat()과 같은 메서드는 문자열에 공백 또는 문자가 포함되어 있는 경우 변환 시 예외(NumberFormatException)가 발생할 수 있으므로 주의해야 한다.  
그래서 문자열 양끝의 공백을 제거해주는 trim()을 습관적으로 같이 사용하기도 한다.  
```java
int val = Integer.parseInt(" 123 ".trim());     // 문자열 양 끝의 공백을 제거 후 변환
```
그러나 부호를 의미하는 '+'나 소수점을 의미하는 '.'와 float형 값을 의미하는 f와 같은 자료형 접미사는 허용된다. 단. 자료형에 알맞은 변환을 사용하는 경우에만 허용된다.  
<br>

## StringBuffer 클래스와 StringBuilder 클래스  
String 클래스는 인스턴스를 생성할 때 지정된 문자열을 변경할 수 없지만 StringBuffer클래스는 변경이 가능하다.  
내부적으로 문자열 편집이 가능한 버퍼(buffer)를 가지고 있으며, StringBuffer 인스턴스를 생성할 때 그 크기를 지정할 수 있다.  
이 때, 편집할 문자열의 길이를 고려하여 버퍼의 길이를 충분히 잡아주는 것이 좋다. 
편집 중인 문자열이 버퍼의 길이를 넘어서게 되면 버퍼의 길이를 늘려주는 작업이 추가로 수행되어야하기 때문에 작업효율이 떨어진다.  

StringBuffer클래스는 String 클래스와 유사한 점이 많다. 
StringBuffer 클래스는 클래스와 같이 문자열을 저장하기 위한 char형 배열의 참조변수 value를 인스턴스 변수로 선언해 놓고 있다.  

### StringBuffer의 생성자  
StringBuffer 클래스의 인스턴스를 생성할 때, 적절한 길이의 char형 배열이 생성되고, 이 배열은 문자열을 저장하고 편집하기 위한 공간(buffer)으로 사용된다.  
```java
// StringBuffer의 생성자  
public StringBuffer(int length){
    value = new char[length];
    shared = false;
}
public StringBuffer() { 
    this(16);
}
public StringBuffer(String str) {
    this(str.length() + 16);
    append(str);
}
```
버퍼의 크기를 지정하지 않을 경우, 버퍼의 크기는 16이 된다. 
또한 버퍼의 크기를 지정할 경우, 지정한 문자열의 길이보다 16이 더 크게 버퍼를 생성한다.  

StringBuffer인스턴스로 문자열을 다루는 작업을 할 때, 버퍼의 크기가 작업하려는 문자열의 길이보다 작을 때는 내부적으로 버퍼의 크기를 증가시키는 작업이 수행된다.  
배열의 길이는 변경될 수 없으므로 새로운 길이의 배열을 생성한 후에 이전 배열의 값을 복사해야 한다.  
```java
...
// 새로운 길이(newCapacity)의 배열을 생성한다. newCapacity는 정수값이다.
char[] newValue = new char[newCapacity];

// 배열 value의 내용을 배열 newValue로 복사한다.  
System.arraycopy(value, 0, newValue, 0, count);     // count는 문자열의 길이
value = newValue;   // 새로 생성된 배열의 주소를 참조변수 value에 저장.
```
이렇게 함으로써 StringBuffer클래스의 인스턴스 변수 value는 길이가 증가된 새로운 배열을 참조하게 된다.  

### StringBuffer의 변경  
String과 달리 StringBuffer는 내용을 변경할 수 있다.  
append()는 반환타입이 StringBuffer이기 때문에 자신의 주소를 반환한다. 
그래서 append(String s)메서드가 수행되면, StringBuffer에 새로운 문자열이 추가되고 StringBuffer 자신의 주소를 반환한다.  
이처럼 자신의 주소를 반환하기 때문에 append(String s)를 여러번 호출할 수 있다.  
```java
StringBuffer sb = new StringBuffer("abc");
sb.append("123").append("ZZ");
```
<br>

### StringBuffer의 비교  
StringBuffer 클래스는 equals() 메서드를 오버라이딩하지 않아서 equals 메서드를 사용해도 등가비교연산자(==)로 비교한 것과 같은 결과를 얻는다.  
반면에 toString()은 오버라이딩되어 있어서 StringBuffer인스턴스에 toString()을 호출하면, 담고있는 문자열을 String으로 반환한다.   
그래서 StringBuffer 인스턴스에 담긴 문자열을 비교하기 위해서는 StringBuffer인스턴스에 toString()을 호출해서 String 인스턴스를 얻은 다음, 여기에 equals()메서드를 사용해서 비교해야 한다.  
```java
// StringBugger의 비교
String s = sb.toString();   // sb와 sb2은 "abc" 문자열을 갖고 있다.
String s2 = sb2.toString();

System.out.println(s.equals(s2));   // true
```
<br>

### StringBuffer클래스의 생성자와 메서드  
- StringBuffer(): 16문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스를 생성한다.
- StringBuffer(int length): 지정된 개수의 문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스를 생성한다.
- StringBuffer(String str): 지정된 문자열 값(str)을 갖는 StringBugger 인스턴스를 생성한다.
- StringBuffer append(추가할 값): 매개변수로 입력된 값을 문자열로 변환하여 StringBuffer 인스턴스가 저장하고 있는 문자열의 뒤에 덧붙인다.
- int capacity(): StringBuffer 인스턴스의 버퍼크기를 알려준다. length()는 버퍼에 담긴 문자열의 길이를 알려준다.
- char charAt(int index): 지정된 위치(index)에 있는 문자를 반환한다.
- StringBuffer delete(int start, int end): 시작위치부터 끝 위치 사이에 있는 문자를 제거한다. 단, 끝위치의 문자는 제외
- StringBuffer deleteCharAt(int index): 지정된 위치(index)의 문자를 제거한다.
- StringBuffer insert(int pos, 추가할 값): 두번째 매개변수로 받은 값을 문자열로 변환하여 지정된 위치(pos)에 추가한다. pos는 0부터 시작한다.
- int length(): StringBuffer 인스턴스에 저장되어 있는 문자열의 길이를 반환한다.
- StringBuffer replace(int start, int end, String str): 지정된 범위(start~end)의 문자들을 주어진 문자열로 바꾼다. end위치의 문자는 범위에 포함되지 않는다.
- StringBuffer reverse(): StringBuffer 인스턴스에 저장되어 있는 문자열의 순서를 거꾸로 나열한다.
- void setCharAt(int index, char ch): 지정된 위치의 문자를 주어진 문자(ch)로 바꾼다.
- void setLength(int newLength): 지정된 길이로 문자열의 길이를 변경한다. 길이를 늘리는 경우에 나머지 빈 공간을 널문자’\u0000’로 채운다.
- String toString(): StringBuffer 인스턴스의 문자열을 String으로 반환한다.
- String substring(int start): 지정된 범위 내의 문자열을 String으로 뽑아서 반환한다.
- String substring(int start, int end): 지정된 범위 내의 문자열을 String으로 뽑아서 반환한다.  
<br>

### StringBuilder
StringBuffer는 멀티쓰레드에 안전하도록 동기화되어 있다. 
동기화가 StringBuffer의 성능을 떨어뜨린다. 
따라서, 멀티쓰레드로 작성된 프로그램이 아닌 경우, StringBuffer의 동기화는 불필요하게 성능만 떨어뜨린다.   
그래서 StringBuffer에서 쓰레드의 동기화만 뺀 StringBuilder가 새로 추가되었다. 
StringBuilder는 StringBuffer와 완전히 똑같은 기능으로 작성되어 있어서, 소스코드에서 StringBuffer 대신 StringBuilder를 사용하도록 바꾸기만 하면 된다.  

<br>

### Math 클래스  
Math 클래스는 기본적인 수학계산에 유용한 메서드로 구성되어 있다.  
Math 클래스의 생성자는 접근 제어자가 private이기 때문에 다른 클래스에사 Math 인스턴스를 생성할 수 없다.  
그 이유는 클래스 내에 인스턴스 변수가 하나도 없어서 인스턴스를 생성할 필요가 없기 때문이다.  
Math 클래스의 메서드는 모두 static이며, 아래와 같이 2개의 상수만 정의해 놓았다.  
```java
public static final double E = 2.718281828...;  // 자연로그의 밑
public static final double PI = 3.14159265...;  // 원주율
```
<br>

### 올림, 버림, 반올림
- 올림, 버림, 반올림
    - 올림
        - ceil(숫자)
    - 버림
        - floor(숫자)
    - 반올림
        - round(숫자): 항상 소수점 첫째자리에서 반올림을 해서 정수값(int, long)을 결과로 반환한다. 따라서, 원하는 자리 수에서 반올림된 값을 얻기 위해서는 간단히 10의 n제곱으로 곱한 후, 다시 실수형(float,double)의 곱한 수로 나눠주기만 하면 된다. ex) round(9075.52)/100.0=90.76
        - rint(숫자): round()처럼 소수점 첫째 자리에서 반올림하지만, 반환값이 double이다. rint()는 두 정수의 정가운데 있는 값(*.5)은 가장 가까운 짝수 정수를 반환한다.    

### 예외를 발생시키는 메서드  
메서드 이름에 `Exact`가 포함된 메서드들이 JDK1.8부터 새로 추가되었다. 이들은 정수형간의 연산에서 발생할 수 있는 오버플로우(overflow)를 감지하기 위한 것이다.  
- int addExact(int x, int y): x+y
- int subtractExact(int x, int y): x - y
- int multiplyExact(int x, int y): x * y
- int incrementExact(int a): a++
- int decrementExact(int a): a--
- int negateExact(int a): -a
- int toIntExact(long value): (int)value - int로의 형변환  

연산자는 단지 결과를 반환할 뿐, 오버플로우의 발생여부에 대해 알려주지 않는다. 그러나 위의 메서드들은 오버플로우가 발생하면, 예외(ArithmeticException)를 발생시킨다.   


### Math 클래스의 메서드  
- static double abs(숫자): 주어진 값의 절대값을 반환한다.
- static double ceil(숫자): 주어진 값을 올림하여 반환한다.
- static double floor(숫자): 주어진 값을 버림하여 반환한다.
- static 숫자형(double,float,int,long) max(숫자1, 숫자2): 주어진 두 값을 비교하여 큰 쪽을 반환한다.
- static 숫자형 min(숫자1, 숫자2): 주어진 두 값을 비교하여 작은 쪽을 반환한다.
- static double random(): 0.0~1.0 범위(1.0은 미포함)의 임의의 double 값을 반환한다.
- static double rint(double a): 주어진 double값과 가까운 정수값을 double형으로 반환한다. 단, 두 정수의 정가운데 있는 값(*.5)은 짝수를 반환한다.
- static long round(double a): 소수점 첫째자리에서 반올림한 정수값(long)을 반환한다. 매개변수의 값이 음수인 경우, round()와 rint()의 결과가 다르다는 것에 주의하자.  
- static double sqrt(double a): 제곱근. sqrt는 square root를 의미하며 제곱근을 뜻한다.
- static double pow(double a, double b): a의 b승. pow는 power를 의미하며 거듭제곱을 뜻한다.  
- static double log(double a): 자연로그 함수. 밑이 자연대수인 e인 함수이다.
- static double log10(double a): 밑이 10인 로그 함수.


### Wrapper 클래스  
자바가 객체지향 언어임에도 8개의 기본형을 객체로 다루지 않는 이유는 높은 성능을 위해서이다.  
때로는 기본형(primitive type) 변수도 객체로 다뤄야 하는 경우가 있다. 예를 들면, 매개변수로 객체를 요구할 때, 기본형 값이 아닌 객체로 저장해야할 때 등이 있다.  
이 때 사용되는 것이 wrapper class 이다. 
8개의 기본형을 대표하는 8개의 래퍼클래스를 이용하면 기본형 값을 객체로 다룰 수 있다.  

래퍼 클래스의 생성자는 매개변수로 문자열이나 각 자료형의 값들을 인자로 받는다. 
이때 주의해야할 것은 생성자의 매개변수로 문자열을 제공할 때, 각 자료형에 알맞은 문자열을 사용해야 한다는 것이다.  
이처럼 래퍼 클래스는 객체생성 시에 생성자의 인자로 주어진 각 자료형에 알맞은 값을 내부적으로 저장하고 있으며, 이에 관련된 여러 메서드가 정의되어 있다.  

|기본형|래퍼클래스|생성자|
|:---:|:---:|:---|
|boolean|Boolean|Boolean(boolean value)<br>Boolean(String s)|
|char|**Character**|Character(char value)|
|byte|Byte|Byte(byte value)<br>Byte(String s)|
|short|Short|Short(short value)<br>Short(String s)|
|int|**Integer**|Integer(int value)<br>Integer(String s)|
|long|Long|Long(long value)<br>Long(String s)|
|float|Float|Float(double value)<br>Float(float value)<br>Float(String s)|
|double|Double|Double(double value)<br>Double(String s)|
생성자는 deprecated된 상태로 대신 valueOf() 메서드를 사용하는 것이 더 적절하다고 작성되어 있다.  

래퍼 클래스는 모두 equals()가 오버라이딩되어 있어서 주소값이 아닌 객체가 가지고 있는 값을 비교한다. 
오토박싱이 된다고 해도 비교연산자를 사용할 수 없다. 대신 compareTo()를 제공한다.  
그리고 toString()도 오버라이딩되어 있어서 객체가 가지고 있는 값을 문자열로 변환하여 반환한다.  
이 외에도 래퍼 클래스들은 MAX_VALUE, MIN_VALUE, SIZE, BYTES, TYPE 등의 static 상수를 공통적으로 가지고 있다.  
```java
System.out.println(Integer.MAX_VALUE);  // 2147483647
System.out.println(Integer.MIN_VALUE);  // -2147483648
System.out.println(Integer.SIZE);       // 32
System.out.println(Integer.BYTES);      // 4
System.out.println(Integer.TYPE);       // int
```
<br>

### Number 클래스  
Number 클래스는 추상클래스로 내부적으로 숫자를 멤버변수로 갖는 래퍼 클래스의 조상이다.  
아래의 그림은 래퍼 클래스의 상속계층도인데, 기본형 중에서 숫자와 관련된 래퍼 클래스들은 모두 Number 클래스의 자손이라는 것을 알 수 있다.   
![image](https://user-images.githubusercontent.com/54930365/213881431-1f3a7264-df19-496d-8313-c1e5fa434f67.png)

Number 클래스의 자손으로 BigInteger와 BigDecimal 등이 있는데, BigInteger는 long으로도 다룰 수 없는 큰 범위의 정수를, BigDecimal은 double로도 다룰 수 없는 큰 범위의 부동 소수점수를 처리하기 위한 것으로 연산자의 역할을 대신하는 다양한 메서드를 제공한다.   

### 문자열을 숫자로 변환하기  
- 문자열 -> 기본형:
  `타입.parse타입(String s)`
- 문자열 -> 래퍼 클래스:
  `타입.valueOf(String s)`

두 메서드 모두 숫자로 바꿔주는 일을 하지만, `타입.parse타입(String s)`은 반환값이 기본형이고 `타입.valueOf(String s)`는 반환값이 래퍼 클래스 타입이라는 차이가 있다.  

JDK 1.5부터 도입된 `오토박싱(autoboxing)` 기능 때문에 반환값이 기본형일 때와 래퍼 클래스일 때의 차이가 없어졌다. 
그래서 그냥 구별없이 valueOf()를 쓰는 것도 괜찮은 방법이다. 
단, 성능은 valueOf()가 조금 더 느리다.  

문자열이 10진수가 아닌 다른 진법(radix)의 숫자일 때도 변환이 가능하독 다음과 같은 메서드가 제공된다.  
```java
static int parseInt(String s, int radix)    // 문자열 s를 radix 진법으로 인식
static int valueOf(String s, int radix)     // 문자열 s를 radix 진법으로 인식
```
<br>

### 오토박싱 & 언박싱(autoboxing & unboxing)  
JDK 1.5 이전에는 기본형과 참조형 간의 연산이 불가능했기 때문에, 래퍼 클래스로 기본형을 객체로 만들어서 연산해야 했다.   
그러나 이제는 기본형과 참조형 간의 덧셈이 가능하다. 
이는 컴파일러가 자동으로 변환하는 코드를 넣어주기 때문에 가능해졌다.  
```java
int sum = i + iObj;             // 컴파일 전의 코드
int sum = i + iObj.intValue();  // 컴파일 후의 코드   
```
이 외에도 내부적으로 객체 배열을 가지고 있는 Vector 클래스나 ArrayList 클래스에 기본형 값을 저장해야할 때나 형변환이 필요할 때도 컴파일러가 자동적으로 코드를 추가해 준다.   
기본형 값을 래퍼 클래스의 객체로 자동 변환해주는 것을 `오토박싱(autoboxing)`이라고 하고, 
반대로 변환하는 것을 `언박싱(unboxing)`이라고 한다.  
```java
ex) 오토박싱과 언박싱  
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(10);               // 오토박싱. 10 -> new Integer(10)
int value = list.get(0);    // 언박싱. new Integer(10) -> 10
```

이처럼 개발자가 간략하게 쓴 구문을 컴파일러가 원래의 구문으로 변경해 주기 때문에, 
기본형과 참조형 간의 형변환, 참조형 간의 연산이 가능하다.  

<br>

### 유용한 클래스  
java.util 패키지에는 많은 수의 클래스가 있지만 실제로 자주 쓰이는 것들은 그렇게 많지 않기 때문에 중요한 클래스들만을 골라서 다양한 용도로 활용하는 방법을 보여주고자 한다.  

### java.util.Objects 클래스  
Object 클래스의 보조 클래스로 모든 메서드가 `static`이다. 
객체의 비교나 널체크에 유용하다.  
- static boolean isNull(Object obj): null이면 true를 반환한다.
- static boolean nonNull(Object obj): null이 아니면 true를 반환한다.  
- static <T> T requireNonNull(T obj): 객체가 null이 아니어야 하는 경우에 사용한다. null이면 NullPointerException을 발생시킨다.
- static <T> T requireNonNull(T obj, String message): 위와 동일하다. 문자열은 예외의 메시지가 된다.
- static <T> T requireNonNull(T obj, Supplier<String> messageSupplier): 위와 동일하다.
  다음의 세 줄을 requireNonNull()의 호출만으로 간단히 끝낼 수 있다.
    ```java
    if(name == null)
        throw new NullPointerException("name must not be null.");
    this.name = name;
    
    // 위의 세 줄을 requireNonNull()을 호출하여 간단하게 한 줄로 해결 가능하다.
    this.name = Objects.requireNonNull(name, "name must not be null.");
    ```
- static int compare(Object a, Object b, Comparator c): 두 비교대상이 같으면 0, 크면 양수, 작으면 음수를 반환한다. Comparator은 두 객체를 비교하는 기준이 된다.
- static boolean equals(Object a, Object b): Object 클래스에 정의된 equals()의 기능 + null 검사
- static boolean deepEquals(Object a, Object b): 객체를 재귀적으로 비교하기 때문에 다차원 배열의 비교도 가능하다.  
- static String toString(Object o): Object 클래스에 정의된 toString()의 기능 + null 검사
- static String toString(Object o, String nullDefault): Object 클래스에 정의된 toString()의 기능 + null 검사 + o가 null일 경우 대신 사용할 값을 nullDefault에 지정할 수 있다.

static import문을 사용하더라도 Object 클래스의 메서드와 이름이 같은 것들은 충돌이 난다. 즉, 컴파일러가 구별을 못한다. 
그럴 때는 클래스의 이름을 붙여줄 수밖에 없다.  

### java.util.Random 클래스  
난수를 얻는 방법을 생각하면 Math.random()이 떠오를 것이다. 
이 외에도 Random클래스를 사용하면 난수를 얻을 수 있다. 
사실 Math.random()은 내부적으로 Random 클래스의 인스턴스를 생성해서 사용하는 것이므로 둘 중에서 편한 것을 사용하면 된다.  

Math.random()과 Random의 가장 큰 차이점이라면, 종자값(seed)을 설정할 수 있다는 것이다. 
종자값이 같은 Random 인스턴스들은 시스템이나 실행시간 등에 관계없이 항상 같은 난수를 순서대로 반환한다.  

### Random 클래스의 생성자와 메서드  
- Random(): 현재시간을 종자값(seed)으로 이용하는 Random 인스턴스를 생성한다.
- Random(long seed): 매개변수 seed를 종자값으로 하는 Random인스턴스를 생성한다.
- boolean nextBoolean(): boolean 타입의 난수를 반환한다.
- void nextBytes(byte[] bytes): bytes 배열에 byte 타입 난수를 채워서 반환한다.  
- double nextDouble(): double 타입의 난수를 반환한다. (0.0 <= x <1.0)
- float nextFloat(): float 타입의 난수를 반환한다. (0.0 <= x < 1.0)
- int nextInt(): int 타입의 난수를 반환한다. (int의 범위)
- int nextInt(int n): 0~n의 범위에 있는 int 값을 반환한다. (n은 범위에 포함되지 않음)
- long nextLong(): long 타입의 난수를 반환한다. (long 범위)
- void setSeed(long seed): 종자값을 주어진 seed 값으로 변경한다.  


### 정규식(Regular Expression) - java.util.regex 패키지  
정규식이란 텍스트 데이터 중에서 원하는 조건(패턴)과 일치하는 문자열을 찾아내기 위해 사용하는 것으로 미리 정의된 기호와 문자를 이용해서 작성한 문자열을 말한다.  
정규식을 이용하면 많은 양의 텍스트 파일 중에서 원하는 데이터를 손쉽게 뽑아낼 수도 있고 입력된 데이터가 형식에 맞는지 체크할 수도 있다.  

다음은 data라는 문자열 배열이 담긴 문자열 중에서 지정한 정규식과 일치하는 문자열을 출력하는 예제이다. 
Pattern은 정규식을 정의하는데 사용되고 Matcher는 정규식(패턴)을 데이터와 비교하는 역할을 한다.  
```java
String[] data = {"bat", "baby", "cA", "ca", "co", "c.", "c0", "car", "combat", "count", "date"};

// 1. 정규식을 매개변수로 Pattern 클래스의 static 메서드인 Pattern compile(String regex)을 호출하여 Pattern 인스턴스를 얻는다.
Pattern p = Pattern.compile("c[a-z]*");     // c로 시작하는 소문자영단어

for(int i=0; i<data.length; i++){
    // 2. 정규식으로 비교할 대상을 매개변수로 Pattern 클래스의 Matcher matcher(CharSequence input)를 호출해서 Matcher 인스턴스를 얻는다.
    Matcher m = p.matcher(data[i]);
    
    // 3. Matcher 인스턴스에 boolean matches()를 호출해서 정규식에 부합하는지를 확인한다.
    if(m.matches()){
        System.out.print(data[i] + ", ");
    }
}
```
```text
[실행결과]
ca, co, car, combat, count, 
```
<br>

자주 사용되는 정규식 패과 설명은 다음과 같다.  

| 정규식 패턴                                 | 설명                                                                                                |
|:---------------------------------------|:--------------------------------------------------------------------------------------------------|
| c[a-z]*                                | c로 시작하는 영단어                                                                                       |
| c[a-z]                                 | c로 시작하는 두 자리 영단어                                                                                  |
| c[a-zA-Z]                              | c로 시작하는 두 자리 영단어 (대소문자 구분안함)                                                                      |
| c[a-zA-Z0-9]<br>c\w                    | c로 시작하고 숫자와 영어로 조합된 두 글자                                                                          |
| .*                                     | 모든 문자열                                                                                            |
| c.                                     | c로 시작하는 두 자리 문자열                                                                                  |
| c.*                                    | c로 시작하는 모든 문자열(기호 포함)                                                                             |
| c\\.                                   | c.와 일치하는 문자열   '.'은 패턴작성에 사용되는 문자이므로 escape 문자인 '\'를 사용해야 된다.                                     |
| c\d<br>c[0-9]                          | c와 숫자로 구성된 두 자리 문자열                                                                               |
| c.*t                                   | c로 시작하고 t로 끝나는 모든 문자열                                                                             |
| [b&#x7c;c].* <br>[bc].* <br>[b-c].*    | b 또는 c로 시작하는 문자열                                                                                  |
| [^b&#x7c;c].* <br>[^bc].* <br>[^b-c].* | b 또는 c로 시작하지 않는 문자열                                                                               |
| .*a.\*                                 | a를 포함하는 모든 문자열 <br> *: 0 또는 그 이상의 문자                                                              |
| .*a.+                                  | a를 포함하는 모든 문자열   <br> +: 1 또는 그 이상의 문자. '+'는 '*'과는 달리 반드시 하나 이상의 문자가 있어야 하므로 a로 끝나는 단어는 포함되지 않았다. |
| [b&#x7c;c].{2}                         | b 또는 c로 시작하는 세 자리 문자열   <br>(b 또는 c 다음에 두 자리이므로 모두 세 자리)                                          |
| 0\\\d{1,2}                             | 0으로 시작하는 최소 2자리 최대 3자리 숫자(0포함)                                                                    |
| \\\d{3,4}                              | 최소 3자리 최대 4자리의 숫자                                                                                 |
| \\\d{4}                                |4자리의 숫자|
| (0\\\d{1,2})-(\\\d{3,4})-(\\\d{4})     | 정규식의 일부를 괄호로 나누어 묶어서 그룹화할 수 있다.<br>그리고 그룹화된 부분은 group(int i)를 이용해서 나누어 얻을 수 있다.|
<br>

```java
ex) 정규식 예제
String source = "A broken hand works, but not a broken heart.";
String pattern = "broken";
StringBuffer sb = new StringBuffer();

Pattern p = Pattern.compile(pattern);
Matcher m = p.matcher(source);

int i = 0;
while(m.find()){
    System.out.println(++i + "번째 매칭:" + m.start() + "~" + m.end());
    m.appendReplacement(sb, "drunken");     // broken을 drunken으로 치환하여 sb에 저장한다.
}

m.appendTail(sb);   // 마지막으로 치환된 이후의 부분을 sb에 덧붙인다.  
System.out.println("Replacement count : " + i);
System.out.println("result:" + sb.toString());
```
```text
[실행결과]
1번째 매칭:2~8
2번째 매칭:31~37
Replacement count : 2
result:A drunken hand works, but not a drunken heart.
```
Matcher의 find()로 정규식과 일치하는 부분을 찾으면, 그 위치를 start()와 end()로 알아 낼 수 있고 appendReplacement(StringBuffer sb, String replacement)를 이용해서 원하는 문자열(replacement)로 치환할 수 있다. 
치환된 결과는 StringBuffer인 sb에 저장되는데, sb에 저장되는 내용을 단계별로 살펴보면 이해하기 쉬울 것이다.  


### java.util.Scanner 클래스  
Scanner는 화면, 파일, 문자열과 같은 입력소스로부터 문자데이터를 읽어오는데 도움을 줄 목적으로 JDK1.5부터 추가되었다. 
Scanner에는 다음과 같은 생성자를 지원하기 때문에 다양한 입력소스로부터 데이터를 읽을 수 있다.  
- Scanner(String source)
- Scanner(File source)
- Scanner(InputStream source)
- Scanner(Readable source)
- Scanner(ReadableByteChannel source)
- Scanner(Path source)  

또한 Scanner는 정규식 표현을 이용한 라인단위의 검색을 지원하며 구분자에도 정규식 표현을 사용할 수 있어서 복잡한 형태의 구분자도 처리가 가능하다.  
- Scanner useDelimiter(Pattern pattern)
- Scanner useDelimiter(String pattern)

입력의 변천사는 다음과 같다.
```text
// JDK1.5 이전
BufferReader br = new BufferedReader(new InputStreamReader(System.in));
String input = br.readLine();

// JDK1.5 이후(java.util.Scanner)
Scanner s = new Scanner(System.in);
String input = s.nextLine();

// JDK1.6 이후(java.io.Console) - 이클립스 등 IDE에서 잘 동작하지 않는다. Scanner와 성능 측면에서 거의 같다.
Console console = System.console();
String input = console.readLine();
```
<br>

Scanner에서는 다음과 같은 메서드를 제공하으로써 입력받은 문자를 다시 변환하는 수고를 덜어 준다.  
- boolean nextBoolean()
- byte nextByte()
- short nextShort()
- int nextInt()
- long nextLong()
- double nextDouble()
- float nextFloat()
- String nextLine()


### java.util.StringTokenizer 클래스  
StringTokenizer는 긴 문자열을 지정된 구분자를 기준으로 토큰이라는 여러 개의 문자열로 잘라내는 데 사용된다.   
StringTokenizer를 이용하는 방법 이외에도 String의 split(String regex)이나 Scanner의 useDelimiter(String pattern)를 사용할 수도 있지만, 
이 두 가지 방법은 정규식 표현을 사용해야하므로 정규식 표현에 익숙하지 않은 경우 StringTokenizer를 사용하는 것이 간단하면서도 명확한 결과를 얻을 수 있을 것이다.  
```java
String[] result = "100,200,300,400".split(",");
Scanner sc2 = new Scanner("100,200,300,400").useDelimiter(",");
```

그러나 StringTokenizer는 구분자로 단 하나의 문자 밖에 사용하지 못하기 때문에 보다 복잡한 형태의 구분자로 문자열을 나누어야 할 때는 어쩔 수 없이 정규식을 사용하는 메서드를 사용해야 할 것이다.   

### StringTokenizer의 생성자와 메서드  
StringTokenizer의 주로 사용되는 생성자와 메서드는 다음과 같다.  
- StringTokenizer(String str, String delim): 문자열(str)을 지정된 구분자(delim)로 나누는 StringTokenizer를 생성한다. (구분자는 토큰으로 간주되지 않음)
- StringTokenizer(String str, String delim, boolean returnDelims): 문자열(str)을 지정된 구분자(delim)로 나누는 StringTokenizer를 생성한다. returnDelims의 값을 true로 하면 구분자도 토큰으로 간주된다.
- int countTokens(): 전체 토큰의 수를 반환한다.
- boolean hasMoreTokens(): 토큰이 남아있는지 알려준다.
- String nextToken(): 다음 토큰을 반환한다.  

다음은 ','를 구분자로 하는 StringTokenizer를 생성해서 문자열을 나누어 출력하는 예제이다.  
```java
ex) StringTokenizer 예제
String source = "100,200,300,400";
StringTokenizer st = new StringTokenizer(source, ",");

while(st.hasMoreTokens()){
    System.out.println(st.nextToken());
}
```
```text
[실행결과]
100
200
300
400
```

주의할 것은 StringTokenizer는 단 한 문자의 구분자만 사용할 수 있기 때문에, `new StringTokenizer(source, "+-*/=()")` 이와 같이 여러 문자들을 구분자로 지정할 경우, 전체가 하나의 구분자가 아니라 각각의 문자가 모두 구분자가 된다.  

split()은 빈 문자열도 토큰으로 인식하는 반면 StringTokenizer는 빈 문자열을 토큰으로 인식하지 않는다. 
이 외에도 성능의 차이가 있다. split()은 데이터를 토큰으로 잘라낸 결과를 배열에 담아서 반환하기 때문에 데이터를 토큰으로 바로바로 잘라서 반환하는 StringTokenizer보다 성능이 떨어질 수밖에 없다. 
그러나 데이터의 양이 많은 경우가 아니라면 별 문제가 되지 않으므로 크게 신경 쓸 부분은 아니다.   

### java.math.BigInteger 클래스  
정수형으로 표현할 수 있는 값에는 한계가 있다. 
가장 큰 정수형 타입인 long으로 표현할 수 있는 값은 10진수로 19자리 정도이다. 
이보다 더 큰 값을 다뤄야할 때 사용하면 좋은 것이 BigInteger이다.  
BigInteger는 내부적으로 int 배열을 사용해서 값을 다룬다. 
그래서 long 타입보다 훨씬 큰 값을 다룰 수 있는 것이다. 
대신 성능은 long 타입보다 떨어질 수밖에 없다.  

BigInteger는 불변이다. 그리고 모든 정수형이 그렇듯이 BigInteger 역시 값을 '2의 보수'의 형태로 표현한다.  
```text
final int signum;   // 부호. 1(양수), 0, -1(음수) 셋 중 하나
final int[] mag;    // 값(magnitude)
```
좀 더 자세히 말하면, 위의 코드에서 알 수 있듯이 부호를 따로 저장하고 배열에는 값 자체만 저장한다. 
그래서 signum의 값이 -1, 즉 음수인 경우, 2의 보수법에 맞게 mag의 값을 변환해서 처리한다. 
그래서 부호만 다른 두 값의 mag는 같고 signum은 다르다.  

- BigInteger의 생성
  - BigInteger val = new BigInteger("12323523343435242");    // 문자열로 생성
  - BigInteger val = new BigInteger("FFFF", 16);            // 지정된 진법(radix)의 문자열로 변환
  - BigInteger val = new BigInteger.valueOf(1234567890L);   // 숫자로 생성
<br>
- 다른 타입으로의 변환  
  - String toString(): 문자열로 변환
  - String toString(int radix): 지정된 진법(radix)의 문자열로 변환
  - byte[] toByteArray(): byte배열로 변환
  - int intValue(): int 기본형으로 변환
  - long longValue(): long 기본형으로 변환
  - float floatValue(): float 기본형으로 변환
  - double doubleValue(): double 기본형으로 변환
  - byte  byteValueExact(): byte 기본형으로 변환. 이름 끝에 'Exact'가 붙은 것들은 변환한 결과가 변환한 타입의 범위에 속하지 않으면 ArithmeticException을 발생시킨다.  
  - int intValueExact(): int 기본형으로 변환. 변환한 결과가 변환한 타입의 범위에 속하지 않으면 ArithmeticException을 발생시킨다.
  - long longValueExact(): long 기본형으로 변환. 변환한 결과가 변환한 타입의 범위에 속하지 않으면 ArithmeticException을 발생시킨다.
    <br>
- BigInteger의 연산  
  - BigInteger add(BigInteger val): 덧셈(this + val)
  - BigInteger subtract(BigInteger val): 뺄셈(this - val)
  - BigInteger multiply(BigInteger val): 곱셈(this * val)
  - BigInteger divide(BigInteger val): 나눗셈(this / val)
  - BigInteger remainder(BigInteger val): 나머지(this % val)   
  ** BigInteger는 불변으로, 반환타입이 BigInteger란 얘기는 새로운 인스턴스가 반환된다는 뜻이다.  
    <br>
- 비트 연산 메서드  
  - int boolean bitCount(): 2진수로 표현했을 때, 1의 개수(음수는 0의 개수)를 반환
  - int bitLength(): 2진수로 표현했을 때, 값을 표현하는데 필요한 bit 수
  - boolean testBit(int n): 우측에서 n+1번째 비트가 1이면 true, 0이면 false
  - BigInteger setBit(int n): 우측에서 n+1번째 비트를 1로 변경
  - BigInteger clearBit(int n): 우측에서 n+1번째 비트를 0으로 변경
  - BigInteger flipBit(int n): 우측에서 n+1번째 비트를 전환(1->0, 0->1)
  ** 워낙 큰 숫자를 다루기 위한 클래스이므로, 성능을 향상시키기 위해 비트단위로 연산을 수행하는 메서드들을 많이 가지고 있다.  


### java.math.BigDecimal 클래스  
double 타입으로 표현할 수 있는 값은 상당히 범위가 넓지만, 정밀도가 최대 13자이 밖에 되지 않고 실수형의 특성상 오차를 피할 수 없다. 
BigDecimal은 실수형과 달리 정수를 이용해서 실수를 표현한다. 
앞에서 배운 것과 같이 실수의 오차는 10진 실수를 2진 실수로 정확히 변환할 수 없는 경우가 있기 때문에 발생하는 것이므로, 오차가 없는 2진 정수로 변환하여 다루는 것이다. 
실수를 정수와 10의 제곱의 곱으로 표현한다.   


