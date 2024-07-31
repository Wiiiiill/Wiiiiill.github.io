---
draft: false
title: UML关系表达
---

示例是基于[PlantUML](https://plantuml.com/)的，使用文本描述即可生成UML图。

#### 1. 继承（Generalization）
继承关系表示类与类之间的父子关系，子类继承父类的属性和方法。
- **表示**：使用带实心三角箭头的实线，箭头指向父类。
- **示例**：`Person <|-- Student`

```plaintext
@startuml
class Person {
  +String name
  +int age
}

class Student extends Person {
  +int studentId
}

Person <|-- Student
@enduml
```

#### 2. 实现（Realization）
实现关系表示类与接口之间的关系，类实现接口定义的方法。
- **表示**：使用带空心三角箭头的虚线，箭头指向接口。
- **示例**：`Drivable <|.. Car`

```plaintext
@startuml
interface Drivable {
  +void drive()
}

class Car implements Drivable {
  +void drive()
}

Drivable <|.. Car
@enduml
```

#### 3. 关联（Association）
关联关系表示类与类之间的静态关系，一个类持有另一个类的实例。
- **表示**：使用实线，箭头可选，指向被关联的类。
- **示例**：`Person -- Address`

```plaintext
@startuml
class Person {
  +String name
}

class Address {
  +String street
  +String city
}

Person -- Address
@enduml
```

#### 4. 聚合（Aggregation）
聚合关系表示整体与部分的关系，部分可以独立于整体存在。
- **表示**：使用带空心菱形箭头的实线，菱形指向整体。
- **示例**：`Team o-- Player`

```plaintext
@startuml
class Team {
  +String name
}

class Player {
  +String name
}

Team o-- Player
@enduml
```

#### 5. 组合（Composition）
组合关系表示整体与部分的关系，部分不能独立于整体存在。
- **表示**：使用带实心菱形箭头的实线，菱形指向整体。
- **示例**：`House *-- Room`

```plaintext
@startuml
class House {
  +String address
}

class Room {
  +String name
}

House *-- Room
@enduml
```

#### 6. 依赖（Dependency）
依赖关系表示一个类依赖于另一个类，通常是方法参数或局部变量。
- **表示**：使用虚线，箭头指向被依赖的类。
- **示例**：`Order ..> Customer`

```plaintext
@startuml
class Order {
  +void placeOrder(Customer customer)
}

class Customer {
  +String name
}

Order ..> Customer
@enduml
```

### 综合示例

以下是一个包含多种关系的综合类图示例：

```plaintext
@startuml
interface Drivable {
  +void drive()
}

class Engine {
  +int horsepower
  +void start()
}

class Car implements Drivable {
  +Engine engine
  +void drive()
}

class Person {
  +String name
  +void driveCar(Car car)
}

Car *-- Engine : has
Person --> Car : drives
Car ..|> Drivable
@enduml
```

在这个示例中：
- `Car *-- Engine : has` 表示组合关系，`Car`包含一个`Engine`，但`Engine`不能独立存在。
- `Person --> Car : drives` 表示依赖关系，`Person`使用`Car`。
- `Car ..|> Drivable` 表示`Car`实现了`Drivable`接口。

