---
    title: 工厂方法
    tags: 创建型模式
---

**亦称：** 虚拟构造函数、Virtual Constructor、Factory Method

**工厂方法**是一种[[ 创建型模式.md | 创建型设计模式 ]]，其在父类中提供一个创建对象的方法，允许子类决定实例化对象的类型。

#### 适合应用场景

**当你在编写代码的过程中，如果无法预知对象确切类别以及依赖关系时，可使用工厂方法。**

工厂方法将创建产品的代码与实际使用产品的代码分离， 从而能在不影响其他代码的情况下扩展产品创建部分代码。

**如果你希望用户能扩展你软件库或框架的内部组件， 可使用工厂方法**

继承可能是扩展软件库或框架默认行为的最简单方法。但是当你使用子类替代标准组件时，框架如何辨识出该子类？

解决方案是将各框架中构造组件的代码集中到单个工厂方法中，并在继承该组件之外允许任何人对该方法进行重写。

**如果你希望复用现有对象来节省系统资源，而不是每次都重新创建对象，可以使用工厂方法。**

在处理大型资源密集型对象（比如数据库连接、 文件系统和网络资源） 时， 你会经常碰到这种资源需求。

#### 实现方式

1. 让所有产品都遵循同一接口。该接口必须声明对所有产品都有意义的方法。
2. 在创建类中添加一个空的工厂方法。该方法的返回类型必须遵循通用的产品接口。
3. 在创建者代码中找到对于产品构造函数的所有引用。将他们依次替换为对于工厂方法的调用，同时将创建产品的代码移入工厂方法。你可能需要在工厂方法中添加临时参数来控制返回的产品类型。
工厂方法的代码看上去可能非常糟糕。其中可能会有复杂的`switch分支`运算符，用于选择各种需要实例化的产品类。
4. 现在，为工厂方法中的每种产品编写一个创建者子类，然后再子类中重写工厂方法，并将基本方法中的相关创建代码移动到工厂方法中。
5. 如果应用中的产品类型太多，那么为每个产品创建子类并无太大必要，这时你也可以在子类中复用基类中的控制参数。
6. 如果代码经过上述移动后，基础工厂方法中已经没有任何代码，你可以将其转变为抽象类。如果基础工厂方法还有其他语句，你可以将其设置为该方法的默认行为。
   
#### 优缺点
&#10004; 你可以避免创建者和具体产品之间的紧密耦合。
&#10004; 单一职责原则。你可以将产品创建代码放在程序的单一位置，从而使得代码更容易维护。
&#10004; 开闭原则。无需更改现有客户端代码，你就可以在程序中引入新的产品类型。
&#10007; 应用工厂方法模式需要引入许多新的子类，代码可能会因此变得更复杂。最好的情况是将该模式引入创建这类的现有层次结构中。 

#### 与其他模式的关系

* 在许多设计工作的初期都会使用<u>**[[FactoryMethods.md | 工厂方法]]**</u>（较为简单，而且可以更方便地通过子类进行定制），随后演化为使用<u>**[[AbstractFactory.md | 抽象工厂]]**</u>、<u>**[[Prototype.md | 原型]]**</u>或<u>**[[Builder.md | 生成器]]**</u>（更灵活但更复杂）。
* <u>**[[AbstractFactory.md | 抽象工厂]]**</u>模式通常基于一组**工厂方法**,但你也可以使用<u>[[Prototype.md | 原型]]</u>模式来生成这些类的方法。
* <u>**[[Prototype.md | 原型]]**</u>并不基于继承，因此没有继承的缺点。另一方面，原型需要对被复制对象进行复杂的初始化。**工厂方法**基于继承，但是它不需要初始化步骤。
* **工厂方法**是<u>**[[TemplateMethod.md | 模板方法]]**</u>的一种特殊形式。同时，工厂方法可以作为一个大型模板方法中的一个步骤。

#### 示例(Go)

由于 Go 中缺少类和继承等 OOP 特性， 所以无法使用 Go 来实现经典的工厂方法模式。 不过， 我们仍然能实现模式的基础版本， 即简单工厂。

在本例中， 将使用工厂结构体来构建多种类型的武器。

首先， 创建一个名为 `i­Gun`的接口， 其中将定义一支枪所需具备的所有方法。 然后是实现了 `iGun` 接口的 `gun`结构体类型。 两种具体的枪支——`ak47`与 `musket` ——两者都嵌入了枪支结构体， 且间接实现了所有的 `i­Gun`方法。

`gun­Factory`结构体将发挥工厂的作用， 即通过传入参数构建所需类型的枪支。 main.go 则扮演着客户端的角色。 其不会直接与 `ak47`或 `musket`进行互动， 而是依靠 g`un­Factory`来创建多种枪支的实例， 仅使用字符参数来控制生产。

##### iGun.go: 产品接口
``` Go
package main

type IGun interface {
    setName(name string)
    setPower(power int)
    getName() string
    getPower() int
}
```
##### gun.go: 具体产品
``` Go
package main

type Gun struct {
    name  string
    power int
}

func (g *Gun) setName(name string) {
    g.name = name
}

func (g *Gun) getName() string {
    return g.name
}

func (g *Gun) setPower(power int) {
    g.power = power
}

func (g *Gun) getPower() int {
    return g.power
}
```

##### ak47.go: 具体产品
``` Go
package main

type Ak47 struct {
    Gun
}

func newAk47() IGun {
    return &Ak47{
        Gun: Gun{
            name:  "AK47 gun",
            power: 4,
        },
    }
}
```

##### musket.go: 具体产品
``` Go
package main

type musket struct {
    Gun
}

func newMusket() IGun {
    return &musket{
        Gun: Gun{
            name:  "Musket gun",
            power: 1,
        },
    }
}
```

##### gunFactory.go: 工厂
``` Go
package main

import "fmt"

func getGun(gunType string) (IGun, error) {
    if gunType == "ak47" {
        return newAk47(), nil
    }
    if gunType == "musket" {
        return newMusket(), nil
    }
    return nil, fmt.Errorf("Wrong gun type passed")
}
```

##### main.go: 客户端代码

``` Go
package main

import "fmt"

func main() {
    ak47, _ := getGun("ak47")
    musket, _ := getGun("musket")

    printDetails(ak47)
    printDetails(musket)
}

func printDetails(g IGun) {
    fmt.Printf("Gun: %s", g.getName())
    fmt.Println()
    fmt.Printf("Power: %d", g.getPower())
    fmt.Println()
}
```

##### output.txt: 执行结果
```
Gun: AK47 gun
Power: 4
Gun: Musket gun
Power: 1
``` 