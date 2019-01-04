---
layout: post
title:  "[디자인 패턴] Decorator pattern (데코레이터 패턴)"
categories: [pattern]
img: use-post/design-pattern.jpg
toc: true
---

이번에 알아볼 패턴은 **데코레이터 패턴** 입니다. 데코레이터라니 뭔가 장식하거나 꾸민다는 느낌이 드는군요.

영어사전에서 Decorator 라는 단어를 찾아보면 아래와 같이 나옵니다.
> decorator
> 1. 주로 실내 장식을 하는 전문가, 업자.

도대체 Decorator Pattern 이 어떤 기능을 하는 패턴이기에 '장식가' 라는 명칭이 붙은 걸까요?

예제를 통해 알아보도록 하겠습니다.

## 카페에서 커피의 가격을 계산하는 포스기를 생각해봅시다.

기본적인 커피의 가격이 있을겁니다. 기본적인 에스프레소 나 아메리카노 같은 종류가 있을 수 있죠. 그대로 먹는 사람들도 많지만 이런 저런 추가를 해서 먹는 사람들도 많습니다. 저만해도 한동안 아메리카노에 휘핑크림을 추가하는 것을 즐기던 때가 있었죠 :D

이런 기본적인 커피의 요금과 부수적인 토핑의 요금을 계산해주는 프로그램을 작성해봅시다.

먼저 모든 음료가 상속받아야 하는 추상클래스 `Bevarage` 가 있습니다. 이 추상클래스를 상속받은 클래스들은 모두 기본 메뉴임을 나타내죠.
```java
public abstract class Beverage {
    public abstract String getDescription();
    public abstract int cost();
}
```
기본 메뉴로는 아메리카노 와 에스프레소 가 있습니다. 기본 메뉴 이므로 `Beverage` 클래스를 상속받아 구현해주도록 합니다.
```java
public class Americano extends Beverage {
    @Override
    public String getDescription() {
        return "아메리카노 ";
    }
    
    @Override
    public int cost() {
        return 3000;
    }
}

public class Espresso extends Beverage{
    @Override
    public String getDescription() {
        return "에스프레소";
    }
    
    @Override
    public int cost() {
        return 2000;
    }
}
```
고객들이 추가할 수 있는 토핑은 무수히 많겠지만 여기에서는 세가지 정도만 생각해보도록 하겠습니다. 고객들은 시럽, 휘핑크림, 얼음 중 원하는 토핑을 넣을 수 있다고 가정해봅시다.

이 부가메뉴들을 포스기에 넣는 여러가지 방법이 있겠지만 일단 기본메뉴에는 모두 추가할 수 있는 토핑들이니 `Beverage` 클래스에 토핑이 들어갔는지 안들어갔는지 체크할 수 있도록 만들어봅시다.

위에서 작성하였던 `Beverage` 클래스를 아래와 같이 바꿔주겠습니다.
```java
public abstract class Beverage {
    private boolean sugar;
    private boolean whip;
    private boolean ice;
    public abstract String getDescription();
    public abstract int cost();
    
    // setter 및 getter 생략
}
```
먼저 첨가 유무를 알 수 있는 boolean 변수를 토핑의 종류만큼 만들어주었고 현재 페이지에서는 생략하였지만 해당 변수들의 getter 와 setter 를 각각 만들어주었습니다.

각 토핑이 추가되었는지 여부가 저장되는 변수가 만들어졌으니 이제 실제 상품의 가격에 반영될 수 있도록 수정해주어야 합니다. `Amaricano` 와 `Espresso` 클래스의 `cost()` 클래스를 아래와 같이 변경시켜줍니다. 
```java
@Override
public int cost() {
    int price = 3000; // Americano 와 Espresso 의 가격은 다릅니다.
    if (this.hasSugar()) {
        price += 200;
    }
    if (this.hasWhip()) {
        price += 300;
    }
    if (this.hasIce()) {
        price += 500;
    }
    return price;
}
```
잘 작동하는지 아주 간단하게 테스트 해봅시다.
```java
public static void main(String[] args) {
    Americano americano = new Americano();
    americano.setIce(true);
    System.out.println(americano.getDescription() + " : " + americano.cost());
}
```
```bash
아메리카노  : 3500
```
결과는 잘 나옵니다! 여러번 해봐도 그렇죠. `Americano` 클래스와 `Espresso` 클래스에 대부분 비슷한 코드를 두 번 작성했다는 점이 맘에 걸리지만 어쨌든 잘 나오고 있네요!

