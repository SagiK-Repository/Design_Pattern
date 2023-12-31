문서정보 : 2023.10.20. 작성, 작성자 [@SAgiKPJH](https://github.com/SAgiKPJH)

<br>

# Design Pattern

- 디자인 패턴
  - 디자인 패턴은 소프트웨어 개발에서 반복적으로 발생하는 문제를 효율적으로 해결하기 위한 재사용 가능한 해결책을 의미한다.
  - 코드의 재사용성, 모듈성, 유지 보수성을 높이는데 도움이 된다.
- 대표적인 패턴으로는 다음 3가지가 있다.
  - 생성 패턴 : 객체가 생성되는 방식을 중점
  - 구조 패턴 : 클래스와 객체를 더 큰 구조로 구성
  - 행위 패턴 : 객체 간의 상호작용과 책임 분배에 초점

<br>

### 목차

- [x] [객체지향 5원칙 (SOLID 원칙)](#객체지향-5원칙-solid-원칙)
- [x] [0. 간단 노트 ](#0-간단-노트)
- [x] [1. 생성 패턴](#1-생성-패턴)
  - [x] [팩토리 패턴](#팩토리-패턴-factory-method)
  - [x] [추상화 팩토리](#추상-팩토리-패턴-abstract-factory)
  - [x] [빌더](#빌더-패턴-builder)
  - [x] [프로토 타입](#프로토-타입)
  - [x] [싱글톤](#싱글톤-패턴-singleton)
- [ ] 2. 구조 패턴
- [ ] 3. 행위 패턴

<br>

### 제작자
[@SAgiKPJH](https://github.com/SAgiKPJH)


<br>

---

# 객체지향 5원칙 (SOLID 원칙)

- SRP (Single Responsibility Principle)
  - 단일 책임 원칙
  - 객체는 오직 하나의 책임(하나의 변경의 이유만)을 가져야 한다.
  - 사칙연산 함수를 가지고 있는 계산 클래스가 있다고 치자. 이 상태의 계산 클래스는 오직 사칙연산 기능만을 책임진다.
  - 단일 책임 원칙은 클래스의 목적을 명확히 함으로써 구조가 난잡해지거나 수정 사항이 불필요하게 넓게 퍼지는 것을 예방하고 기능을 명확히 분리.
- OCP (Open-Closed Principle)
  - 개방-폐쇄 원칙
  - 확장에 대해 열려 있어야 하고, 수정에 대해서는 닫혀 있어야 한다.
  - 확장 : IShape -> Cirecle, Rectangle, 갈아끼울 수 있게 구성
  - 수정 : AreCalculator에서는 IShape에 또 다른 내용이 추가 되어도 수정 필요 없도록 구성
  - `갈아끼울거 추가해도 다른거 수정 없도록 구성`
- LSP (Liskov Substitution Principle)
  - 리스코프 치환 원칙
  - 자식 클래스는 언제나 자신의 부모 클래스를 대체할 수 있다.
  - 부모 자리에 자식클래스를 넣어도 잘 작동해야 한다.
- ISP (Interface Segregation Principle)
  - 인터페이스 분리 원칙
  - 클라이언트에서 사용하지 않는 메서드는 사용해선 안 된다. 
  - 클라이언트가 자신이 사용하지 않는 메서드에 의존하도록 강요받지 않아야 한다는 원칙.
  - 인터페이스를 다시 작게 나누어 만든다.
  - 인터페이스의 SRP
- DIP (Dependency Inversion Principle)
  - 의존성 역전의 원칙
  - 갈아끼울 수 있어야 한다.

<br><br>

# 0. 간단 노트

- SOLID 원칙(Principle)
  - SRP : 사칙연산을 책임지는 함수 (단일 책임, Single Responsibility)
  - OCP : 갈아끼울거(IShape 상속 -> Circle) 추가(확장 Open)해도 다른거(AreaCalculator) 수정 없도록(수정 Close) 구성 (개방-폐쇠, Open-Closed)
  - LSP : 참새와 타조 : Bird, 하지만 타조 Fly 불가. 더 분리해야 한다. (리스코프 치환, Liskov Substitution)
  - ISP : 인터페이스 SRP, 인터페이스로 인해 사용하지 않는 내용(함수)가 생기면 그 인터페이스를 분리하자. (인터페이스 분리, Interface Segregation)
  - DIP : 갈아끼우기 (의존성 역전, Dependency Inversion)
- 생성 패턴
  - 팩토리 패턴 : 객체 생성에 있어서, 서브 클래스에 위임하는 방법. 직접 생성 없이 팩토리 메서드로 원하는 동물 객체를 얻을 수 있다.
    ```cs 
    Dog youDog = newDog(); // 직접 생성 X하도록 합니다.

    IAnimal myDog = AnimalFactory.CreateAnimal("Dog");
    myDog.Speak();  // Outputs: Bark!
    
    IAnimal myCat = AnimalFactory.CreateAnimal("Cat");
    myCat.Speak();  // Outputs: Meow!
    ```
  - 추상 팩토리 : 팩토리의 팩토리, 시스템 독립 + 유연성 확보
    ```cs
    IAnimalFactory dogFactory = new DogFactory();
    IAnimal myDog = dogFactory.CreateANimal();
    myDog.Speak();  // Outputs: Bark!
    ```
  - 빌더 패턴 : 동일한 생성 절차에 다른 결과, 필요한 정보가 모두 입력될 때까지 객체 생성을 지연 후 생성 가능
    ```cs
    Car car = new CarBuilder() // 내부에 Car car가 있다.
                  .SetMake("Toyota") // Car 내부 변수 지정 1
                  .SetModel("Corolla") // Car 내부 변수 지정 2
                  .SetYear(2023) // Car 내부 변수 지정 3
                  .Build(); // Build() : Return car
    
    Console.WriteLine(car.ToString()); // Outputs: Make: Toyota, Model: Corolla, Year: 2023, // Car 함수
    ```
  - 프로토 타입 패턴 : ~~.Clone(), 기존 객체 동일 복제, 복제가 복잡한 경우 활용.
  - 싱글톤 패턴 : 클래스 내 공유자원으로 리소스 낭비 감소, private 생성자 + lock등을 활용.
- 구조 패턴

<br><br>

# 1. 생성 패턴

### 팩토리 패턴 (Factory Method)

- 객체를 생성하도록 하는 디자인 패턴
- 객체 생성 로직을 서브 클래스에게 위임
- 사용자가 직접 클래스 X, 팩토리 메서드를 통해 객체를 생성하도록 하는 디자인 패턴
- Example
  - 클라이언트 코드에서 직접 개나 고양이 객체를 생성하지 않고, 팩토리 메서드로 원하는 동물 객체를 얻을 수 있습니다.
```cs
Dog youDog = newDog(); // 직접 생성 X하도록 합니다.

IAnimal myDog = AnimalFactory.CreateAnimal("Dog");
myDog.Speak();  // Outputs: Bark!

IAnimal myCat = AnimalFactory.CreateAnimal("Cat");
myCat.Speak();  // Outputs: Meow!
```

<details><summary>접기/펼치기</summary>

- Code
```cs
public interface IAnimal
{
    void Speak();
}

public class Dog : IAnimal
{
    public void Speak()
    {
        Console.WriteLine("Bark!");
    }
}

public class Cat : IAnimal
{
    public void Speak()
    {
        Console.WriteLine("Meow!");
    }
}

public static class AnimalFactory
{
   public static IAnimal CreateAnimal(string animalType)
   {
       switch (animalType)
       {
           case "Dog":
               return new Dog();
           case "Cat":
               return new Cat();
           default:
               throw new ArgumentException("Invalid animal type");
       }
   }
}
```

</details>

<br>

### 추상 팩토리 패턴 (Abstract Factory)
- 팩토리의 팩토리
- 추상 팩토리 패턴은 관련된 객체 그룹을 함께 생성해야 할 때 유용
- 시스템을 독립적으로 만들되, 여전히 유연성을 유지할 수 있게 합니다.
- 새로운 종류의 객체 집합을 쉽게 통합할 수 있도록 지원하며, 이로써 시스템의 확장성을 높이는 데 도움
```cs
IAnimalFactory dogFactory = new DogFactory();
IAnimal myDog = dogFactory.CreateANimal();
myDog.Speak();  // Outputs: Bark!
```

<br>

### 빌더 패턴 (Builder)

- 복잡한 객체의 생성 과정과 표현 방법을 분리
- 동일한 생성 절차에서 서로 다른 표현 결과를 만들 수 있게 하는 디자인 패턴
- 빌더 패턴은 필요한 정보가 모두 입력될 때까지 객체 생성을 지연하고, 최종적인 결과물이 필요할 때까지 여러 단계에 걸쳐 구축되어야 할 경우 유용
```cs
Car car = new CarBuilder() // 내부에 Car car가 있다.
              .SetMake("Toyota")
              .SetModel("Corolla")
              .SetYear(2023)
              .Build(); // Build() : Return car

Console.WriteLine(car.ToString()); // Outputs: Make: Toyota, Model: Corolla, Year: 2023, // Car 함수
```

<details><summary>접기/펼치기</summary>

- Code
```cs
public class Car
{
    public string Make { get; set; }
    public string Model { get; set; }
    public int Year { get; set; }

    public override string ToString()
    {
        return $"Make: {Make}, Model: {Model}, Year: {Year}";
    }
}

public class CarBuilder
{
   private Car _car;

   public CarBuilder()
   {
       _car = new Car();
   }

   public CarBuilder SetMake(string make)
   {
       _car.Make = make;
       return this;
   }

   public CarBuilder SetModel(string model)
   {
       _car.Model = model;
       return this;
   }

   public CarBuilder SetYear(int year)
   {
       _car.Year = year;
       return this;
  }

  public Car Build()
  {
      return _car;
  }
}

```

</details>

<br>

### 프로토 타입

- 기존 객체를 복제하여 새로운 객체를 생성하는 디자인 패턴
- 객체 생성 비용이 높거나 복잡한 경우에 유용하며, 새로운 객체를 만들기 위해 동일한 초기화 과정을 반복하지 않고도 효율적으로 객체를 생성할 수 있다.
```cs
var clonedCircle = circle.Clone() as Circle;
clonedCircle.Draw();
```


<br>

### 싱글톤 패턴 (Singleton)

- 특정 클래스의 인스턴스가 오직 하나만 존재하도록 보장
- 전역적으로 접근 가능한 공유 인스턴스를 제공, 객체의 생성과 관련된 리소스 사용을 최소화
- 특징
  - private 생성자
```cs
Singleton instance1 = Singleton.GetInstance();
Singleton instance2 = Singleton.GetInstance();

Console.WriteLine(instance1 == instance2);  // Outputs: True, 한번 정의로 공통적으로 동작 (자원공유로 인한 리소스 낭비 감소)

instance1.SomeMethod();  // Call methods on the singleton instance
```


<details><summary>접기/펼치기</summary>

- Code
```cs
public class Singleton
{
    private static Singleton _instance;
    private static readonly object _lock = new object();

    // 외부에서 객체 생성 방지
    private Singleton() { }

    public static Singleton GetInstance()
    {
        if (_instance == null)
        {
            lock (_lock)
            {
                if (_instance == null)
                {
                    _instance = new Singleton();
                }
            }
        }
        return _instance;
    }

    public void SomeMethod()
    {
        // 싱글톤 객체의 동작 정의
    }
}
```

</details>

<br>

<br><br>

# 2. 구조 패턴

이어서 : https://namu.wiki/w/%EB%94%94%EC%9E%90%EC%9D%B8%20%ED%8C%A8%ED%84%B4

### 

<br>

### 

<br>

### 

<br>

### 

<br>

### 

<br>

### 

<br>

### 

<br>

<br><br>

# 3. 행위 패턴

### 

<br>

### 

<br>

### 

<br>

### 

<br>

### 

<br>

### 

<br>

### 

<br>

### 

<br>

### 

<br>

### 

<br>

### 

<br>

