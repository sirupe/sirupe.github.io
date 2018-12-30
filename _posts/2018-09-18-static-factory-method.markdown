---
layout: post
title:  "[Effective java] 정적 팩토리 메소드 (Static factory method)"
date:   2018-09-18 14:40:15 +0900
categories: [java, Effective Java]
img: use-post/java.jpg
toc: true
---
## 정적 팩토리 메소드 (Static factory method) 란?
***
객체를 생성하는 기법중의 하나. 객체를 생성하는 public static method 를 선언한 것이다.

ex) `java.lang.Boolean` 클래스의 `valueOf()` 메소드
```java
public static Boolean valueOf(boolean b) {
    return (b ? TRUE : FALSE);
}
```


<br/><br/>
## 장점
***
1. 정적 팩터리 메서드에는 이름이 있다.
    
    ex) `java.math.BigInteger` 클래스의 `probableInteger()` 메소드
    
    ```java
    public static BigInteger probablePrime(int bitLength, Random rnd) {
        if (bitLength < 2)
            throw new ArithmeticException("bitLength < 2");
    
        return (bitLength < SMALL_PRIME_THRESHOLD ?
                smallPrime(bitLength, DEFAULT_PRIME_CERTAINTY, rnd) :
                largePrime(bitLength, DEFAULT_PRIME_CERTAINTY, rnd));
    }
    ```
    <br/>

2. 호출할 때마다 새로운 객체를 생성할 필요가 없다.

    ex) 상기의 `Booelan.valueOf()` 메소드
    
    <br/>


3. 반환값 자료형의 하위 자료형 객체를 반환할 수 있다.

    ex1) `java.util.EnumSet` 클래스 `noneOf()` 메소드. `EnumSet` 클래스를 상속받은 `RegularEnumSet` 클래스와 `JumboEnumSet` 클래스 중 조건에 따라 다른 클래스를 반환한다. 
    
    ```java
    class JumboEnumSet<E extends Enum<E>> extends EnumSet<E> {
        ...
    }
    ```
    ```java
    class RegularEnumSet<E extends Enum<E>> extends EnumSet<E> {
        ...
    }
    ```
    ```java
    public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
        Enum<?>[] universe = getUniverse(elementType);
        if (universe == null)
            throw new ClassCastException(elementType + " not an enum");
    
        if (universe.length <= 64)
            return new RegularEnumSet<>(elementType, universe);
        else
            return new JumboEnumSet<>(elementType, universe);
    }
    ```
    <br/>
    ex2) 서비스 제공자 프레임워크 (Service Provider Framework - JDBC 등) 의 근간이 됨.
    
    서비스 제공자 프레임워크의 구조
    
    1) 서비스 인터페이스
    ```java
    public class Service {
        // 서비스 고유의 메소드들이 이 자리에 온다.
    }
    ```
    2) 제공자 등록 API
    
    3) 서비스 접근 API
    ```java
    // 서비스 등록과 접근에 사용되는 객체 생성 불가능 클래스
    public class Services {
        // 객체 생성 방지
        private Services() {};
        
        // 서비스 이름과 서비스 간 대응관계 보관
        private static final Map<String, Provider> providers = new ConcurrentHashMap<>();
        public static final String DEFAULT_PROVIDER_NAME = "<def>";
        
        //제공자 등록 API
        public static void registerDefaultProvider(Provider p) {
            registerProvider(DEFAULT_PROVIDER_NAME, p);
        }
        
        public static void registerProvider(String name, Provider p) {
            providers.put(name, p);
        }
        
        // 서비스 접근 API
        public static Service newInstance() {
            return newInstance(DEFAULT_PROVIDER_NAME);
        }
        
        public static Service newInstance(String name) {
            Provider p = providers.get(name);
            if (p == null) {
                throw new IllegalArgumentException("No provider registered with name: " + name);
            }
            return p.newService();
        }
    }
    ```
    4) 서비스 제공자 인터페이스
    ```java
    public interface Provider {
        Service newService();
    }
    ```
<br/>

4. 형인자 자료형(parameterized type) 객체를 만들기가 수월하다.(단, 자바 1.7 부터 생성자를 호출할 때도 자료형 유추를 사용할 수 있도록 update 되었다. 아래와 같이 <mark>다이아몬드 연산자가 추가됨으로써 표준 컬렉션 메서드에서 정적 팩터리 메서드를 추가할 필요가 없어졌다.</mark>)
    ```java
    Map<String, List<String>> myMap = new HashMap<>();
    ```

<br/><br/>
## 문제점
***
1. public 이나 protected 로 선언된 생성자가 없으면 하위 클래스를 만들 수 없다.

2. 다른 정적 메서드와 확연히 구분되지 않는다.
