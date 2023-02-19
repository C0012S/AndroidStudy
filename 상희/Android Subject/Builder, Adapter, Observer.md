# 💡 Builder, Adapter, Observer

# ✅ Builder
## Builder 패턴이란?
- 안드로이드, 자바 개발을 할 때 자주 쓰이는 패턴 중 하나다.
- Builder 패턴은 클래스 생성 패턴으로 멤버 변수가 많은 경우 다양한 생성자가 필요하다는 단점을 해결할 수 있는 패턴이다.
- Builder 패턴은 복합 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에서 서로 다른 표현 결과를 만들 수 있게 하는 패턴이다.
    - 간단하게 얘기하면 복잡한 객체를 생성하는 과정을 추상화하여 서브 클래스에게 넘겨 서브 클래스에서 객체를 생성하도록 하는 디자인 패턴이다.
- Builder 패턴을 사용하면 코드의 가독성이 높아지고 유지 보수를 편리하게 할 수 있다. 또한 객체 생성도 깔끔히 할 수 있다.
- 안드로이드에서 NotificationCompat와 자바에서 StringBuilder에서 Builder 패턴을 사용하는 것을 확인할 수 있다.

<br/>

## Builder 패턴 구조
![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/drtLMo/btraqs152lI/4FGvPpVjNhlFhekXXBI0qK/img.jpg](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/drtLMo/btraqs152lI/4FGvPpVjNhlFhekXXBI0qK/img.jpg)

<br/>

- Director : 내부적으로 Builder 인터페이스 객체를 통해 Product를 생성하는 클래스
- Builder : Product에 필요한 기능을 가지는 인터페이스
- ConcreteBuilder : Builder 인터페이스를 상속받아 구체적으로 기능을 구현하고 Product를 생성하는 클래스
- Product : 생성해야 할 복합 객체

<br/>

## Builder 패턴 장단점
### Builder 패턴 장점
- 클래스의 내부 표현을 변경할 수 있다.
- 구성 및 표현을 위한 코드를 캡슐화한다.
- `construction process`의 단계에 대한 제어를 제공한다.

<br/>

### Builder 패턴 단점
- 각 클래스 유형에 대한 고유한 `ConcreteBuilder`를 만들어야 한다.
- Builder 클래스는 변경 가능해야 한다.
- 종속성 주입을 방해하거나 복잡하게 만들 수 있다.

<br/>

## Android Builder Pattern
- AlertDialog
- Retrofit

<br/>
<br/>

