---
layout: post
title:  "[디자인 패턴] Observer pattern (옵저버 패턴)"
categories: [pattern]
img: use-post/design-pattern.jpg
toc: true
---

이번주에 알아볼 패턴은 **옵저버 패턴** 입니다. 분산 이벤트를 핸들링 해야 할 때 주로 사용한다 하며 발행/구독 모델로 알려져 있다고도 하네요. 

저번 시간과 동일하게 **옵저버** 라는 단어가 뭔지부터 확인해보도록 합시다.

## Observer 란?

---
> observer
> 명사
> 1. 보는 사람, 목격자
> 2. (회의·수업 등의) 참관인[옵서버]
> 3. 관찰자; 관측자

위와 같이 옵저버 란 지켜보는 것이라 할 수 있습니다. 게임에서도 여기 저기 관찰하기 위해 돌아다니는 것들을 옵저버 라고 하죠. 그렇다면 프로그래밍의 세계에서는 무엇을 가지고 옵저버라 칭하는 걸까요 ?

## 발행과 구독

---
위에서 옵저버 패턴은 발행/구독 모델로 알려져 있다고 언급했었습니다. '발행' 과 '구독'이라.. 너무나도 적당하여 옵저버 패턴을 설명할 때 항상 사용되는 예가 있죠. 

신문사가 있습니다. 신문사에서는 신문을 발행하죠. 이 신문을 정기적으로 받아보고 싶은 사람들은 신문사에 전화하여 구독을 신청하면 됩니다. 이 때 신문사에서 알 필요가 있는 정보는 고객의 집 주소 정도이죠. 그 주소로 신문을 보내면 그만이니까요. '누가' 신문을 구독하고 있는지는 중요치 않습니다. 더불어 신문을 받아보는 고객들은 그 날 신문을 어떤 기사로 구성했는지 신문사는 발행 후 무슨 일을 할 것인지, 다른 어떤 고객들이 구독신청을 했는지 알 필요가 없습니다. 그저 본인이 신청한 신문만 제시간에 집에 잘 도착하면 그만인 것이지요. 

단순한 예를 들어봤을 때에도 이 시스템을 구축하는 주요 요인이 하나만은 아니라는 생각이 들겁니다. 최소한 신문을 발행할 신문사와 그것을 구독할 구독자가 있어야 시스템이 돌아가겠죠. 현실 세계에서는 신문사를 발행자, 고객들을 구독자 라고 칭할 수 있을 것입니다. 이러한 구조를 옵저버 패턴으로 옮긴다면 당연히 <mark>발행자</mark> 와 <mark>구독자</mark> 가 있어야 할 것입니다.

## 발행자 (subject)

---
옵저버 패턴에서는 이런 발행자를 주로 **Subject(주제)** 라고 부릅니다. subject 들은 본인이 알고 있는 구독자 목록에 '어떤 것' 을 '특정 조건 달성 시' 발행하는 역할을 하죠. 

신문사로 예를 들면 조간신문 이라면 매일 아침 이 조건이 될 것이고 특보가 있다면 새롭고 급한 소식이 있을 때가 조건이 될 수 있겠습니다. subject 들은 여러 데이터를 가지고 있을 수 있으며 어떤 데이터가 변경되었을 때 알림을 줄 것인지 결정할 수 있습니다.

## 구독자 (Observer)

---
옵저버 패턴에서 subject 가 발행한 내용을 받을 구독자를 **Observer** 라고 부릅니다. 네, 패턴의 이름이 되는 옵저버는 여기에서 나온 것이죠. 패턴의 이름이 된다니, 얼마나 중요한지 감이 오시나요? 

보통 옵저버들은 자신이 관심있는 분야의 subject 에게 자신을 등록합니다. 등록은 subject 에게 지금 다른 observer 가 얼마나 등록되어 있는지 어떤 observer 가 등록되어 있는지 신경쓸 필요 없이 할 수 있습니다. 