하지만 여기에 메인 음료로 카페라떼, 카푸치노, 마끼아또.. 토핑으로 시나몬가루, 초코시럽, 바닐라시럽 이 추가된다면 어떨까요 ..? 얼마나 많은 클래스를 변경해야 할까요..?

## 추가되는 디자인 원칙 

이 지점에서 중요한 디자인 원칙을 하나 짚고 넘어가야 합니다.

위의 경우 메뉴와 토핑이 많아질수록 수정해야 하는 클래스와 범위가 넓어진다는 것이 문제였습니다. 생각만 해도 아찔한 이 문제들을, 많은 선배 개발자들이 미리 경험하고 최대한 해결하려 한 결과물이 디자인 패턴이라고 할 수 있죠. 이런 문제점들을 모아 '이렇게 하지 말라~' 고 디자인 원칙도 나왔을 것이구요. 

이 경우 해당되는 디자인 원칙은 <mark>클래스는 확장에 대해서는 열려 있어야 하지만 코드 변경에 대해서는 닫혀 있어야 한다.</mark> 는 점입니다.

이 문장만 따로 보았을 때는 이게 무슨 소린가 싶었지만 비슷한 경우를 미미하게나마 겪어보고 나니 확실히 이해가 가는군요. 적어도 __코드 변경에 대해서는 닫혀 있어야 한다.__ 는 부분은 말이죠.

사람은 누구나 실수를 하는 존재이기 때문에 아무리 완벽한 계획을 가지고 있는 사람이라도 실수하기 마련입니다. 하물며 변경해야 할 지점을 너무나도 많이 만들어놓고 실수하지 않길 바라는 것은 운이 좋길 바라는 것과 다를 게 없죠 ^^

이제 `Decorator Pattern` 을 이용하여 이 프로그램이 어떻게 __확장에 열려있는 클래스__ 가 되어가는지 확인해봅시다.

## Decorator Pattern 이란?

적용을 하려면 먼저 `Decorator Pattern` 이 뭔지에 대해 알아야겠죠. 먼저 `Decorator Pattern`에 대해 정의해보자면 아래와 같습니다.
> Decorator Pattern
>
> 객체에 추가적인 요건을 동적으로 첨가한다. 데코레이터는 서브클래스를 만드는 것을 통해서 기능을 유연하게 확장할 수 있는 방법을 제공한다.

무슨말인지 이해가 잘 안가네요 ~

지금까지 만들었던 커피 가격 계산 프로그램을 빌리자면 `Decorator Pattern` 을 이용하면 이런 순서로 작업이 진행된다 할 수 있습니다.
> 1. Americano 객체를 가져옵니다.
> 2. Sugar 객체로 장식합니다.
> 3. Whip 객체로 장식합니다.
> 4. cost() 메소드를 호출합니다. 이 때 가격을 계산하는 일은 각각의 객체들에게 위임됩니다.

이 순서에 따르면 `Americano` 클래스가 가장 먼저 호출되며 이를 `Sugar` 클래스로 감쌉니다. 그리고 이 `Sugar` 클래스를 `Whip` 클래스로 감싼 후 `Whip` 의 `cost()` 메소드를 호출합니다. 

`Whip` 클래스의 `cost()` 메소드는 `Sugar` 클래스의 `cost()` 메소드를 호출하고 `Sugar` 클래스의 `cost()` 메소드는 `Americano` 클래스의 `cost()` 메소드를 호출하죠. 이렇게 순서대로 호출된 `cost()` 메소드들은 거슬러 올라오며 각 클래스에 맞는 금액을 계산합니다.

여기에서 알아야 할 점은 세 클래스가 모두 `cost()` 메소드를 가지고 있다는 점이며 서로가 서로의 메소드를 호출하며 그 결과값을 전달받고 있다는 것입니다. 

이러한 점들을 어떻게 코드로 나타낼 수 있을까요? 

이해를 돕기 위해 예제를 먼저 작성해보도록 합시다.

