---
    title: 抽象工厂
    tags: 创建型模式
---
**亦称：** Abstract Factory

**抽象工厂模式**是一种创建型设计模式， 它能创建一系列相关的对象， 而无需指定其具体类。

#### 适合应用场景

**如果代码需要与多个不同系列的相关产品交互， 但是由于无法提前获取相关信息， 或者出于对未来扩展性的考虑， 你不希望代码基于产品的具体类进行构建， 在这种情况下， 你可以使用抽象工厂。**

抽象工厂为你提供了一个接口， 可用于创建每个系列产品的对象。 只要代码通过该接口创建对象， 那么你就不会生成与应用程序已生成的产品类型不一致的产品。

**如果你有一个基于一组抽象方法的类， 且其主要功能因此变得不明确， 那么在这种情况下可以考虑使用抽象工厂模式。**

在设计良好的程序中， 每个类仅负责一件事。 如果一个类与多种类型产品交互， 就可以考虑将[[ FactoryMethods.md | 工厂方法]]抽取到独立的工厂类或具备完整功能的抽象工厂类中。

#### 实现方法

1. 以不同的产品类型与产品变体为维度绘制矩阵。

2. 为所有产品声明抽象产品接口。 然后让所有具体产品类实现这些接口。

3. 声明抽象工厂接口， 并且在接口中为所有抽象产品提供一组构建方法。

4. 为每种产品变体实现一个具体工厂类。

5. 在应用程序中开发初始化代码。 该代码根据应用程序配置或当前环境， 对特定具体工厂类进行初始化。 然后将该工厂对象传递给所有需要创建产品的类。

6. 找出代码中所有对产品构造函数的直接调用， 将其替换为对工厂对象中相应构建方法的调用。

#### 优缺点

&#10004; 你可以确保同一工厂生成的产品相互匹配。
&#10004; 你可以避免客户端和具体产品代码的耦合。
&#10004; 单一职责原则。 你可以将产品生成代码抽取到同一位置， 使得代码易于维护。
&#10004; 开闭原则。 向应用程序中引入新产品变体时， 你无需修改客户端代码。
&#10007; 由于采用该模式需要向应用中引入众多接口和类， 代码可能会比之前更加复杂。

#### 与其他模式的关系
* 在许多设计工作的初期都会使用<u>**[[FactoryMethods.md | 工厂方法]]**</u>（较为简单，而且可以更方便地通过子类进行定制），随后演化为使用<u>**[[AbstractFactory.md | 抽象工厂]]**</u>、<u>**[[Prototype.md | 原型]]**</u>或<u>**[[Builder.md | 生成器]]**</u>（更灵活但更复杂）。
* <u>**[[Builder.md | 生成器]]**</u>重点关注如何分布生成复杂对象。<u>**[[AbstractFactory.md | 抽象工厂]]**</u>专门用于生产一系列相关对象。抽象工厂会马上返回产品，生成器则允许你在获取产品前执行一些额外构造步骤。
* <u>**[[AbstractFactory | 抽象工厂]]**</u>模式通常基于一组<u>**[[FactoryMethods.md | 工厂方法]]**</u>，但你也可以使用<u>**[[Portotaype.md | 原型]]**</u>模式来生成这些类的代码。
* 当只需对客户端代码隐藏子系统创建对象的方式时，你可以使用<u>**[[AbstractFactory | 抽象工厂]]**</u>来代替<u>**[[Facade.md | 外观模式]]**</u>。
* 你可以将<u>**[[AbstractFactory | 抽象工厂]]**</u>和<u>**[[Bridge.md | 桥接模式]]**</u>搭配使用。 如果由桥接定义的抽象只能与特定实现合作， 这一模式搭配就非常有用。 在这种情况下， 抽象工厂可以对这些关系进行封装， 并且对客户端代码隐藏其复杂性。
* <u>**[[AbstractFactory | 抽象工厂]]**</u>、 <u>**[[Builder.md | 生成器]]**</u>和<u>**[[Prototype.md | 原型]]**</u>都可以用<u>**[[Singleton.md | 单例模式]]**</u>来实现。

#### 示例(Go)

假设一下， 如果你想要购买一组运动装备， 比如一双鞋与一件衬衫这样由两种不同产品组合而成的套装。 相信你会想去购买同一品牌的商品， 这样商品之间能够互相搭配起来。

