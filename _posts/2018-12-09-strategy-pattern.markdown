---
layout: post
title:  "[디자인 패턴] Strategy pattern (전략 패턴)"
categories: [pattern]
img: use-post/design-pattern.jpg
toc: true
---

최근 진행중인 스터디는 Head First Design Pattern 책을 통한 디자인 패턴에 대한 스터디입니다. Head First Design Patterns 에 처음으로 등장하는 pattern 은 **전략패턴**, 즉 **Strategy Pattern** 입니다.

# Strategy Pattern 이란?
***
먼저 strategy 라는 영단어는 어떤 뜻을 가지고 있는지 확인해봅시다.
> strategy 명사(pl. -ies)
> 1. [C] (특정 목표를 위한) 계획[전략]
> 2. [U] 계획[전략] 수립[집행]
> 3. [U, C] (군사적인) 전략 참조 tactic


strategy 는 계획, 전략의 뜻을 가지고 있네요. 한국어로 그대로 바꿔 전략 패턴이란 말로 사용되고 있는거죠. 

전략패턴의 정의를 책 및 구글링으로 찾아보니 아래와 같이 말할 수 있었습니다.
> 전략패턴이란 실행 중에 알고리즘을 선택할 수 있게 하는 행위 소프트웨어 디자인 패턴이다. 이 패턴에서는 알고리즘군을 정의하고 각각을 캡슐화하여 교환해서 사용할 수 있도록 만든다.

순서를 나열해보자면
1. 특정한 계열의 알고리즘들을 정의하고
2. 각 알고리즘을 캡슐화하며
3. 이 알고리즘들을 해당 계열 안에서 상호 교체가 가능하게 만든다.

이렇게 정리할 수 있을 것입니다. 

하지만, 이 말들만으로는 쉽게 이해가 안되는군요. 원활한 이해를 돕기 위해 책에 있는 예제를 활용해보도록 하겠습니다.

# Strategy Pattern 예제
***

### 문제 상황
상황은 이렇습니다. 오리 시뮬레이터를 만드는 회사에서 상속을 통해 관련 클래스 및 인터페이스를 정의하고 구현하였습니다. 구조는 아래와 같습니다.

먼저 모든 '오리클래스' 들이 상속받아야 하는 추상클래스 <mark>Duck</mark> 이 있습니다.
```java
public abstract class Duck {
    public void quack() { System.out.println("Quack Quack"); }
    public void swim() { System.out.println("오리가 수영중 ~~"); }
    public abstract void display();
    
    // 오리에게 '날기' 액션을 주기 위해 추가한 메서드.
    public void fly() { System.out.println("오리 날다 !!!"); }
}
```
<mark>Duck</mark> 을 상속받은 여러 종류의 오리 클래스들도 있죠.
```java
public class MallardDuck extends Duck {
    @Override
    public void display() { System.out.println("청둥오리입니다."); }
}

public class RedheadDuck extends Duck {
    @Override
    public void display() { System.out.println("빨간머리 오리입니다."); }
}

public class RubberDuck extends Duck {
    @Override
    public void display() { System.out.println("고무인형 오리입니다."); }

    @Override
    public void quack() { System.out.println("ppi ~ ppi ~"); }
}
```
뭔가 좀 다른 게 보이시나요? 고무 오리인 <mark>RubberDuck</mark> 클래스만 울음소리 메서드인 `quack()` 메서드까지 오버라이드 되었습니다. 오리의 외형을 나타내는 `display()` 메서드는 알아서 다들 다르게 오버라이드 하였습니다. 애초에 추상메서드이니 반드시 할 수밖에 없죠. <mark>RubberDuck</mark> 클래스 개발을 담당한 개발자는 <mark>RubberDuck</mark> 이 꽥꽥 울지 않는다는 사실을 알고 `quack()` 클래스까지 오버라이드 해 두었군요.

`fly()` 메서드는 필요에 의해 이번에 새로 추가되었습니다. 모든 오리는 당연히 날 수 있다고 생각한 누군가가 `fly()` 메서드를 <mark>Duck</mark> 클래스에 만들어놨군요! 그래서 시연 시 모두 날아다니고 있는 상황입니다. 


### 첫번째 해결책 : 상속으로 해결
<mark>RubberDuck</mark> 클래스 개발자는 이미 `quack()` 클래스를 오버라이드 한 경험이 있습니다. <mark>RubberDuck</mark> 클래스는 <mark>Duck</mark> 클래스를 상속받았으므로 <mark>Duck</mark> 클래스의 어떠한 메서드라도 오버라이드 할 수 있죠. 그래서 간단히 `fly()` 메서드도 오버라이드 하기로 결정합니다. 

그리하야 <mark>RubberDuck</mark> 클래스를 아래와 같이 변경하게 되죠.
```java
public class RubberDuck extends Duck {
    @Override
    public void display() { System.out.println("고무인형 오리입니다."); }

    @Override
    public void quack() { System.out.println("ppi ~ ppi ~"); }

    @Override
    public void fly() {}
}
```
`fly()` 메서드를 오버라이드 하여 날지 못하게 만들었습니다. 당장 문제는 해결됐습니다. 날아다니던 고무오리가 날지 않게 되었으니까요. 

