# Android & JAVA Design Pattern

### (1) Strategy (2) Observer (3) Facade (4) Builder (5) Command



> Design Pattern이란, 많은 실무 프로그래머들이 인정한 효율적인 코딩 방법 또는 구조이다.

- 요구 사항 변경에 대한 소스코드 변경을 최소화 (유지보수)

- 함께 코딩하는 경우를 고려하여 범용적인 코딩 스타일 적용

  (1) 코딩이 명확하고 단순하다.

  (2) 모듈(Class or Method)은 한가지 기능만 하도록 작게 세분화.

  (3) 재사용성이 높다.

  (4) 유지 보수가 쉽다.

  (5) 리소스의 낭비가 없다.

---

### 1. Strategy Pattern (전략 패턴)

* 개요 : 동작은 다르지만, 서로 밀접한 관계를 가지는 여러 클래스에 대해 필요한 시점에 수행하는 클래스를 골라 사용하고자 할 때 사용하는 패턴.

  ex) 가장 쉬운 예로, Game에서 몬스터를 공격할 때, Attack() Method가 실행되는데 

  ​      현재 User의 무기에 대한 동작이 실행되는 경우 (주먹, 칼, 활, 총 ....)

  ​

* 내용

  기존에 동작에 대한 내용을 if, else 구문으로 무기 이름과 비교하여 무기에 따른 동작을 실행. 이런 구조에서 몇 가지의 무기가 새로 추가가 된다면.................if, else 문은 끝없이 이어질 것이다. 

  ```java
  void Attack(){
      if(weapon == "Fist"){
      }
      else if(weapon == "Sword"){
      }
      else if(weapon == "Bow"){
      }
      else if(weapon == "Etc"){
  	}
      .
      .
      .
  }
  ```



​	**계속 무기를 추가한다면? / 무기를 삭제한다면?**

​	--> 코드에서 변화하는 부분을 끝없는 if문으로 관리하는 것은 비효율적.

​       --> **변화하는 부분**을 분리해서 관리하는 것이 **스트래티지 패턴**이다.

​       

###How?

1. 무기에 대한 Interface 생성

   ```java
   interface IWeapon{
     	void Attack(Charater character);
   }
   ```

2. 검에 대한 Class 생성

   ```java
   Class Weapon_Sword implements IWeapon{
     	@Override
     	public void Attack(Character character){
         	character.display(this);
        	if(character.hasTargetMonster() == true){
             	...
               ...
               ...
   		} 	
     	}
   }
   ```

3.  이런식으로 검, 활, 지팡이 등, 변화하는 부분 마다 Class 생성.

4. (최종) Character Class

   ```java
   Class Character{
     	private IWeapon weapon;
     	...
     	Character(){
         	weapon = new Weapon_Fist();
     	}
       ...
       // 공격에 관한 Method.
       public void Attack(){
       	weapon.Attack(this); 
      }
      
      // 무기 바꾸기.
     public void setWeapon(IWeapon weapon){
       this.weapon = weapon;
     }
   }

   ```

   ​

### 정리

> 전략 패턴은 서로 행위만 다를 뿐, 밀접한 연관 관계를 가지는 여러 클래스를 사용할 때 유용.
>
> 조건문의 나열된 형태일 때 유용. 즉, 각각의 알고리즘을 캡슐화하여 이들을 상호 교환이 가능하도록 만듬.
>
> 특정 알고리즘을 임의로 선택해서 적용할 수 있게 해준다!
>
> 상속보다 **구성**을 활용한다. ( 구성이란 "A에 B가 있다" 라는 것을 의미하며 한 객체에 객체를 포함시키는 것이 아닌 인터페이스를 포함하는 방식을 말한다. )

* 장점 : 1) 로직을 독립적으로 구현할 수 있다.

  ​           2) 유지 보수가 쉽고 확장성이 좋다.

  ​           3) 동적으로 변경 가능하다.

  ​	