observer 는 subject 에 구독을 등록해놓고 신경쓰지 않습니다. 자기의 할 일을 할 뿐입니다. 그러다 발행자에게서 알림이 오면 적당한 행동을 하죠. observer 에게는 이미 subject 에게서 알림이 왔을 때 해야 할 일이 정해져 있고 이에 따라 subject 에서 같은 알림이 올 때마다 해당 행동을 반복합니다. 

## Observer 패턴의 정의

---
옵저버 패턴을 한 마디로 정의해보자면 `한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식으로 일대다(one-to-many) 의존성을 정의한다.` 라고 할 수 있습니다. 

이 일대다 관계는 subject 와 observer 에 의해 정의됩니다. observer 는 subject 에 의존하게 되죠. 한 객체(subject) 의 상태가 변경되면 그 객체에 의존하고 있는 모든 객체(observers) 에게 연락이 갑니다. 

## 이벤트에 대한 처리

---
옵저버 패턴을 공부하기 전에는 알지 못했지만 옵저버 패턴의 행동방식을 알고 난 뒤에 보이는 것들이 있습니다. 

옵저버 패턴이 적용된 아주 대표적인 예에 해당하는 스윙 및 브라우저 등의 각종 이벤트 처리에 대한 것입니다. 스윙이나 브라우저의 화면에서 버튼이나 체크박스 등의 컴포넌트들을 사용자가 동작시켰을 때 실행되는 메서드들은 프로그램을 개발한 개발자가 직접 호출한 것이 아니죠. 그 메서드들은 특정 조건이 발동하였을 때 실행됩니다. 예를 들면, '버튼을 클릭했을 때', '체크박스에 체크되었을 때' 등이죠. 

메서드나 함수는 코드상에서 명시적으로 호출해주어야 동작합니다. `toString();` 과 같이 말이죠. 하지만 이벤트 발생시 실행되는 위와 같은 메서드, 함수들은 전혀 명시적으로 호출해 준 적이 없을텐데요, 어떻게 동작하는 걸까요? 예제를 통해 알아보도록 하겠습니다.

## Observer 와 Subject 인터페이스

---
observer 패턴은 기본적으로 인터페이스를 사용하여 구현합니다. 

반드시 구현되어야 하는 메소드를 포함한 인터페이스 Observer 와 Subject 가 존재하죠.
```java
public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObserver();
}

public interface Observer {
    void update();
}
```
Subject 인터페이스는 observer 를 등록, 삭제하는 메소드와 observer 에 변경사실을 알려줄 메소드를 포함하고 있습니다. Observer 인터페이스는 알림이 오면 실행될 메소드를 하나만 가지고 있을 뿐입니다. 둘 다 인터페이스이므로 Subject 나 Observer 역할을 하기 위해 어떤 클래스가 해당 인터페이스를 구현한다면 반드시 위 메소드들을 Override 해야 합니다. 

지금 사용할 예제는 기상스테이션 입니다. 기온, 습도, 기압 데이터가 갱신될 때마다 변경되는 데이터를 기계에 display 하는 프로그램이죠. 여기에서 기계는 여러가지가 될 수 있습니다. 이러한 프로그램을 작성해야 한다고 가정하고 Observer 패턴을 적용해봅시다. 

## 기상 스테이션 예제

---
#### **Subject**
변경될 데이터가 수신될 클래스는 WeatherData 클래스로 아래와 같이 생겼습니다.
```java
public class WeatherData implements Subject {
    private ArrayList<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData() {
        this.observers = new ArrayList<>();
    }
    @Override
    public void registerObserver(Observer o) {
        this.observers.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        int i = this.observers.indexOf(o);
        if (i >= 0) {
            this.observers.remove(i);
        }
    }

    @Override
    public void notifyObserver() {
        observers.forEach(observer -> observer.update(this.temperature, this.humidity, this.pressure));
    }
    
    public void measurementsChanged() {
        this.notifyObserver();
    }
    
    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        this.measurementsChanged();
    }
}
```
`WeatherData` 클래스는 `Subject` 인터페이스를 상속받아 Subject 역할을 하게 되었습니다. 이로 인해 `Subject` 인터페이스에서 구현을 강제하고 있는 메소드들을 구현하였죠. 데이터를 수신받아 변경되었다면 display 를 담당하고 있는 클래스들에게 데이터가 변경되었다는 것을 알려야 합니다. 대신 단순히 알려주기만 한다면 변경된 데이터를 표시해야 하는 display 측에서 어떤 데이터가 변경되었는지 알 수 없으므로 알려줄 때 변경된 데이터도 함께 알려주어야 합니다. 이로 인해 알림 메소드인 `notifyObserver()` 메소드에는 매개변수로 변경되는 데이터가 포함되었죠. 당연히 `Observer` 인터페이스도 아래와 같이 변경될 것입니다.
```java
public interface Observer {
    void update(float temp, float humidity, float pressure);
}
```
이외에도 변경된 데이터를 수신하는 메소드, 변경된 것을 알려주는 메소드가 포함되어 있습니다. 

