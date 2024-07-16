# 프록시 패턴

## 프록시패턴이란?

- 대상 원본 객체를 대리하여 대신 처리하게 함으로써 로직의 흐름을 제어하는 행동 패턴
- 프록시의 사전적 의미는 ‘대리인’으로, 누군가에게 어떤 일을 대신 시키는 것을 의미하는데, 클라이언트가 대상 객체를 직접 쓰는게 아니라 중간에 프록시(대리인)을 거쳐서 쓰는 패턴

## 왜 프록시 패턴을 사용할까?

그냥 객체를 바로 이용하면 되는데, 이렇게 번거롭게 대지라를 통해 이용하는 방식을 사용하는 이유는, 대상 클래스가 민감한 정보를 가지고 있거나, **원본 객체를 수정할 수 없는 상황일 때**를 극복하기 위해서이다.

## 프록시 패턴 장점

- 개방 폐쇄 원칙(OCP) 준수 :  기존 대상 객체의 코드를 변경하지 않고 새로운 기능 추가
- 단일 책임 원칙(SRP) 준수 : 대상 객체는 자신의 기능에만 집중하고, 그 외 부가 기능을 제공하는 역할을 프록시 객체에 위임하여 다중 책임 회피
- 원래 하려던 기능을 수행하며 부가 작업 수행하는데 유용
- 클라이언트는 객체를 신경쓰지 않고, 서비스 객체를 제어하거나 생명 주기 관리
- 사용자 입장에서 프록시 객체나 실제 객체나 사용법은 유사하므로 사용성에 문제 되지 않는다.

## 프록시 패턴 단점

- 프록시 클래스 도입으로 코드의 복잡도 증가
- 프록시 클래스에 들어가는 자원이 많으면 서비스로부터 응답이 늦음

```java
interface IImage {
	void showImage();
}

class HighResolutionImage implements IImage {
	
	String img;
	
	HighResolutionImage(String path) {
		loadImage(path);
	}
	
	private void loadImage(String path) {
        try {
            Thread.sleep(1000);
            img = path;
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.printf("%s에 있는 이미지 로딩 완료\n", path);
    }

    @Override
    public void showImage() {
        System.out.printf("%s 이미지 출력\n", img);
    }
}

//프록시객체
class ImageProxy implements IImage {
    private IImage proxyImage;
    private String path;

    ImageProxy(String path) {
        this.path = path;
    }

    @Override
    public void showImage() {
        proxyImage = new HighResolutionImage(path);
        proxyImage.showImage();
    }
}

```

## 실무에서 사용하는 예

### 스프링 AOP

스프링에서 Bean을 등록할 때 Singleton을 유지하기 위해 프록시 기법을 이용해 프록시 객체를 bean에 등록한다.

프록시를 통해, 비즈니스 로직과 부가 기능을 분리