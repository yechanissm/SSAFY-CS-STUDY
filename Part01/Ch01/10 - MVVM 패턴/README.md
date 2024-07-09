# MVVM 패턴

- MVC 패턴에서 파생되었으며, [**C**ontroller]가 [뷰모델(**V**iew **M**odel)]로 교체된 패턴
- MVC 패턴과는 다르게 뷰 <-> 뷰모델 간 알림 시 커맨드와 데이터 바인딩을 가짐

### View Model(VM)

- View Model은 View를 더 추상화한 계층
- View 상태를 유지 및 변화시키고, View에 대한 작업의 결과로 Model을 조작한다
- View에서 발생되는 이벤트를 전달 하는데 도움이 되는 메서드, 명령, 또는 다른 속성들을 노출하는 역할을 한다
- MVVM은 View와 ViewModel사이의 관계가 1대N으로 되어있다
- 장점 :
  - View와 View Model 사이 양방향 데이터 바인딩을 지원
  - View와 Model 사이의 의존성이 없다. View와 View Model 사이의 의존성이 없다
  - UI를 별도의 코드 수정 없이 재사용 가능
  - 단위 테스팅하기 쉬움
- 단점 :
  - View Model의 설계가 어려움

## MVVM 패턴 예시

### Vue.js

- 반응형(reactivity)이 특징인 프론트엔드 프레임워크
  - watch와 computed 등으로 쉽게 반응형적인 값을 구축할 수 있음
- 함수를 사용하지 않고 값 대입만으로도 변수가 변경됨
- 양방향 바인딩, html을 토대로 컴포넌트를 구축할 수 있음
- 재사용 가능한 컴포넌트를 기반으로 UI를 구축할 수 있음 (BMW, 루이비통, 구글에서 사용)