#### **Observer**
Subject 인 `WeatherData` 는 자신이 가지고 있던 데이터가 변경되면 자신을 구독하고 있던 Observer 들에게 알립니다. 그 Observer 들은 observers 리스트에 자신을 등록함으로써 알림을 받을 수 있습니다. Observer 가 Subject 에게 알림을 받아야 하는 중요한 이유는 바로 그 변경된 데이터를 가지고 해야 하는 작업이 있기 때문이죠. 여기에서는 `display()` 가 그 중요한 작업입니다.
```java
public class CurrentConditionsDisplay implements Observer, DisplayElement {
    private float temperature;
    private float humidity;
    private Subject weatherData;

    public CurrentConditionsDisplay(Subject weatherData) {
        this.weatherData = weatherData;
        this.weatherData.registerObserver(this);
    }

    @Override
    public void display() {
        System.out.println("Current conditions: " + this.temperature + "F degrees and " + humidity + "% humidity");
    }

    @Override
    public void update(float temp, float humidity, float pressure) {
        this.temperature = temp;
        this.humidity = humidity;
        this.display();
    }
}
```
`DisplayElement` 는 모든 display 가 구현하기로 약속된 인터페이스이며 `display()` 메소드를 가지고 있습니다. `CurrentConditionsDisplay` 는 현재 상태를 나타내는 display 로 `Display` 인터페이스를 구현함으로써 자신이 display 임을 나타내고 `Observer` 인터페이스를 구현함으로써 Subject 로부터 데이터가 변경되었다는 사실과 변경된 데이터를 받아올 수 있습니다. 

 이는 `CurrentConditionsDisplay` 클래스가 생성될 때 Subject 인 `WeatherData` 객체를 매개변수로 받아 `WeatherData` 가 가지고 있는 옵저버 리스트에 본인을 등록함으로써 가능해집니다. 
 
 `Subject` 를 상속받은 `WeatherData` 는 무조건 Subject 가 가지고 있는 메소드들을 가지고 있을 것이고 Observer 인 `CurrentConditionsDisplay` 입장에서는 안전하게 자기 자신을 옵저버로 등록할 수 있을 것입니다. 
 
 하지만 `WeatherData` 의 옵저버 등록 메소드는 `Observer` 가 될 수 있는 클래스만 등록이 가능합니다. 그렇지 않은 클래스는 아예 입장조차 하지 못하죠. `Subject` 입장에서는 `Observer` 가 가지고 있는 `update()` 메소드를 가지고 있는 클래스라야 안전하게 `update()` 를 호출할 수 있을테니까요. 이 때문에 `WeatherData` 에 관심이 있는 display 들은 반드시 `Observer` 인터페이스를 구현해야만 합니다.
 