* 활용처 : 1) 상황에 따라 최적의 알고리즘을 선택해서 사용하도록 할 때  

  ​              2) if,else가 끝없이 이어질 경우

---

## 2. Observer Pattern (옵저버 패턴)

### 개요

옵저버 패턴은 **한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식**으로 일대다(1:N) 의존성을 정의합니다.



### 내용

- 쉬운 상황을 예로 들면, 다음과 같은 상황이 있습니다.

  1. 기상스테이션에서 옵저버 패턴 시스템에 주기적으로 온도, 습도, 압력 데이터를 전송한다. 
  2. 옵저버 패턴 시스템에서는, 전달받은 데이터를 등록된 디스플레이 장비에 전송하여 데이터를 갱신한다. 
  3. 디스플레이 장비는 해당 데이터를 사용하여 각 목적에 맞도록 데이터를 디스플레이 한다.

  : 내가 데이터를 받을 때마다 나의 정보를 구독하길 원하는 대상(옵저버)들에게 데이터를 던져주는 것.



### HOW?

①  JDK에서 지원하는 **Observable, Observer API** 활용 ( 패턴의 우수성을 인정받아 JAVA에서 채택!! )

- Observable 클래스와 Observer 인터페이스는 java.util 패키지에 들어 있는 자바 내장 API 입니다.

- **Observable 클래스는 등록된 옵저버들을 관리**하며, 새로운 데이터가 들어오면 등록된 **옵저버에게 데이터를 전달**합니다.

- Observer 인터페이스를 implements 하여 **등록된 옵저버들은 Observable로 부터 데이터를 받을 수 있습니다.**

