# 객체 지향

## 객체 지향 언어의 특징

1. 캡슐화
    - 객체에 필요한 데이터나 기능(메소드)을 책임이 있는 객체에 `그룹화`시키는 것
    - 객체가 맡은 역할을 수행하기 위한 하나의 목적을 위해 데이터와 기능들을 묶는다.
    - 데이터를 `은닉`하고 그 데이터를 접근하는 기능을 노출시키지 않는다는 의미
2. 상속성
    - 상속이 없으면 객체지향은 절차지향과 다른점이 거의 없다.
    - 상위 개념의 특징을 하위 개념이 물려받는 것을 말한다.
    - 하나의 클래스가 가지고 있는 특징(데이터, 함수)들을 그대로 다른 클래스가 물려주고자 할 때 상속성의 특징을 사용한다.
3. 다형성
    - 상속과 연관이 있는 개념으로 한 객체가 다른 여러형태(객체)로 재구성 되는 것을 말한다.
    - 대표적인 예로 오버라이딩, 오버로딩이 있다.
        - 오버로딩 : **같은 이름**의 메소드(method) 또는 생성자지만 입력받는 **매개변수의 타입이나 갯수**에 따라 다른 액션을 취하도록 하는 것

            ```java
            OverLoding(int n){
            	System.out.println("int " + n);
            }

            OverLoading(String s){
            	System.out.println("String " + s);
            }
            ```

        - 오버라이딩 : 부모클래스로부터 메소드를 상속받았으나, 그 메소드(매개변수, 리턴타입이 완전히 같은)를 자식클래스에서 재정의하여 사용하는 것(상속받은 메소드 무시)

            ```java
            class Dog{
            	void sound(){
            		System.out.println("멍멍");
            	}
            }

            class Poodle extends Dog{
            	@Override
            	void sound(){
            		System.out.println("왈왈");
            	}
            }
            ```

4. 추상화
    - 객체들의 공통적인 특징(속성과 기능)을 뽑아내는 것이다.
    - 필요한 정보만을 중심으로 간소화하는 것

## 객체지향설계 5원칙 SOLID

[https://bottom-to-top.tistory.com/27](https://bottom-to-top.tistory.com/27)

### 1. SRP(Single Responsibility Principle - 단일 책임의 원칙)

**"한 클래스는 하나의 책임을 가져야 한다."**

### 2. OCP(Open Close Principle - 개방 폐쇄 원칙)

**"소프트웨어 요소는 확장에는 열려있고, 변경에는 닫혀 있어야 한다."**

### 3. LSP(Liskov Substitution Principle - 리스코프 치환 원칙)

**"프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다."**

### 4. ISP(Interface segregation principle - 인터페이스 분리 원칙)

**"특정 클라이언트를 위한 인터페이스 여러개가 범용 인터페이스 하나보다 낫다."**

- SRP를 어기지 않도록 인터페이스 분리.
- 업캐스팅을 통해 여러 인터페이스 활용

### 5. DIP(Dependency Inversion Principle - 의존관계 역전 원칙)

**"추상화에 의존해야지, 구체화에 의존하면 안된다."**

---

REFERENCE

[https://sesok808.tistory.com/31](https://sesok808.tistory.com/31)

[https://youngjinmo.github.io/2021/04/features-of-oop/](https://youngjinmo.github.io/2021/04/features-of-oop/)

[https://blog.naver.com/alcmskfl17/221736323482](https://blog.naver.com/alcmskfl17/221736323482)

[https://bottom-to-top.tistory.com/27](https://bottom-to-top.tistory.com/27)