如果我们把这样的行为转换成代码的话， 帮助我们创建此类产品组的工具就是抽象工厂， 便于产品之间能够相互匹配。

##### iSportsFactory.go: 抽象工厂接口
```GO
package main

import "fmt"

type ISportsFactory interface {
    makeShoe() IShoe
    makeShirt() IShirt
}

func GetSportsFactory(brand string) (ISportsFactory, error) {
    if brand == "adidas" {
        return &Adidas{}, nil
    }

    if brand == "nike" {
        return &Nike{}, nil
    }

    return nil, fmt.Errorf("Wrong brand type passed")
}
```

##### adidas.go: 具体工厂
``` Go
package main

type Adidas struct {
}

func (a *Adidas) makeShoe() IShoe {
    return &AdidasShoe{
        Shoe: Shoe{
            logo: "adidas",
            size: 14,
        },
    }
}

func (a *Adidas) makeShirt() IShirt {
    return &AdidasShirt{
        Shirt: Shirt{
            logo: "adidas",
            size: 14,
        },
    }
}
```

##### nike.go: 具体工厂
``` Go
package main

type Nike struct {
}

func (n *Nike) makeShoe() IShoe {
    return &NikeShoe{
        Shoe: Shoe{
            logo: "nike",
            size: 14,
        },
    }
}

func (n *Nike) makeShirt() IShirt {
    return &NikeShirt{
        Shirt: Shirt{
            logo: "nike",
            size: 14,
        },
    }
}
```

#####  iShoe.go: 抽象产品
``` Go
package main

type IShoe interface {
    setLogo(logo string)
    setSize(size int)
    getLogo() string
    getSize() int
}

type Shoe struct {
    logo string
    size int
}

func (s *Shoe) setLogo(logo string) {
    s.logo = logo
}

func (s *Shoe) getLogo() string {
    return s.logo
}

func (s *Shoe) setSize(size int) {
    s.size = size
}

func (s *Shoe) getSize() int {
    return s.size
}
```

##### adidasShoe.go: 具体产品
``` Go
package main

type AdidasShoe struct {
    Shoe
}
```

##### nikeShoe.go: 具体产品
``` Go
package main

type NikeShoe  struct {
    Shoe
}
```

#####  iShirt.go: 抽象产品
``` Go
package main

type IShirt interface {
    setLogo(logo string)
    setSize(size int)
    getLogo() string
    getSize() int
}

type Shirt struct {
    logo string
    size int
}

func (s *Shirt) setLogo(logo string) {
    s.logo = logo
}

func (s *Shirt) getLogo() string {
    return s.logo
}

func (s *Shirt) setSize(size int) {
    s.size = size
}

func (s *Shirt) getSize() int {
    return s.size
}
```

##### adidasShirt.go: 具体产品
``` Go
package main

type AdidasShirt struct {
    Shirt
}
```

##### nikeShirt..go: 具体产品
``` Go
package main

type NikeShirt  struct {
    Shirt
}
```


#### main.go: 客户端代码
``` GO
package main

import "fmt"

func main() {
    adidasFactory, _ := GetSportsFactory("adidas")
    nikeFactory, _ := GetSportsFactory("nike")

    nikeShoe := nikeFactory.makeShoe()
    nikeShirt := nikeFactory.makeShirt()

    adidasShoe := adidasFactory.makeShoe()
    adidasShirt := adidasFactory.makeShirt()

    printShoeDetails(nikeShoe)
    printShirtDetails(nikeShirt)

    printShoeDetails(adidasShoe)
    printShirtDetails(adidasShirt)
}

func printShoeDetails(s IShoe) {
    fmt.Printf("Logo: %s", s.getLogo())
    fmt.Println()
    fmt.Printf("Size: %d", s.getSize())
    fmt.Println()
}

func printShirtDetails(s IShirt) {
    fmt.Printf("Logo: %s", s.getLogo())
    fmt.Println()
    fmt.Printf("Size: %d", s.getSize())
    fmt.Println()
}
```

####  output.txt: 执行结果
```
Logo: nike
Size: 14
Logo: nike
Size: 14
Logo: adidas
Size: 14
Logo: adidas
Size: 14
```