- 이 자바 내장 옵저버 패턴을 사용하면 옵저버 패턴을 직접 구현할 필요 없이, 간단히 해당 클래스를 상속하면 됩니다.

  (1) WeatherData Class - Observer에게 전달할 Data를 담고있는 Class

  ```java
  import java.util.Observable;

  public class WeatherData extends Observable{      // java.util.Observable 클래스 상속

      private float temperature;                    // 온도
      private float humidity;                       // 습도
      private float pressure;                       // 기압
      
      public WeatherData(){

      }
      
      // 새로운 데이터를 전달 받아 갱신하고 새로운 데이터가 들어왔음을 알린다.
      // 기상스테이션(WeatherStation)에서는 주기적으로 이 함수를 사용해 최신 데이터를 전달한다.
      public void setMeasurements(float temperature, float humidity, float pressure){
          this.temperature = temperature;   
          this.humidity = humidity; 
          this.pressure = pressure;
          measurementsChanged();
      }

     // 갱신할 새로운 데이터 여부의 플래그 값을 변경하고 (setChanged())
     // 옵저버들에게 새로운 데이터를 전달한다. (notifyObservers())
     public void measurementsChanged(){
          setChanged();
          notifyObservers();
      }
      
     // 온도값 반환
      public float getTemperature(){
          return temperature;
      }

    // 습도값 반환
      public float getHumidity(){
          return humidity;
      }
      
    // 기압값 반환
      public float getPressure(){
          return pressure;
      }
  }
  ```

  (2) DisplayElment Interface - Data를 전달 받아서 표시할 옵저버

  ```java
  public interface DisplayElement {
      public void display();
  }
  ```

  (3) CurrentConditionDisplay Class - Observer와 DisplayElement를 Implements하여 구현하는 Observer.

  ```java

  public class CurrentConditionDisplay implements Observer, DisplayElement{   
      
      Observable observable;                            // 등록될 Observable
      private float temperature;                        // 온도
      private float humidity;                           // 습도
      
      public CurrentConditionDisplay(Observable observable){// 생성자 
          this.observable = observable;                     // 등록될 Observable을 import
          observable.addObserver(this);                     // CurrentConditionPlay 옵저버 등록
      }
      
      @Override
      public void update(Observable obs, Object arg){    // update 로 새로운 데이터 갱신
          if(obs instanceof WeatherData){                // Observable이 WeatherData인지 확인
              WeatherData weatherData = (WeatherData)obs;         // WeatherData로 변환
              this.temperature = weatherData.getTemperature();    // 온도 값 갱신
              this.humidity = weatherData.getHumidity();          // 습도값 갱신
              display();                                          // 최신 값 출력
          }
      }

      @Override
      public void display() {                                   
       System.out.println("현재 온도 : " + temperature + "도,  현재 습도 : " + humidity + "%");
          
      }

  }
  ```

  (4) ForecastDisplay Class - 위와 똑같은 역할의 Observer

  ```java
  blic class ForecastDisplay implements Observer, DisplayElement{    

      Observable observable;                             // 등록될 Observable
      private float currentPressure  = 29.92f;           // 현재 기압 (Default : 29.92f)
      private float lastPressure;                        // 마지막 기압

      public ForecastDisplay(Observable observable) {    // 생성자 
          this.observable = observable;                  // 등록될 Observable을 import
          observable.addObserver(this);                  // this(ForecastDisplay) 옵저버로 등록
      }

   	@Override
      public void update(Observable obs, Object arg){      // update 로 새로운 데이터 갱신
          if(obs instanceof WeatherData){                  // Observable이 WeatherData인지 확인
              WeatherData weatherData = (WeatherData)obs;  // WeatherData로 변환
              this.lastPressure = currentPressure;              // 온도 값 갱신
              this.currentPressure = weatherData.getPressure(); // 습도값 갱신
              display();                                        // 최신 값 출력
          }
      }

    	@Override
      public void display() {                                   // 출력
          System.out.print("Forecast: ");
          if (currentPressure > lastPressure) {
              System.out.println("기압 증가");
          } else if (currentPressure == lastPressure) {
              System.out.println("기압 변동 없음");
          } else if (currentPressure < lastPressure) {
              System.out.println("기압 하강");
          }
      }
  }
  ```

  -  Observable 클래스를 상속한 클래스를 만들고, 새로운 데이터가 들어오면 setChanged(), notifyObservers()를 호출하도록 구현한다.
  -  Observer 를 implements한 클래스를 만들고 Observable에 addObserver(this)로 자신을 Observer로 등록한다.*update() 함수를 구현하여, 전달받은 데이터를 처리해준다.

  (5) 실행

  ```java

  public class WeatherStation {

      static WeatherData weatherData;                    // weatherData Import
      static CurrentConditionDisplay currentDisplay;     //currentConditionDisplay
      static ForecastDisplay forecastDisplay;            // forecastDisplay
      
      public static void weatherStation(){            // weatherStation 초기화
          weatherData = new WeatherData();            // WeatherData 객체 생성
          
          currentDisplay = new CurrentConditionDisplay(weatherData); // observer 등록   
          forecastDisplay = new ForecastDisplay(weatherData);        // observer 등록
      }
      
      // WeatherData의 setMeasurements 함수 실행
      public static void changeWeather(float temp, float humity, float pressure) {  
          weatherData.setMeasurements(temp, humity, pressure);
      }
      
      
      public static void main(String[] args){
          
          weatherStation();                    // WeatherStation 생성
          
          // WeatherStation에서 날씨의 변화를 입력한다.
          System.out.println("-----날씨가 변한다.----");
          changeWeather(40, 50, 10);                    // WeatherData에 새로운 데이터 전송
                  
          System.out.println("");
          
          System.out.println("-----날씨가 변한다.----");
          changeWeather(50, 60, 20);                    // WeatherData에 새로운 데이터 전송
          
          System.out.println("");
                  
      }
  }
  ```