아니 그런데.. 이번엔 운영진이 목재오리 를 추가하기로 하였군요. 목재오리는 소리도 나지 않고 날 수도 없습니다. 이를 어쩌죠? <mark>RubberDuck</mark> 과 똑같이 두 메서드를 모두 오버라이드 하여 클래스를 작성하여야만 할까요? 이렇게 어떤 클래스가 추가될 때마다 오버라이드가 필요한 클래스를 정해서 매번 오버라이드를 하여야 할까요?


### 두번째 해결책 : 인터페이스
어떤 오리는 날고 어떤오리는 못 날고, 어떤 오리는 꽥꽥거리며 어떤 오리는 삑삑거립니다. 오리별로 행동이 갈리는 '날다' 는 행동과 '울다' 는 행동을 행동 인터페이스로 분리하면 어떨까요?

각각의 행동을 정의하는 두가지 인터페이스를 만들었습니다.
```java
public interface Flyable {
    void fly();
}

public interface Quackable {
    void quack();
}
```
각 오리에게 알맞는 행동을 하도록 코드를 변경해봅시다.
```java
public class MallardDuck extends Duck implements Flyable, Quackable {
    @Override
    public void display() { System.out.println("청둥오리입니다."); }

    @Override
    public void fly() { System.out.println("오리 날다!!!"); }

    @Override
    public void quack() { System.out.println("Quack Quack"); }
}

public class RedheadDuck extends Duck implements Flyable, Quackable  {
    @Override
    public void display() { System.out.println("빨간머리 오리입니다."); }

    @Override
    public void fly() { System.out.println("오리 날다!!!"); }

    @Override
    public void quack() { System.out.println("Quack Quack"); }
}

public class RubberDuck extends Duck implements Quackable {
    @Override
    public void display() { System.out.println("고무인형 오리입니다."); }

    @Override
    public void quack() { System.out.println("ppi ~ ppi ~"); }
}
```
별로 좋지 않군요. <mark>MallardDuck</mark> 클래스와 <mark>RedheadDuck</mark> 클래스는 모두 날 수도 있고 꽥꽥거릴 수도 있는데 같은 코드를 항상 반복해서 넣어주어야 합니다. 만약 오리의 울음소리가 바뀐다면 두 클래스를 다 찾아다니며 같은 코드를 작성하여야 할 것입니다. 또 만약 생명체 오리인 집오리가 추가된다면 같은 작업을 세 번 반복하여야 하는 것입니다.

#### __애플리케이션은 항상 변화합니다. 이것만이 소프트웨어 개발에 있어 절대 변하지 않는 진리이죠.__

디자인 패턴은 이런 변화하는 프로그램을 어떻게 하면 좀 더 유지보수 하기 쉽게, 중복 없이, 재사용이 가능하도록 작성할 수 있는가에 대한 고민에서부터 시작되었습니다. 

이쯤에서 한 번 디자인의 원칙에 대해 생각해봅시다.
> - 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분으로부터 분리시킨다.
> - 구현이 아닌 인터페이스에 맞춰서 프로그래밍 한다.
> - 상속보다는 구성을 활용한다.

이 세가지를 염두에 두면서 위 코드에 대해 다시 생각해보도록 합시다.


### 세번째 해결책 : Strategy 패턴 적용
1. 먼저 바뀌는 부분과 그렇지 않은 부분을 고려해봅시다.
<mark>Duck</mark> 클래스에서 바뀌는 부분은 `fly()` 메서드와  `quack()` 메서드 입니다. 이 클래스는 각각 특정 행동을 나타내죠. '나는 행동' 과 '꽥꽥거리는 행동' 이 그것입니다. 이 '나는 행동' 과 '꽥꽥거리는 행동' 에는 오리별로 다른 행동들이 포함됩니다. 하지만 오리의 외형처럼 모든 오리가 전부 다른 행동을 하는 것이 아니라 어떤 오리'들' 은 이런 행동을, 어떤 오리'들'은 저런 행동을 하는 것이죠. 

