CH05. 배열
====
## 배열
배열은 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것을 의미한다.  

### 배열의 생성  
배열을 생성하기 위해서는 연산자 'new'와 함께 배열의 타입과 길이를 지정해 주어야 한다.  
```java
    타입[] 변수이름;  // 배열을 선언 (배열을 다루기 위한 참조변수 선언)
    변수이름 = new 타입[길이];  // 배열을 생성 (실제 저장공간을 생성)
```  

### 배열 길이와 인덱스   
인덱스는 배열의 요소마다 붙여진 일련번호로 각 요소를 구별하는데 사용된다. 
유효하지 않은 범위의 값을 index로 사용하면 실행 시에 에러(ArrayIndexOutOfBoundsException)가 발생한다.  

### 배열의 길이  
배열을 생성할 때 괄호[] 안에 배열의 길이를 적어줘야 하는데, 배열의 길이는 배열의 요소의 개수, 즉 값을 저장할 수 있는 공간의 개수다.  
배열의 길이는 양의 정수여야 하며 최대값은 int 타입의 최대값, 약 20억이다.  
길이가 0인 배열도 생성 가능하다.  
`ex) int[] arr = new int[0];`  

### 배열이름.length  
자바에서는 jVM이 모든 배열의 길이를 별도로 관리하며, `배열이름.length`를 통해서 배열의 길이에 대한 정보를 얻을 수 있다.  
배열은 한번 생성하면 길이를 변경할 수 없기 때문에, 이미 생성된 배열의 길이는 변하지 않는다.  
따라서 `배열이름.length`는 상수이다. 즉, 값을 읽을 수만 있을 뿐 변경할 수 없다.  

### 배열의 길이 변경하기  
배열은 한번 선언되고 나면 길이를 변경할 수 없다고 배웠는데, 그렇다면 배열에 저장할 공간이 부족한 경우에는 어떻게 해야 할까?  
```text
배열의 길이를 변경하는 방법: 
1. 더 큰 배열을 새로 생성한다.  
2. 기존 배열의 내용을 새로운 배열에 복사한다.
```
이러한 작업들은 꽤나 비용이 많이 들기 때문에, 처음부터 배열의 길이를 넉넉하게 잡아줘서 새로 배열을 생성해야하는 상황이 가능한 적게 발생하도록 해야 한다.  

### 배열의 초기화  
배열은 생성과 동시에 자동적으로 자신의 타입에 해당하는 기본값으로 초기화되므로 배열을 사용하기 전에 따로 초기화를 해주지 않아도 된다.  
또한 다음과 같이 값을 지정하여 초기화할 수 있다.  
```java
// 배열의 선언과 생성을 같이 하는 경우
int[] score = new int[] {50, 60, 70, 80, 90};   // OK.
int[] score = {50, 60, 70, 80, 90}; // OK. new int[]를 생략 가능할 수 있음

// 배열의 선언과 생성을 따로 하는 경우
int[] score;        
score = new int[] {50, 60, 70, 80, 90}; // OK.
score = {50, 60, 70, 80, 90};   // ERROR. new int[]를 생략 가능할 수 없음

// 메서드의 매개변수로 배열을 받는 경우
int result = add(new int[] {50, 60, 70, 80, 90});   // OK.
int reault = add({50, 60, 70, 80, 90}); // ERROR. new int[]를 생략할 수 없음.
``` 

그리고 괄호{} 안에 아무 것도 넣지 않으면 길이가 0인 배열이 생성된다. 
참조변수의 기본 값은 null이지만, 배열을 가리키는 참조변수는 null대신 길이가 0인 배열로 초기화하기도 한다.  
```java
int[] score = new int[0];   // 길이가 0인 배열
int[] score = new int[]{};  // 길이가 0인 배열
int[] score = {};           // 길이가 0인 배얼. new int[]가 생략됨.
```

### 배열의 출력  
`Arrays.toString(배열이름)` 메서드를 사용하면 '[첫번째 요소, 두번째 요소, ...]'와 같은 형식의 문자열로 만들어서 반환한다.  
배열은 참조변수이므로 배열을 출력하면 '배열의 주소'가 출력될 것 같지만 실제로는 '타입@주소'의 형식으로 출력된다. 
'@'뒤에 나오는 16진수는 배열의 주소인데 실제 주소가 아닌 내부 주소이다.  
```java
int[] iArr = { 100, 95, 80, 70, 60};
System.out.println(Arrays.toString(iArr));  // [100, 95, 80. 70, 60]이 출력된다. 'import java.util.*' 필요.
System.out.println(iArr);                   // [[I@14318bb와 같은 형식의 문자열이 출력된다.
```

예외적으로 char 배열은 println 메서드로 출력하면 각 요소가 구분자 없이 그대로 출력된다. 
이것은 println 메서드가 char배열일 때만 이렇게 동작하도록 작성되었기 때문이다.  
```java
char[] chArr = {'a', 'b', 'c', 'd'};
System.out.println(chArr);  // abcd가 출력된다.
```  

### 배열의 복사
배열을 복사하는 방법은 두 가지가 있다.  
1. for문을 이용한 배열의 복사    
2. System.arraycopy()를 이용한 배열의 복사  
   ```java
    System.arraycopy(num, 0, newNum, 0, num.length);
    // num[0]에서 newNum[0]으로 num.length개의 데이터를 복사. 이때 복사하려는 내용보다 여유 공간이 적으면 에러(ArrayIndexOutOfBoundsException)가 발생한다.
    ```

