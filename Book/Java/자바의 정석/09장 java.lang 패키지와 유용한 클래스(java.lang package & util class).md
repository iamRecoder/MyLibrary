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