이제 필요한 것은 observer 와 subject 의 관계를 설정해 줄 클래스입니다. 설계도를 만들었으니 실제로 제작하고 사용해봐야겠지요. 사용처에서는 데이터를 입력할 수 있어야 하므로 데이터를 가지고 있는 `WeatherData` 클래스는 `WeatherStation` 클래스 에서 생성 후 변수로 가지고 있도록 합니다. 사용처인 main 에서 원하는 값을 입력해주었습니다.
```java
public class WeatherStation {
    private WeatherData weatherData;

    public WeatherStation() {
        this.weatherData = new WeatherData();
        new CurrentConditionsDisplay(this.weatherData);
    }

    public WeatherData getWeatherData() {
        return weatherData;
    }

    public static void main(String[] args) {
        WeatherStation station = new WeatherStation();
        station.getWeatherData().setMeasurements(80, 65, 30.4f);
    }
}
```
```text
Current conditions: 80.0F degrees and 65.0% humidity
```
입력된 값이 콘솔에 정상적으로 출력되었습니다. 실행처에서 직접 `display()` 메소드를 호출하지 않았지만 값을 입력한 것 만으로 `display()` 가 실행되었죠. display 를 아주 여러개를 만든다고 해도..
```java
public class WeatherStation {
    private WeatherData weatherData;

    public WeatherStation() {
        this.weatherData = new WeatherData();
        new CurrentConditionsDisplay(this.weatherData);
        new CurrentConditionsDisplay(this.weatherData);
        new CurrentConditionsDisplay(this.weatherData);
    }

    public WeatherData getWeatherData() {
        return weatherData;
    }

    public static void main(String[] args) {
        WeatherStation station = new WeatherStation();
        station.getWeatherData().setMeasurements(80, 65, 30.4f);
    }
}
```
```text
Current conditions: 80.0F degrees and 65.0% humidity
Current conditions: 80.0F degrees and 65.0% humidity
Current conditions: 80.0F degrees and 65.0% humidity
```
단 한 번 값이 세팅된 것 만으로 구독하고 있던 모든 `Display` 가 `display()` 를 알아서 실행합니다. 만약 아주 다른 display 라서 `Display` 클래스를 여러개 만들었다고 한다면 Subject 인 `WeatherData` 에 구독신청을 해주가면 하면 알아서 알림이 가죠. 이것이 Observer Pattern 의 정의에 거론되었던 <mark>1 : 다 관계</mark> 인 것입니다. 



한 번의 변경만으로 여러곳의 값이 바뀌어야 한다면 Observer pattern 을 사용하는 것이 아주 좋은 선택이 될 것입니다. Subject 는 Observer 에 대해서 별로 (그가 옵저버라는 것을 제외하면) 아는 것이 없고 Observer 입장에서도 새로운 Observer 가 탄생한다 해도 Subject 와 별 상관이 없습니다. 갑자기 모든 Observer 가 어떤 Subject 를 동시에 구독하지 않는다 하더라도 양쪽에 전혀 문제될 것이 없죠. 이렇게 둘은 서로 `느슨하게 결합(Loose Coupling)` 되어 있습니다. 여기서 네번째 디자인 원칙이 등장합니다. 
> 서로 상호작용을 하는 객체 사이에서는 가능하면 느슨하게 결합하는 디자인을 사용해야 한다.

Obsrver pattern 과 아주 딱 맞는 디자인 원칙이죠? 디자인 원칙이 등장한 김에 앞서 [Strategy pattern(전략패턴)](https://sirupe.github.io/strategy-pattern/) 포스팅에서 등장했던 3가지 디자인 원칙도 다시 한번 확인해보겠습니다.

> - 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분으로부터 분리시킨다.
> - 구현이 아닌 인터페이스에 맞춰서 프로그래밍 한다.
> - 상속보다는 구성을 활용한다.
> - **서로 상호작용을 하는 객체 사이에서는 가능하면 느슨하게 결합하는 디자인을 사용해야 한다.**

디자인 패턴을 적용할 때에는 위의 디자인 원칙을 숙고하여 적당한 디자인 패턴을 적용하는 것이 좋겠습니다. 아무리 디자인 패턴이 좋아도 필요없는 곳에 적용시키면 안되겠죠 ^^ 

JAVA 에서 직접 제공하는 Observer Pattern 기능도 있다고 합니다. 이 클래스와 인터페이스들은 다음에 포스팅하도록 하겠습니다 ^^ 


<br/><br/><br/>