for문 대신 System 클래스의 arraycopy()를 사용하면 보다 간단하고 빠르게 배열을 복사할 수 있다. 
for문은 배열의 요소 하나하나에 접근해서 복사하지만, arraycopy()는 지정된 범위의 값들을 한 번에 통째로 복사한다.  

<br>

## String 배열  
### String 배열 생성 및 초기화
```java
// String 배열 생성
String[] name = new String[3];  // 길이가 3인 String 배열 생성

// String 배열 생성 및 초기화
String[] name = new String[]{"Kim", "Park", "Yi"};
String[] name = {"Kim", "Park", "Yi"};  // new String[] 생략 가능
```
배열에 실제 객체가 아닌 객체의 주소가 저장되어 있다. 이처럼, 기본형 배열이 아닌 경우, 즉, 참조형 배열의 경우 배열에 저장되는 것은 객체의 주소이다.  

### char 배열과 String 클래스  
사실 문자열이라는 용어는 '문자를 연이어 늘어놓은 것'을 의미하므로 문자배열인 char 배열과 같은 뜻이다.  
__그런데 자바에서 char 배열이 아닌 String 클래스를 이용해서 문자열을 처리하는 이유는 String 클래스가 char 배열에 여러 가지 기능을 추가하여 확장한 것이기 때문이다.__ 
그래서 char 배열을 사용하는 것보다 String 클래스를 사용하는 것이 문자열을 다루기 더 편하다. 

객체지향개념이 나오기 이전의 언어들은 데이터와 기능을 따로 다루어지지만, 객체지향언어에서는 데이터와 그와 관련된 기능을 하나의 클래스에 묶어서 다룰 수 있게 한다. 
즉, 서로 관련된 것들끼리 데이터와 기능을 구분하지 않고 함께 묶는 것이다.  

__char 배열과 String 클래스의 한 가지 중요한 차이가 있는데, String 객체(문자열)는 읽을 수만 있을 뿐 내용을 변경할 수 없다는 것이다.__  

### String 클래스의 주요 메서드  
| 메서드                                | 설명                                                                                                    |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------|
| char charAt(int index)             | 문자열에서 해당 위치(index)에 있는 문자를 반환한다.                                                                      |
| int length()                       | 문자열의 길이를 반환한다.                                                                                        |
| String substring(int from, int to) | 문자열에서 해당 범위(from ~ to)에 있는 문자열을 반환한다. to는 범위에 포함되지 않음                                                 |
| boolean equals(Object obj)         | 문자열의 내용이 obj와 같은지 확인한다. 같으면 결과는 true, 다르면 false가 된다. 대소문자를 구분하지 않고 비교하려면 equalsIgnoreCase()를 사용해야 한다. |
| char[] toCharArray()               | 문자열을 문자배열(char[])로 변환해서 반환한다.                                                                         |

### 커멘드 라인을 통해 입력받기  
커맨드 라인을 통해 프로그램을 실행할 때 클래스 이름 뒤에 공백문자로 구분하여 여러 개의 문자열을 프로그램에 전달할 수 있다. 
커맨드라인에 매개변수를 입력하지 않으면 JVM이 null 대신 크기가 0인 배열을 생성해서 args에 전달하도록 구현되어 있다.  

<br>  

## 다차원 배열  
메모리의 용량이 허용하는 한, 차원의 제한은 없지만, 주로 1,2차원의 배열이 사용된다.  

### 2차원 배열의 선언과 인덱스  
```java
// 2차원 배열 선언하기
타입[][] 변수이름;
타입 변수이름[][];
타입[] 변수이름[];
```

2차원 배열의 각 요소에 접근하는 방법은 `배열이름[행index][열index]`이다.  

### 2차원 배열의 초기화  
2차원 배열도 괄호{}를 사용해서 생성과 초기화를 동시에 할 수 있다. 다만, 1차원 배열보다 괄호{}를 한번 더 써서 행별로 구분해 준다.  
```java
// 2차원 배열의 초기화 
int[][] arr = new int[][] {{1,2,3}, {4,5,6}};
int[][] arr = {{1,2,3}, {4,5,6}};   // new int[][] 생략 가능.
```

2차원 배열은 '배열의 배열'로 구성되어 있다. 즉, 여러 개의 1차원 배열을 묶어서 또 하나의 배열로 만든 것이다.  

### 가변 배열  
자바에서는 2차원 이상의 배열을 '배열의 배열' 형태로 처리한다는 사실을 이용하면 보다 자유로운 형태의 배열을 구성할 수 있다.  
2차원 이상의 다차원 배열을 생성할 때 전체 배열 차수 중 마지막 차수의 길이를 지정하지 않고, 추후에 각기 다른 길이의 배열을 생성함으로써 고정된 형태가 아닌 보다 유동적인 가변 배열을 구성할 수 있다.  
```java
// 가변 배열 예시
int[][] score = new int[3][];   // 두 번째 차원의 길이는 지정하지 않는다.
score[0] = new int[3];
score[1] = new int[4];
score[2] = new int[2];
```
score.length의 값은 여전히 5지만, 일반적인 2차원 배열돠 달리 score[0].length는 3, score[1].length는 4로 서로 다르다.  
가변 배열 역시 중괄호{}를 이용해서 생성과 초기화를 동시에 하는 것이 가능하다.  