- Observable, Observer Class 파헤치기

  (1) Observer Class

  ```java
  package java.util;

  public interface Observer {
      void update(Observable o, Object arg);
  }
  ```

  (2) Observable Class

  ```java

  public class Observable {
    
      // 신규 데이터 여부 체크
      private boolean changed = false;             
      private Vector obs;

      // Observable 생성자
      public Observable() {                         
          obs = new Vector();
      }
    
      // Observer 등록
      public synchronized void addObserver(Observer observer) {      
          if (observer == null)
              throw new NullPointerException();
          if (!obs.contains(observer)) {
              obs.addElement(observer);
          }
      }
      
      // Observer 삭제
      public synchronized void deleteObserver(Observer observer) {  
          obs.removeElement(observer);
      }
    
      // Observer에 데이터 전달
      public void notifyObservers() {                        
          notifyObservers(null);
      }
    
      // Observer에 데이터 전달
      public void notifyObservers(Object arg) {              
      
          Object[] arrLocal;

          synchronized (this) {
          
              if (!changed)
                  return;
              arrLocal = obs.toArray();
              clearChanged();
          }

          for (int i = arrLocal.length-1; i>=0; i--)
              ((Observer)arrLocal[i]).update(this, arg);
      }
   
      //등록된 Observer 전체 제거
      public synchronized void deleteObservers() {       
          obs.removeAllElements();
      }
      // 신규 데이터 수신; 플래그값 변경
      protected synchronized void setChanged() {           
          changed = true;
      }
      // 신규 데이터 없음; 플래그값 변경
      protected synchronized void clearChanged() {         
          changed = false;
      }
      // 신규 데이터 여부 확인
      public synchronized boolean hasChanged() {         
          return changed;
      }
      // 현재 등록된 옵저버 수 조회
      public synchronized int countObservers() {            
          return obs.size();
      }
  }
  ```

  ​

  ####해당 API의 단점

  - Observable은 Interface가 아닌 Class이다. --> 활용도와 재사용성에 있어서 제약조건으로 작용하는 몇가지 문제점이 존재한다.
    - Observable이 Class이기 때문에 이를 상속받는 서브클랙스를 만들어야 한다는 것. **이미 다른 수퍼클래스를 확장하고 있는 Class에 Observable의 기능을 추가할 수 없다.**
    - Observable의 핵심 Method를 외부에서 호출할 수 없다. setChanged() Method가 protected로 선언되어 있어서 Observable의 서브클래스에서만 setChanged() Method를 호출 할 수 있다. 이는, 즉 **Observable의 서브 클래스를 인스턴스 변수로 사용하는 방법을 쓸 수 없다.**



② **직접 구현** 하는 Observer, Observable. - 느슨한 결합.How?

- Observable과 Observer는 서로 독립적으로 재사용할 수 있게 한다.

- Observable이나 Observer의 기능이 바뀌더라도 서로한테 영향을 미치지 않는다.

  (1)  Observable Class에서 Interface로 변경

  ```java
  public interface Observerable {
      
      public void addObserver(Observer observer);             // 옵저버 등록
      public void deleteObserver(Observer observer);          // 옵저버 제거
      public void notifyObservers();                         // 옵저버에 데이터 전달

  }
  ```

  (2) WeatherData Class - 옵저버에게 Data를 전달하는 Class

  ```java

  public class WeatherData implements Observable{            
      
      private ArrayList<Observer> observers;            // 등록할 옵저버 리스트
      private float temperature;                        // 온도
      private float humidity;                           // 습도
      private float pressure;                           // 기압
      
      public WeatherData(){                           // 생성자
          observers = new ArrayList<Observer>();      // 옵저버를 등록할 수 있는 ArrayList 생성
      }
      
      // 옵저버 등록
      public void addObserver(Observer observer){            
          observers.add(observer);
      }
      
      // 옵저버 제거
      public void deleteObserver(Observer observer){            
          int i = observers.indexOf(observer);                
          if(i>=0){
              observers.remove(i);
          }
      }
      
      // 옵저버에 데이터 전달
      public void notifyObservers(){                            
          for(int i=0; i<observers.size(); i++){
              Observer observer = (Observer)observers.get(i);
              // update로 옵저버에게 전달한다.
              observer.update(this);    
          }
      }
      
      // 최신 데이터 갱신, 기상 스테이션에서 이 메소드로 최신의 데이터를 던져준다.
      public void setMeasurements(float temperature, float humidity, float pressure){
          this.temperature = temperature;
          this.humidity = humidity;
          this.pressure = pressure;
          measurementsChanged();
      }
      
      // 데이터 변경 발생
      public void measurementsChanged(){
          notifyObservers();              // 데이터 변경 발생시 옵저버들에게 데이터를 전달한다.
      }
     
      // get, setter
  }

  ```

  ​

