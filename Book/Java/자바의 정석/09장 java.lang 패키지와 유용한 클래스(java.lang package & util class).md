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
- String(StringBuffer buf): StringBuffer인스턴스가 갖고 있는 무자열과 같은 내용의 String 인스턴스를 생성한다.  
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