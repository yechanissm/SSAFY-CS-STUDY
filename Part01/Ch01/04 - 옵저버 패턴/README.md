# 옵저버 패턴

## 옵저버 패턴이란?

- 어떤 객체의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 패턴
- 객체간 1대다 의존 관계를 정의한다. 그 이유는 어떤 객체의 상태가 변할 때 옵저들에게 알려주기 때문에 일대다이다. 한 객체의 상태가 변할 때 이를 의존하는 다른 객체들이 자동으로 통지 받고 업데이트 될 수 있도록 하는 패턴으로, 주로 이벤트 핸들링 시스템, 데이터 변화 통지 시스템에 사용
- 주체는 관찰자, 옵저버는 변화가 생기는 객체들
- 옵저버 패턴은 객체 간 의존성을 최소화하면서 효율적 통지가 가능한 패턴

## 옵저버 패턴의 장점과 단점

### 장점

1. 객체 간의 느슨한 결합 : 주체와 옵저버는 서로에 대해 잘 알 필요가 없다. 즉 시스템 유연성과 확장성 증가
2. 확장성 : 새로운 옵저버를 쉽게 추가할 수 있으며, 주체를 변경하지 않고도 옵저버 변경 가능
3. 자동 업데이트 : 주체의 상태 변화가 자동으로 옵저버들에게 통지되어 일관된 상태를 유지할 수 있다.

### 단점

1. 복잡성 증가 : 다수의 옵저버가 있을 경우, 통지 메커니즘이 복잡
2. 성능 정하 : 많은 옵저버가 등록되어 있는 경우, 주체의 상태 변화가 빈번하면 서능 저하

## 옵저버 패턴을 사용한 예 - 트위터

![Untitled](%E1%84%8B%E1%85%A9%E1%86%B8%E1%84%8C%E1%85%A5%E1%84%87%E1%85%A5%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%204e177fe3519e46878facf66519458df8/Untitled.png)

위의 그림처럼, 사용자가 다른 사용자인 주체를 ‘팔로우’하게 될 경우 주체가 포스팅을 올리게 되면 알림이 ‘팔로워’에게 가도록 설정

```java
package com.example.cs.Singleton;

import java.util.ArrayList;
import java.util.List;

interface Observer {
    void update(String username, String tweet);
}

class User implements Observer {
    private String username;
    private List<Observer> followers = new ArrayList<>();
    private List<User> following = new ArrayList<>();
    private String latestTweet;

    public User(String username) {
        this.username = username;
    }

    public void follow(User user) {
        user.addFollower(this);
        following.add(user);
    }

    public void unfollow(User user) {
        user.removeFollower(this);
        following.remove(user);
    }

    public void addFollower(Observer observer) {
        followers.add(observer);
    }

    public void removeFollower(Observer observer) {
        followers.remove(observer);
    }

    public void notifyFollowers() {
        for (Observer follower : followers) {
            follower.update(username, latestTweet);
        }
    }

    public void tweet(String tweet) {
        this.latestTweet = tweet;
        System.out.println(username + " tweeted: " + tweet);
        notifyFollowers();
    }

    @Override
    public void update(String username, String tweet) {
        System.out.println(this.username + " received tweet from " + username + ": " + tweet);
    }

    public String getUsername() {
        return username;
    }
}

public class ObserverMain {
    public static void main(String[] args) {
        User userA = new User("UserA");
        User userB = new User("UserB");
        User userC = new User("UserC");

        userB.follow(userA);
        userC.follow(userA); 

        userA.tweet("하이 내 팔로워들아 + A말함");

        userA.follow(userB); 
        userB.tweet("안녕 내 팔로워들아 + B 말함");

        userC.follow(userB); 
        userB.tweet("안녕 내 팔로워들아 + B 두번째 말함");
    }
}

```

위의 코드는 트위터와 비슷하게, 옵저버 패턴을 이용하여 주체와 옵저버 간의 관계를 설정 후 주체가 tweet을 할 경우 옵저버들에게 변화가 생기는 코드이다.

주체가 tweet을 할 경우 주체를 팔로워하는 옵저버들에게 tweet한 내용이 전달

## 옵저버 패턴은 언제 사용하는 가?

1. GUI 이벤트 시스템 : 버튼 클릭, 마우스 이동 등의 이벤트를 여러 컴포넌트가 수신할 때 사용
2. MVC 아키텍처 : 모델의 상태 변화가 뷰에 반영되어야 할 때 사용. 모델이 주체이고 뷰가 옵저버
3. 실시간 데이터 업데이트 : 주식 가격, 스포츠 경기 결과 등 실시간 데이터를 여러 클라이언트에게 전파할 때 사용
4. 알림 시스템 : 특정 이벤트가 발생했을 때 여러 사용자나 시스템 컴포넌트에게 알림을 보낼 때 사용