- 두 가지 방법 중 어떤 것이 더 좋은 방법일까?

  **정답은 X** : 그때 구현 목적, 향후 확장 및 변경을 고려하여 구현하자!

  - Observable API 사용 - 구현이 편하지만, 상속때문에 확장과 재사용성이 떨어진다.

### 정리

> Observer Pattern은 객체 지향 설계를 하다보면 객체들 사이에서 다양한 처리를 할 경우가 많은데, 한 객체의 상태가 바뀔 경우 다른 객체들에게 변경됐다고 알려주는 경우에 정말 **많이** 쓰이는 패턴이다.
>
> 부연 설명으로, 서로의 정보를 넘기고 받는 과정에서 정보의 단위가 클 수록, 객체들의 규모가 클 수록, 각 객체들의 관계가 복잡할 수록 복잡성이 매우 증가하는데, 이런 기능을 단순히 처리할 수 있게 만들어줄 수 있다.



---

## 3. Facade Pattern (퍼사드 패턴)

### 개요

퍼사드 패턴은, 많은 양의 코드를 단순화 된 인터페이스로 제공하는 객체이다. 

![facade_screenshow](http://cfile10.uf.tistory.com/image/1364C9374E8E72212BBEA6)

### 내용

(1) 각각의 Sub Class 생성

```java
public class Remote_Control {
    
    public void turn_On()
    {
        System.out.println("TV를 켜다");
    }
    public void turn_Off()
    {
        System.out.println("TV를 끄다");
    }
}
```

```java
public class Movie {
    
    private String name="";
    
    public Movie(String name)
    {
        this.name = name;
    }
    
    public void search_Movie()
    {
        System.out.println(name+" 영화를 찾다");
    }
    
    public void charge_Movie()
    {
        System.out.println("영화를 결제하다");
    }
    public void play_Movie()
    {
        System.out.println("영화 재생");
    }
 
}
```

```java
public class Beverage {
    
    private String name="";
    
    public Beverage(String name)
    {
        this.name = name;
    }
    
    public void prepare() {
        System.out.println(name+" 음료 준비 완료 ");
    }
 
}
```

```java

public class Facade {
    
    private String beverageName ="";
    private String movieName="";
    
    public Facade(String beverage,String Movie_Name)
    {
        this.beverage_Name=beverage_Name;
        this.Movie_Name=Movie_Name;
    }
    
    public void view_Movie()
    {
        Beverage beverage = new Beverage(beverageName);
        Remote_Control remote= new Remote_Control();
        Movie movie = new Movie(movieName);
        
        beverage.Prepare();
        remote.Turn_On();
        movie.Search_Movie();
        movie.Charge_Movie();
        movie.play_Movie();
    }
}

```

(2) SubClass 호출

```java
public class Viewer {
    
    public void view()
    {
        Facade facade = new Facade("콜라","어벤져스");
        facade.view_Movie();
    }
 
```

### 정리

> 1. SubClass 를 Upgrade해도 Client에는 아무 영향이 없다.
> 2. Facade Class에서 클라이언트 대신 모든 서브시스템 구성요소를 관리해준다.



- 활용처

  - Activity에서 도서 목록이 필요한 경우 저장소, 캐시 및 API등의 내부 동작을 이해하지 않고 하나의 객체를 사용하여 해당 목록을 요청하는 경우
  - [`Retrofit`](http://square.github.io/retrofit/)은 Facade 패턴을 구현하는데 도움이 되는 REST API 호출 라이브러리
    - 클라이언트는 책의 목록을 얻기위해서는 `listBooks()`만 호출하면 됨
    - `Retrofit`을 사용하면 `Interceptor` 및 `OkHttpClient`를 이용하여 클라이언트가 현재 진행 중인 상황을 알지 못해도 캐싱 동작을 제어 할 수 있음

  ```java
  public interface BooksApi {
      @GET("/books")
      void listBooks(Callback<List> callback);
  }
  ```


---



## 4. Builder Pattern

### 개요

> 생성 패턴이란, 객체 생성에 대해서 다루고 상황에 적절한 객체를 만드는 것이다. 이 패턴을 사용하면, 쉽고 간단한 객체 생성을 할 수 있다.

### 내용

- 복잡한 인스턴스를 조립하여 만드는 구조
- 생성자에 파라미터가 많은 클래스인 경우 가독성을 좋게 하기 위해 사용
- Android에서는 NotificationCompat.Builder 와 같은 클래스를 사용할 때 Builder 패턴이 나타남.

  (1)  Example 1 

  ```java
  Notification notification =new NotificationCompat.Builder(this)
                                      .setSmallIcon(R.drawable.ic_notification)
                                      .setContentIntent(pendingIntent)
                                      .setTicker(message)
                                      .build();
  ```

  (2 - 1)  Example 2 PersonInfoBuilder 객체

```java
public class PersonInfoBuilder {
    private String name;
    private Integer age;
    private String favoriteColor;
    private String favoriteAnimal;
    private Integer favoriteNumber;

    public PersonInfoBuilder setName(String name) {
        this.name = name;
        return this;
    }

    public PersonInfoBuilder setAge(Integer age) {
        this.age = age;
        return this;
    }

    public PersonInfoBuilder setFavoriteColor(String favoriteColor) {
        this.favoriteColor = favoriteColor;
        return this;
    }

    public PersonInfoBuilder setFavoriteAnimal(String favoriteAnimal) {
        this.favoriteAnimal = favoriteAnimal;
        return this;
    }

    public PersonInfoBuilder setFavoriteNumber(Integer favoriteNumber) {
        this.favoriteNumber = favoriteNumber;
        return this;
    }

    public PersonInfo build(){
        PersonInfo personInfo = new PersonInfo(name, age, favoriteColor, favoriteAnimal, favoriteNumber);
        return personInfo;
    }
}
```

(2- 2)  PersonInfoBuilder 생성

```java
PersonInfoBuilder personInfoBuilder = new PersonInfoBuilder();
        // 빌더 객체에 원하는 데이터를 입력합니다. 순서는 상관 없습니다.
 	    // 다시 같은 메소드를 호출한다면 나중에 호출한 값이 들어갑니다
        PersonInfo result = personInfoBuilder
                .setName("MISTAKE")
                .setAge(20)
                .setFavoriteAnimal("cat")
                .setFavoriteColor("black")
                .setName("JDM")    .
                .setFavoriteNumber(7)
                // 마지막에 .build() 메소드를 호출해서 최종적인 결과물을 만들어 반환합니다.
                .build();
```

### 정리

> - 불필요한 생성자를 만들지 않고 객체를 만든다.
> - **데이터 순서에 상관 없이** 객체를 만들어 낸다.
> - 사용자가 봤을 때 명시적이고 이해하기 쉽다.

### Tip

빌더 클래스를 꼭 객체를 만들어낼 클래스와 분리할 필요는 없다. 객체를 만들어낼 클래스 내부에 빌더 클래스를 포함할 수 도 있다!

```java
PersonInfo p = PersonInfo.Builder().setName("JDM").setAge(20).build();
```

이때의, Builder() Method는 Static으로 선언하고 빌더 클래스를 반환해주면 된다.

```java
/* PersionInfo.java */
public static PersonInfoBuilder Builder(){
    return new PersonInfoBuilder();
}
```



---



## 5. Command Pattern 

### 개요

> 프로그래밍을 하다보면 사용자가 선택한(또는 입력한) 명령어에 따라 그에 알맞은 처리를 해야 할 때가 있다. 
>
> 예를 들어, 워드프로세서를 생각해보자. 사용자들은 복사(copy), 잘라내기(cut), 붙여넣기(paste) 기능을 사용한다. 이 때 복사, 잘라내기, 붙여넣기 등은 모두 한번의 명령어에 해당한다. 사용자들은 메뉴나 툴바의 아이콘 또는 키보드 단축키를 사용함으로써 워드 프로세서에 이 명령들을 실행할 것을 요청하며, 워드 프로세서는 사용자가 전달한 명령어에 알맞은 기능을 실행한다. 
>
> 그리고 대부분의 워드 프로세서는 사용자가 실행한 명령을 취소할 수 있는 '명령 취소(Undo)' 기능을 제공하고 있다. 이러한 취소 기능을 제공하기 위해서는 사용자가 실행한 명령어들을 순서대로 저장할 수 있어야 한다. 
>
> 이처럼 명령어를 실행하고, 실행한 명령어를 저장하고, 실행한 명령을 취소하고, 재실행하고 또는 그러한 명령어를 처리할 때 사용되는 패턴이 바로 커맨드(Command) 패턴이다.



- 요청을 수행하는 객체가 별도의 수신자를 알지 못해도 요청을 실행하는 방식

- 요청을 별도의 객체로 캡슐화하고 전송

- [EventBus](https://github.com/greenrobot/EventBus)는 Android에 대표적인 Command 패턴을 보여주는 라이브러리

  ![command_pattern](https://github.com/greenrobot/EventBus/raw/master/EventBus-Publish-Subscribe.png)

### 내용

![command_pattern](http://cfs12.tistory.com/image/3/tistory/2008/12/08/09/58/493c71412d493)



- AbstractCommand 클래스는 모든 명령어 클래스가 상속받아야 할 클래스로서 추상 메소드인 execute()와 undo()를 선언하고 있다.
- 각각의 명령어 객체를 저장하고 관리해주는 관리자 클래스인 CommandManager 클래스가 필요하다. 이 클래스는 실행되는 명령어를 차례대로 저장하고 있으며, 사용자가 '명령취소'를 요청할 경우 가장 최근에 저장된 명령어 객체의 undo() 메소드를 호출해주는 역할을 한다. 즉, 전체적인 클래스 사이의 관계는 다음과 같다.

![command_pattern2](http://cfs12.tistory.com/image/33/tistory/2008/12/08/09/58/493c71413ac00)



(1) AbstractCommand 추상 Class

- CommandManager의 인스턴스를 Static으로 해서 Invoker가 CommandManager Class의 인스턴스를 단순히 생성시키기 위함.

```java
public abstract class AbstractCommand {
    public final static CommandManager manager = new CommandManager();
    
    // 이 객체가 캡슐화하고 있는 명령을 수행한다.
    public abstract boolean execute();
   
    // execute()를 통해서 수행된 작업을 취소한다.
    public abstract boolean undo();
}
```

( 2-1 ) PasteCommand - 복사 기능의 Class

- 특이할 만한 점: 생성자에서 CommandManager의 executeCommand() Method 호출
  - 명령어 객체를 사용하는 객체들은 CommandManager에 대한 자세한 내용을 알 필요 없이 객체 생성

```java
public class PasteCommand extends AbstractCommand {
    
  ....
      
    public PasteCommand(Document document, int position) {
        this.document = document;
        this.position = position;
        ....
        manager.executeCommand(this);
    }
  
    public boolean execute() {
        try {
            document.insertStringCommand(position, pasteString);
        } catch(Exception ex) {
            return false;
        }
        return true;
    }
  
    public boolean undo() {
        try {
            document.deleteStringCommand(position, pasteString.length() );
        } catch(Exception ex) {
            return false;
        }
        return true;
    }
}
```

( 2-2 ) PasteActionListener - 복사 기능의 Listener

    public class PasteActionListener implements ActionListener {
    
        public void actionPerformed(ActionEvent e) {
            // 현재 document와 position을 구한다.
            .....
            new PasteCommand(document, position); // 명령어 객체 생성
        }
    }

(2-3) Menu Class 

- 사용자들이 메뉴에서 "붙여넣기"를 클릭하면 actionPerformed() Method 호출되고 PasteCommand 객체 생성이 됨.
- 실제 사용자의 입력을 받는 MenuItem 객체와 실제 내부적으로 "붙여넣기"를 처리하는 PasteCommand 객체와 상관이 없다는 것을 알 수 있다.

    Menu menu = new Menu ("편집");
    MenuItem pasteMenuItem = new MenuItem("붙여넣기");
    menu.add(pasteMenuItem);
    pasteMenuItem.addActionListener(new PasteActionListener());

( 3-1 ) CommandManager Class

- history = 사용자가 입력한 명령어를 순서대로 저장하는 리스트.
- redoList = 사용자가 실행 취소한 것을 저장하는 리스트.

```java
class CommandManager {
    private static final int MAX_HISTORY_LENGTH = 50;

    private LinkedList history = new LinkedList();
    private LinkedList redoList = new LinedList();

    // 인자로 받은 AbstractCommand를 실행한다.
    // 만약 command가 Undo나 Redo의 인스턴스일 경우에는
    // 각각 '명령 취소'와 '취소 명령 재실행'을 수행한다.
    public void executeCommand(AbstractCommand command) {
        if (command instanceof Undo) {
            undo();
            return;
        }
        if (command instanceof Redo) {
            redo();
            return;
        }
        if (command.execute()) {
            addToHistory(command);
        } else { 
            // execute()가 false를 리턴한 경우, 즉 명령어가 올바르게 실행되지 않은 경우
            // 명령어가 실패했다는 것을 알린다.
        }
    }
    // 바로 이전에 실행한 명령어를 취소한다.
    private void undo() {
        if (history.size() > 0 ) { // 사용자가 실행한 명령어가 있을 경우
            AbstractCommand undoCommand = (AbstractCommand) history.removeFirst();
            undoCommand.undo();
            redoList.addFirst(undoCommand);
        }
    }
    // 바로 이전에 취소한 명령어를 다시 실행한다.
    private void redo() {
        if (redoList.size() > 0) {
            AbstractCommand redoCommand = (AbstractCommand) redoList.removeList();
            redoCommand.execute();
            history.addFirst(redoCommand);
        }
    }
    // history에 사용자가 실행한 명령어를 저장한다.
    private void addToHistory(AbstractCommand command) {
        history.addFirst(command);
        if (history.size() > MAX_HISTORY_LENGTH)
            history.removeLast();
    }
}
```

( 3-2 ) Undo와 Redo 는 Interface 이다.

```
interface Undo {}

interface Redo {}
```

( 3-3 ) UndoCommand : 명령 취소와 관련된 명령어 Class

```java
class UndoCommand extends AbstractCommand implements Undo {
    public UndoCommand() {
        manager.execute(this);
    }
    public boolean execute() { return false; }
    public boolean undo() { return false; }
}
```

( 3-4 ) RedoCommand : 실제로 사용자가 '명령 취소'를 클릭하면 이를 처리하는 이벤트 리스너는 단순히 다음과 같은 코드를 실행하면 된다.

```java
new RedoCommand();
```



### 정리

> 커맨드 패턴을 사용함으로써 사용자로부터 요청을 받는 객체(즉, 메뉴나 툴바)와 실제로 사용자가 필요로 하는 기능을 구현한 객체를 완전히 분리할 수 있게 되었다. 
>
> 따라서 사용자로부터 요청을 받는 객체는 실제 내부 로직이 어떻게 되는 지 알 필요가 없으며, 내부 로직을 구현한 명령어 객체 역시 사용자로부터 어떻게 요청을 받는 지 알 필요가 없다. 
>
> 즉, 이 두 객체 사이에 의존관계가 없는 것이다. 따라서 요청을 받는 **객체를 변경할 필요 없이 손쉽게 새로운 명령어 객체를 추가**할 수 있다.
