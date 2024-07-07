# 전략 패턴

## 전략 패턴이란?

- 정책패턴이라고 불리며, 객체의 행위를 바꾸고 싶은 경우 직접 수정하지 않고 전력이라 부르는 캡슐화한 알고리즘을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만다는 패턴
- 객체의 행위(동작)을 클래스의 집합으로 정의하고, 이 동작을 캡슐화하여 동적으로 변경할 수 있도록 만든다.

## 전략 패턴의 장점과 단점

### 장점

1. 유연성과 확장성 : 전략 패턴을 사용하면 알고리즘을 독립적으로 쉽게 추가하거나 변경 가능. 새로운 전략 클래스를 추가하거나 기존의 전략 클래스를 수정하지 않고도 클라이언트 코드에서 동작을 변경
2. 코드 재사용성 : 각 전략은 독립적인 클래스로 구현되므로, 코드 재사용성 높음
3. 알고리즘의 독립성 : 각 전략은 캡슐화되어 있기 때문에 클라이언트 코드는 구체적인 알고리즘의 세부 사항을 알 필요가 없다. 이로 인해 클라이언트 콛와 알고리즘 간의 결합도가 낮아진다.
4. 테스트 용이성 : 전략 간의 교체가 쉬으므로 단위, 통합 테스트가 간편함

### 단점

1. 클라이언트가 선택해야 함 : 클라이언트는 사용할 전략을 명시적으로 선택해야 한다. 이로 인해 코드가 복잡해질 수 있음
2. 전략 클래스의 증가 : 많은 전략이 필요한 경우 클래스의 수가 증가

## 코드

```java
package com.example.cs.Singleton;

import java.util.ArrayList;
import java.util.List;

interface PaymentStrategy {
    public void pay(int amount);
}

class KAKAOCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpriy;

    public KAKAOCardStrategy(String name, String cardNumber, String cvv, String dateOfExpriy) {
        this.name = name;
        this.cardNumber = cardNumber;
        this.cvv = cvv;
        this.dateOfExpriy = dateOfExpriy;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using KAKAOCard");
    }
}

class LUNACardStrategy implements PaymentStrategy {
    private String emailId;
    private String password;

    public LUNACardStrategy(String emailId, String password) {
        this.emailId = emailId;
        this.password = password;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard");
    }
}

class Item {
    private String name;
    private int price;

    public Item(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
}

class ShoppingCart {
    List<Item> items;

    public ShoppingCart() {
        this.items = new ArrayList<>();
    }

    public void addItem(Item item) {
        this.items.add(item);
    }

    public void removeItem(Item item) {
        this.items.remove(item);
    }

    public int calculateTotal() {
        int sum = 0;
        for (Item item : items) {
            sum += item.getPrice();
        }
        return sum;
    }

    public void pay(PaymentStrategy paymentMethod) {
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}

public class Strategy {

    public static void main(String[] args) {

        ShoppingCart cart = new ShoppingCart();

        Item A = new Item("A", 100);
        Item B = new Item("B", 300);

        cart.addItem(A);
        cart.addItem(B);

        cart.pay(new LUNACardStrategy("dldpcks34@naver.com", "dldldl"));
        cart.pay(new KAKAOCardStrategy("lee", "123445", "123", "12/01"));
    }
}

```

## 전략 패턴은 언제 사용하는 가?

1. 결제 처리 : 결제 방식에 따라 다른 전략 사용 가능. 위의 코드와 같이 예를 들어, 신용카드 페이팔, 은행 송금 등의 결제 방식에 대한 각각의 전략을 정의하고 이를 사용자의 선택에 따라 처리

1. 이벤트 처리 : 이벤트 핸들링 시 각 이벤트 유형에 따라 다른 전략을 사용할 수 있다. 예를 들어, 클릭 이벤트, 드래그 앤 드롭 이벤트 등에 대한 전략을 정의하고 이를 구현

1. 자원 관리 : 자원(메모리, 파일 등)의 사용 방법에 따라 다른 전략을 적용한다.

## Passport의 전략 패턴

- passport는 Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리로, 여러 가지 ‘전략’을 기반으로 인증할 수 있게 한다. 서비스 내의 회원가입된 아이디와 비밀번호를 기반으로 인증하는 LocalStrategy 전략과 페이스북, 네이버 등 다른 서비스를 기반으로 인증하는 OAuth 전략 등을 지원한다.

```jsx
var passport = require('passport'),
LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
	function(username, password, done) {
		User.findOne({ username : username}, function (err, user) {
			if(err) {return done(err);}
				if((!user) {
					return done(null, false, {message : 'Incorrect username.'});
				}
				if(!user.validPassword(password)) {
					return done(null, false, {message : 'Incorrect password.'});
				}
				return done(null, user);
			});
		}
));
```