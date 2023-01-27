# CH10. 날짜와 시간 & 형식
## 날짜와 시간
### Calener와 Date  
JDK1.0부터 날짜와 시간을 다룰 목적으로 Date 클래스를 제공했다.  
Date 클래스의 빈약함을 보완하기 위해 JDK1.1부터 Calendar 클래스를 제공했다.  
Calendar 클래스의 단점을 보완하기 위해 JDK1.8부터 java.time 패키지를 제공했다.  

Calendar는 추상클래스이기 때문에 직접 객체를 생성할 수 없고, 메서드를 통해서 완전히 구현된 클래스의 인스턴스를 얻어야 한다.  
```java
Calendar cal = Calendar.getInstance();  // OK, getInstance() 메서드는 Calendar 클래스를 구현한 클래스의 인스턴스를 반환한다.
```
getInstance()를 통해 얻은 인스턴스는 기본적으로 현재 시스템의 날짜와 시간에 대한 정보를 담고 있다. 
원하는 날짜나 시간에 대한 정보를 담고 있다. 
원하는 날짜나 시간으로 설정하려면 set 메서드를 사용하면 된다. 
원하는 필드의 값을 가져오기 위해서는 'int get(int field)' 메서드를 사용하면 된다. 
get 메서드의 매개변수로 사용되는 int 값들은 Calendar에 정의된 static 상수이다. 
한 가지 주의해야할 것은 get(Calendar.MONTH)로 얻어오는 값의 범위가 0~11이라는 것이다.(0이면 1월을 의미)  

### Date와 Calendar간의 변환  
Calendar가 새로 추가되면서 Date는 대부분의 메서드가 `deprecated`되었으므로 잘 사용되지 않는다.  
그럼에도 불구하고 여전히 Date를 필요로 하는 메서드들이 있기 때문에 Calendar를 Date로 또는 그 반대로 변환할 일이 생긴다.  
```java
1. Calendar를 Date로 변환
Calendar cal = Calendar.getInstance();
        ...
Date d = new Date(cal.getTimeInMillis());   // Date(long date)
        
2. Date를 Calendar로 변환
Date d = new Date();
        ...  
Calendar cal = Calendar.getInstance();
cal.setTime(d);
```