abstract class 인 `Beverage` 클래스는 별로 다른 점이 없습니다. 
```java
public abstract class Beverage {
    String description = "";
    
    public String getDescription() {
        return this.description;
    }
    
    public abstract int cost();
}
```
기본 메뉴들도 모두 비슷하게 `Beverage` 클래스를 상속받아 작성합니다.
```java
public class Americano extends Beverage {
    public Americano() {
        this.description = "아메리카노";
    }
    
    @Override
    public int cost() {
        System.out.println("기본메뉴 아메리카노");
        return 3000;
    }
}
```
(`Espresso` 클래스도 금액과 설명만 다르고 모두 동일합니다.)

달라진 점이 있다면 토핑들입니다.

먼저 `Beverage` 클래스를 상속받은 데코레이터 `ToppingDecorator` 가 존재합니다.
```java
public abstract class ToppingDecorator extends Beverage {
    public abstract String getDescription();
}
```
`Beverage` 클래스를 상속받았기 때문에 `Beverage` 가 가지고 있는 것은 모두 가지고 있습니다. `cost()` 메소드까지요. 하지만 추상클래스이기 때문에 구지 구현을 하지 않았습니다. 실제로 사용할 녀석들은 추상클래스가 아닌 구현클래스 들이거든요.

그럼 Topping 에 해당하는 구현클래스를 작성해봅시다.

```java
public class Ice extends ToppingDecorator {
    private Beverage beverage;
    
    public Ice(Beverage beverage) {
        this.beverage = beverage;
    }
    
    @Override
    public String getDescription() {
        return "얼음";
    }
    
    @Override
    public int cost() {
        System.out.println("얼음 추가");
        return 500 + beverage.cost();
    }
}
```
(`Suger` 및 `Whip` 클래스 역시 가격과 설명만 다르고 모두 같습니다.)

Topping 클래스에서는 생성자를 이용하여 자신에게 메뉴(Beverage)가 전달되며 자신의 cost() 메소드를 통해 메뉴와 자신의 값을 합산하여 return 합니다. 그 이후는 신경쓰지 않습니니다. 

이제 변경된 프로그램을 사용해봅시다.
```java
public static void main(String[] args) {
    Beverage americano = new Americano();
    americano = new Whip(americano);
    americano = new Sugar(americano);
    System.out.println(americano.description + " : " + americano.cost());
}
```
```bash
설탕 추가
휘핑크림 추가
기본메뉴 아메리카노
가격 : 3500
```
`println()` 을 `cost()` 에 촐력하였더니 결과물이 뒤집혀 출력되는 것을 확인할 수 있습니다. 제일 마지막으로 감싼 `Suager` 클래스의 cost() 메소드까지 먼저 들어가 나머지 내용을 실행하며 최종 클래스까지 거쳐 나오는 것이죠. 

이렇게 되면 나중에 토핑이 추가되었다 해도 기본메뉴 커피가 추가되었다 해도 그 메뉴에 해당하는 클래스 하나만 추가하면 됩니다. 다른 지역에서 사용된 코드에 문제가 생길까 걱정할 필요도 이유도 없죠. 

하지만 `new Whip... new Suger...` 등을 매번 반복하는 행위가 다소 바보스럽게 느껴지기도 합니다. 기본 메뉴가 아메리카노 라면 아메리카노만 만들어지면 될 것 같은데 필요한 모든 것들이 생성되고 있네요. 

사실 데코레이터 패턴은 일반적으로 팩토리 패턴이나 빌더 패턴 같은 다른 패턴을 써서 만들고 사용하게 됩니다. 

이에 대해서는 팩토리 패턴 및 빌더 패턴이 등장하면 추가적으로 기술해보도록 하겠습니다 ^^

마지막으로 이번에 추가된 디자인 원칙까지 포함된, 현재까지의 디자인 원칙을 모두 확인해보며 글을 마치도록 하겠습니다. 

> - 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분으로부터 분리시킨다.
> - 구현이 아닌 인터페이스에 맞춰서 프로그래밍 한다.
> - 상속보다는 구성을 활용한다.
> - 서로 상호작용을 하는 객체 사이에서는 가능하면 느슨하게 결합하는 디자인을 사용해야 한다.
> - **클래스는 확장에 대해서는 열려 있어야 하지만 코드 변경에 대해서는 닫혀 있어야 한다.**