# ✅ Adapter
![https://obsoletedeveloper.files.wordpress.com/2012/09/hf-adapteruml.jpg](https://obsoletedeveloper.files.wordpress.com/2012/09/hf-adapteruml.jpg)

위 그림을 간단히 설명하면 Client가 Adapter의 인터페이스의 시그내처만 참고하여 구현을 하면, 상황에 따라 Adaptee에 맞는 Adapter를 사용하는 것만으로 수정 없이 사용이 가능하다는 것이다.

<br/>

### 실생활 예
![https://obsoletedeveloper.files.wordpress.com/2012/09/hf-adapter.jpg?w=300&h=167](https://obsoletedeveloper.files.wordpress.com/2012/09/hf-adapter.jpg?w=300&h=167)

<br/>

어댑터를 이용하면 호환되지 않는 충전기도 사용할 수 있다.

<br/>

## Adapter 패턴이 필요한 이유?
위의 예처럼 어댑터 디자인 패턴을 사용하는 경우 서로 호환되지 않는 인터페이스를 사용하는 클라이언트를 그대로 활용할 수 있다.  
인터페이스를 변환해 주는 어댑터만 만들면 된다.  
이를 통해 클라이언트와 구현된 인터페이스를 분리할 수 있고, 나중에 인터페이스가 바뀌더라도 그 변경 내역은 어댑터에 캡슐화되기 때문에 클라이언트는 바뀔 필요가 없어진다.

<br/>

## Adapter 패턴 언제 사용할 수 있을까?
- 기존 클래스를 사용하고 싶은데 인터페이스가 맞지 않을 때
- 이미 만든 것을 재사용하고자 하나 이 재사용 가능한 라이브러리를 수정할 수 없을 때

<br/>

## Adapter 패턴이란?
어댑터 패턴(Adapter Pattern)은 클래스의 인터페이스를 사용자가 기대하는 다른 인터페이스로 변환하는 패턴으로, 호환성이 없는 인터페이스 때문에 함께 동작할 수 없는 클래스들이 함께 작동하도록 해 준다.

<br/>

## Adapter 패턴의 역할
- 어떤 인터페이스를 클라이언트에서 요구하는 형태의 인터페이스에 적응시켜 주는 역할. 즉, 중개인 역할
- 한 인터페이스를 다른 인터페이스로 변환하기 위한 용도

<br/>

## Adapter 패턴 다이어그램
![Untitled](https://velog.velcdn.com/images%2Fellyheetov%2Fpost%2Fc1df519d-c3f6-451d-973b-618f1d6b4b18%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.53.13.png)

<br/>

- Target : 인터페이스를 정의하는 클래스
- Client : Target 인터페이스를 만족하는 객체와 동작할 대상
- Adaptee : 적응 대상자. 인터페이스의 적응이 필요한 기존 클래스, 쉽게 말해 호환되지 않는 클래스. Client 의 Target 인터페이스를 요구하는 요소에 집어넣고 싶은 클래스
- Adapter : Target 인터페이스에 Adaptee 인터페이스를 적용시키는 클래스

<br/>

## Adapter 패턴의 장단점
### 장점
- 기존 코드를 변경하지 않아도 된다.
- 기존 코드를 변경하지 않기 때문에 클래스 재활용성을 증가시킬 수 있다.

<br/>

### 단점
- 구성 요소를 위해 클래스를 증가시켜야 하기 때문에 복잡도가 증가한다.
- 클래스 Adapter의 경우 상속을 사용하기 때문에 유연하지 못하다.
- 객체 Adapter의 경우는 대부분의 코드를 다시 작성해야 하기 때문에 효율적이지 못하다.

<br/>

## Adapter 패턴 사용 시 고려할 점
- Adapter 클래스를 사용하기 위해 들어가는 비용이 얼마나 되나?
    - 작업량을 결정짓는 요인은 Target 인터페이스와 Adaptee 간에 얼마만큼의 유사성을 갖는가?
- 대체 가능(Pluggable) 적응자, 누가 이 클래스를 사용할지에 대한 생각은 최소화
    - 모든 사용자에게 동일한 인터페이스르 제공하겠다는 생각은 넣어 둬라. 어댑터 패턴을 통해 부담을 덜 수 있다.

<br/>
<br/>

# ✅ Observer
프로그래밍에서 Observer 패턴이라고 한다면 어떤 '이벤트' 가 일어나는 것을 감시하는 패턴을 의미한다.

<br/>

> 안드로이드를 예로 들어 설명하자면 아래와 같은 것들이 이벤트가 발생한 순간이라고 할 수 있다.  
> 1. 사용자가 키보드를 눌렀을 때  
> 2. 사용자가 어떤 버튼을 터치했을 때  
> 3. 호출한 API의 응답 데이터가 수신됐을 때
> 

<br/>

정리하면, 아무도 함수로 직접 요청한 적 없지만 시스템에 의해 발생하는 동작들을 이벤트라고 한다.  
이러한 이벤트들을 감시하여, 이벤트가 발생할 때마다 미리 정의해 둔 어떠한 동작을 즉각 수행하게 해주는 프로그래밍 패턴을 Observer 패턴이라고 한다.
  - ex : A라는 버튼이 클릭될 때마다 화면에 '안녕'을 출력하는 동작 등

Observer 패턴을 활용하면 **다른 객체의 상태 변화를 별도의 함수 호출 없이 즉각적으로 알 수 있기** 때문에, 이벤트에 대한 처리를 자주 해야 하는 프로그램이라면 매우 **효율적인 프로그램을 작성**할 수 있다.

<br/>

## Observer 패턴 클래스 다이어그램
![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/dwPxTH/btqwl5c864B/ShHjE0ZAKTLhN6XrNneI9K/img.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/dwPxTH/btqwl5c864B/ShHjE0ZAKTLhN6XrNneI9K/img.png)

<br/>
<br/>

# 🗂 참고
- [[Java] 디자인 패턴 (Builder 패턴) (tistory.com)](https://math-coding.tistory.com/148)
- [빌더 패턴(Builder Pattern) (tistory.com)](https://sorjfkrh5078.tistory.com/131)
- [Builder Pattern 이란 (tistory.com)](https://devgeek.tistory.com/m/60)
- [안드로이드의 어댑터(Adapter) – Dog발자 (sunphiz.me)](http://sunphiz.me/wp/archives/1292)
- [어댑터 패턴(Adapter/Wrapper Pattern) (velog.io)](https://velog.io/@ellyheetov/%EC%96%B4%EB%8C%91%ED%84%B0-%ED%8C%A8%ED%84%B4AdapterWrapper-Pattern)
- [[안드로이드] 어댑터 패턴 (Adapter Pattern) - BictoSelfDev](https://bictoselfdev.blogspot.com/2021/03/adapter-pattern.html)
- [옵저버 패턴 개념 떠먹여드립니다 🥄 (velog.io)](https://velog.io/@haero_kim/%EC%98%B5%EC%A0%80%EB%B2%84-%ED%8C%A8%ED%84%B4-%EA%B0%9C%EB%85%90-%EB%96%A0%EB%A8%B9%EC%97%AC%EB%93%9C%EB%A6%BD%EB%8B%88%EB%8B%A4)
