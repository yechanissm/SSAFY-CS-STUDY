# 싱글톤 패턴

## 싱글톤 패턴이란?

- 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴으로, 보통 하나의 클래스를 기반으로 여러 개의 개별적인 인스턴스를 만들 수 있지만, 그렇지 않고 하나의 클래스를 기반으로 단 하나의 인스턴스를 만드는 패턴이다.
- 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용하기에, 인스턴스를 생성할 때 드는 비용이 줄지만, 의존성이 높아지는 단점이 있다.

## 싱글톤 패턴은 보통 언제 사용하는 가?

1. 글로벌 설정 : 애플리케이션 전체에서 하나의 인스턴스로 상태를 관리하고 공유할 때 사용.
    
    예를 들어, 설정 정보, 환경 설정 등 싱글톤으로 구현하면 애플리케이션 어디서든 동일한 설정 정보를 참조 가능 ex) Configuration 클래스
    
2. 자원 관리 : 데이터베이스 연결 풀, 스레드 풀, 캐시, 로깅 등 제한된 자원을 관리하는 경우 싱글톤 패턴을 사용한다. 이런 자원들은 생성과 소멸에 비용이 많이 들기에 싱글톤 패턴처럼 하나의 인스턴스를 생성하여 재사용하는 것이 효율 적 ex) CacheManager, DBConnectionManager 클래스 등

1. 서비스 제공 : 특정 서비스 객체가 하나만 존재할 때 사용. ex) 이메일 발송 서비스, 알림 서비스

## 싱글톤 패턴을 구현하는 7가지 방법

1. **단순한 메서드 호출**
    
    ```java
    package com.example.cs.Singleton;
    
    public class Singleton {
    
        private static Singleton singleton;
    
        private Singleton() {
    
        }
    
        public static Singleton getInstance() { //호출이 되었을 때 인스턴스 생성
            if( singleton == null ) {
                singleton = new Singleton();
            }
            return singleton;
        }
    }
    ```
    
    싱글톤 패턴 생성 여부를 확인하고 싱글톤이 업으면 새로 만들고 있다면 만들어진 인스턴스 반환
    
    하지만, 이 방법은 메서드의 원자성이 결여되어 있다. 멀티스레드 환경에서 동작할 때, 동시에 여러 스레드가 
    
    ‘singleton’ 인스턴스를 초기화하려고 시도할 수 있다는 뜻이다. 즉 인스턴스를 2개 이상 만들 수 있다.
    
2. **sychronized** 
    
    ```java
    package com.example.cs.Singleton;
    
    public class Singleton {
    
        private static Singleton singleton;
    
        private Singleton() {
    
        }
    
        public static synchronized Singleton getInstance() { 
            if( singleton == null ) {
                singleton = new Singleton();
            }
            return singleton;
        }
    }
    
    ```
    
    synchronized를 사용하여 멀티스레드 환경에서 안전하게 지연 초기화를 구현한 방식으로, 최초로 접근한 스레드가 해당 메서드 호출 시 다른 스레드가 접근하지 못하도록 잠금(lock)을 걸어준다.
    
    이 때 멀티스레드 환경에서 안전하지만 getInstance()를  호출할 때마다 lock이 걸려 성능이 저하된다.
    
3. **정적 멤버** 
    
    ```java
    package com.example.cs.Singleton;
    
    public class Singleton {
    
        private static final Singleton singleton = new Singleton(); //정적멤버
    
        private Singleton() {
    
        }
    
        public static Singleton getInstance() { 
            return singleton;
        }
    }
    
    ```
    
    인스턴스 생성 과정이 최초 JVM이 클래스 로딩 때 모든 클래스들을 로드할 때 이용한 방법이다.
    
    클래스 로딩과 동시에 싱글톤 인스턴스를 만들어, 요청할 때 그냥 만들어진 인스턴스를 반환하면 된다. 
    
    이를 통해 synchronized 키워드 사용할 필요 없으며 스레드 안전성을 보장한다.
    
    하지만 불필요한 자원낭비가 있다. (호출 하지 않아도 무조건 최초에 만들어야 하기 때문)
    
4. **정적 블록**
    
    ```java
    package com.example.cs.Singleton;
    
    public class Singleton {
    
        private static Singleton instance = null;
    
        //정적 블록
        static {
            instance = new Singleton();
        }
    
        private Singleton() {}
    
        public static Singleton getInstance() {
            return instance;
        }
    }
    
    ```
    
    클래스가 로드될 때 정적 블록을 통해 인스턴스를 생성. 이 역시 위의 3번과 비슷
    
5. **정적 멤버와 Lazy Holder(중첩 클래스) - 가장 많이 사용 됨** 
    
    ```java
    package com.example.cs.Singleton;
    
    public class Singleton {
    
        private static class singleInstanceHolder {
    
            private static final Singleton INSTANCE = new Singleton();
        }
    
        public static Singleton getInstance() {
            return singleInstanceHolder.INSTANCE;
        }
    }
    
    ```
    
    singleInstanceHolder라는 내부 클래스를 하나 더 만듬으로써, Singleton 클래스가 최초에 로딩되더라도 함께 초기화가 되지 않고, getInstance()를 호출될 때 singleInstanceHolder라는 클래스가 로딩되어 인스턴스를 생성하게 됨
    
6. **이중 확인 잠금(DCL, Double Checked Locking)**
    
    ```java
    package com.example.cs.Singleton;
    
    public class Singleton {
    
        private volatile Singleton instance;
        private Singleton() {}
    
        public Singleton getInstance() {
            if (instance == null) {
                synchronized (Singleton.class) {
    		            //동기화 블록
                    if (instance == null) { // 체크 2번
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }
    }
    
    ```
    
    이 방식은 인스턴스를 지연 초기화하면서도 스레드 안정성을 보장한다. ‘volatile’ 키워드를 사용하여 인스턴스의 변경이 즉시 반영되도록 하며, 두 번의 null 체크와 동기화를 통해 불필요한 동기화를 최소화한다.
    
    volatile는 Java에 변수를 선언하면, 해당 변수의 읽기 및 쓰기 연산이 모두 메인 메로이세서 직접 수행되도록 보장하여, 이를 통해 변수 값을 인관성을 유지하고 스레드 간의 안정성을 보장한다.
    
7. **Enum**
    
    ```java
    package com.example.cs.Singleton;
    
    public enum SingletonEnum {
    
        INSTANCE;
        public void oortCloud() {}
    }
    
    ```
    
    기본적으로 스레드세이프한 점이 보장되기 때문에 이를 통해 생성할 수 있음
    

## 싱글톤 패턴의 단점

싱글톤 패턴은 TDD(Test Driven Development)를 할 때 걸림돌이 된다.. TDD를 할 때 단위 테스트를 주로 하는데, 단위 테스트는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 한다.

하지만 싱글톤 패턴은 미리 생성된 하나으 ㅣ인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 

‘독립적인’ 인스턴스를 만들기가 어렵다.

또한, 싱글톤 패텅은 사용하기 쉽고 실용적이지만 모듈 간의 결합을 강하게 만들 수 있다는 단점이 있다. 이를 해결하는 방법이 의존성 주입이다.