2. 각 행동을 나타낼 집합을 만듭시다.
위에서 고려한 것을 토대로 오리별로 다른 행동군의 집합을 만들 수 있습니다. 이 때 우리는 __디자인 패턴의 원칙__ 중 **인터페이스에 맞춰 프로그래밍 한다** 는 부분을 떠올려보도록 합시다. 행동별로 인터페이스를 작성하고 특정 행동은 해당 인터페이스를 구현하도록 코드를 작성한다면 여러가지 이점이 따라옵니다.
```text
- 오리의 행동을 동적으로 바꿀 수 있습니다.
- java 의 오래된 규칙인 '변수를 선언할 때는 상위 클래스 및 인터페이스 형식으로 선언하라' 는 규칙을 지킬 수 있습니다. 
```
이로써 다형성을 지키며 좀 더 유연한 클래스를 작성할 수 있게 되는 것입니다. 이를 숙지하여 아래와 같은 인터페이스를 구성해줍시다. (FlyBehavior 코드만 게재하지만 QuackBehavior 인터페이스 및 하위클래스도 동일하게 구현합니다.)
```java
public interface FlyBehavior {
    void fly();
}
```
이 인터페이스는 단순이 나는 행동과 구체적인 나는 행동들이 무엇을 구현해야 하는지만 나타냅니다. 이러한 인터페이스를 따라 구체적인 클래스를 작성해주도록 합니다.
```java
public class FlyWithWings implements FlyBehavior {
    @Override
    public void fly() { System.out.println("오리 날다!!!"); }
}
```
```java
public class FlyNoWay implements FlyBehavior {
    @Override
    public void fly() { System.out.println("저는 날지 못해요."); }
}
```
<mark>FlyBehavior</mark> 인터페이스와 그 하위클래스들은 이제 오리뿐만이 아니라 다른 클래스들에서도 사용할 수 있습니다. 게다가 나는 행동에 어떠한 다른 행동양식(예를 들어 로켓을 달고 날아간다던가)이 추가된다면 기존 클래스들을 건드리지 않고 간단히 추가가 가능합니다.

3. Duck 클래스 변경
인터페이스가 바뀌었으니 이에 맞추어 오리 클래스들을 변경해주도록 합시다. 먼저 이 행동들은 <mark>Duck</mark> 클래스가 가지고 있습니다. 물론 인터페이스 형식으로 가지고 있어야 하겠죠.
```java
public abstract class Duck {
    FlyBehavior flyBehavior;
    QuackBehavior quackBehavior;
    public void swim() { System.out.println("오리가 수영중 ~~"); }
    public abstract void display();
    public void performQuack() {
        this.quackBehavior.quack();
    }
    public void performFly() {
        this.flyBehavior.fly();
    }
}
```
이제 <mark>Duck</mark> 클래스는 직접 날거나 꽥꽥거리지 않고 해당 행동을 행동클래스에게 위임하였습니다. 실제 행동 자체는 <mark>FlyBehavior</mark>, <mark>QuackBehavior</mark> 클래스가 고 <mark>Duck</mark> 클래스는 이를 실행만 시켜줍니다. 이것으로 끝이 아닙니다. 이 프로그램에는 <mark>Duck</mark>클래스를 상속받은 많은 클래스들이 있으므로 이 실제 오리들 에게 이 행동들을 사용할 수 있는 방법을 알려주어야 합니다.

4. 실제 오리들에게 알려주기
클래스가 생성될 때 어떤 행동을 하는 오리인지 지정해서 만들 수도 있고 setter 를 이용하여 중간에 변경할 수 있게끔 만들수도 있을 것입니다.
```java
public class MallardDuck extends Duck {
    public MallardDuck() {
        this.flyBehavior = new FlyWithWings();
        this.quackBehavior = new Quack();
    }
    @Override
    public void display() { System.out.println("청둥오리입니다."); }
    
    public void setFlyBehavior(FlyBehavior flyBehavior) {
        this.flyBehavior = flyBehavior;
    }
    
    
    public void setQuackBehavior(QuackBehavior quackBehavior) {
        this.quackBehavior = quackBehavior;
    }
}
```
<mark>MallardDuck</mark> 클래스는 생성될 때는 날개로 날며 꽥꽥 소리를 내는 상태로 만들어져 개발자가 원하는 순간 setter 메서드를 통해 다른 행동을 하는 객체로 변경시킬 수도 있습니다. 예를 들면 아래와 같이 사용할 수 있을거에요.
```java
Duck mallardDuck = new MallardDuck();
mallardDuck.performFly(); // 여기에선 날 수 있다고 나오겠지만
mallardDuck.setFlyBehavior(new FlyNoWay());
mallardDuck.performFly(); // 여기에선 못 난다고 나올거에요.
```
이렇게 두 클래스를 합치는 것을 **구성(composition)**을 이용하는 것이라고 부릅니다. 오리 클래스는 행동을 상속받은 게 아니라 알맞은 행동을 구성함으로써 알맞은 행동을 부여받았죠. 이것은 디자인 원칙의 세번째 원칙인 **상속보다는 구성을 활용한다** 는 원칙이기도 합니다. 구성을 사용한 프로그래밍은 유연성을 크게 향상시킬 수 있습니다. 


네, 이것으로 오리 클래스들은 전략 패턴을 이용하여 리팩토링 되었습니다. 이제 디자인 패턴을 하나 배운 것이지요. 아직도 앞으로 갈 길이 멀군요 ^^; 

당연히 다들 알고 있겠지만 이 패턴이 모든 상황의 정답이 아니며 어떠한 디자인 패턴도 모든 상황의 정답이 될 순 없답니다. 앞으로 여러가지 디자인 패턴을 공부해가며 알맞게 적용해가는 방법도 배우도록 합시다 ^^
