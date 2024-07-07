# 팩토리 패턴

## 팩토리 패턴이란?

- 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴
- 상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문에 더 많은 유연성을 갖게 된다. 객체 생성 로직이 따로 떼어져 있기 때문에 코드를 리팩터링 하더라도 한 곳만 고칠 수 있게 되니 유지 보수성이 증가한다.
- 팩토리 패턴은 객체 생성 로직을 분리하고, 코드의 유연성과 확장성을 높이기 위해 사용되는 패턴으로 유지보수성과 변화에 대응하기 쉽도록 설계

## 팩토리 패턴의 장점과 단점

### 장점

1. 객체 생성의 캡슐화 : 객체 생성 로직을 한 곳에 모아 코드의 가독성 증가
2. 코드 재사용성 : 객체 생성 로직을 재사용
3. 확장성  : 새로운 클래스를 추가 하더라도 기존 코드를 수정할 필요 없이 팩토리 클래스만 수정
4. 유연성 : 클라이언트 코드가 객체의 구체적인 클래스에 의존하지 않기 때문에 코드를 변경하기 쉬움

### 단점

1. 복잡성 증가 : 코드의 구조가 복잡해질 수 있다.
2. 추가 클래스 필요 : 팩토리 클래스를 별도로 작성해야 하므로 클래스 수가 증가
3. 추가 레이어 도입 : 팩토리 클래스가 또 다른 추상화 레이어를 도입하여 시스템의 복잡도 증가

## 코드

```java
package com.example.cs.Singleton;

abstract  class Coffee {
    public abstract  int getPrice();
    
    @Override
    public String toString() {
        return "Hi this coffee is " + this.getPrice();
    }
}

class CoffeeFactory {
    public static Coffee getCoffee(String type, int price) {
        if("Latte".equalsIgnoreCase(type)) return new Latte(price);
        else if("Americano".equalsIgnoreCase(type)) return new Americano(price);
        else {
            return new DefalueCoffee();
        }
    }
}

class DefaultCoffee extends Coffee {
    private int price;
    
    public DefaultCoffee() {
        this.price = -1;
    }
    
    @Override
    public int getPrice() {
        return this.price;
    }
}

class Latte extends Coffee {
    private int price;

    public Latte(int price) {
        this.price = price;
    }

    @Override
    public int getPrice() {
        return this.price;
    }
}

class Americano extends Coffee {
    private int price;

    public Americano(int price) {
        this.price = price;
    }

    @Override
    public int getPrice() {
        return this.price;
    }
}

public class Factory {

    public static void main(String[] args) {
        
        Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
        Coffee americano = CoffeeFactory.getCoffee("Americano", 3000);
        
        System.out.println("Factory latte ::"  + latte);
        System.out.println("Factory americano ::" + americano);
    }
}

```

## 팩토리 패턴은 언제 사용하는 가?

1. 로그인 처리 
    
    로그인 방식에 따라 다양한 인증 객체를 생성할 때 사용. 예를 들어 Oauth2를 사용하여 소셜 로그인(Google, Naver, Kakao) , 일반 로그인 등 다양한 방식의 인증 객체를 팩토리 패턴을 통해 생성
    
2. 데이터베이스 연결 
    
    애플리케이션에서 사용되는 데이터베이스 종류 (MySQL, PostgreSQL, Oracle 등) 에 따라 다른 커넥션 객체를 생성할 때 사용
    
3. UI 컴포넌트 생성 
    
    다양한 UI 컴포넌트 (버튼, 텍스트 필드, 라벨 등) 을 팩토리 패터을 통해 생성하여 코드의 일관성과 유연성을 유지