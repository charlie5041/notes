---
title: C++ Design Patterns Documentation
tags: website, textbook
notebook: DesignPattern(C++)
---
<!-- TOC -->

- [Ch1. Design Principle](#ch1-design-principle)
  - [1.1 SOLID](#11-solid)
    - [Single Responsibility Principle(SRP)](#single-responsibility-principlesrp)
    - [Open-Closed Principle(OCP)](#open-closed-principleocp)
    - [Liskov Substitution Principle(LSP)](#liskov-substitution-principlelsp)
    - [Interface Segregation Principle(ISP)](#interface-segregation-principleisp)
    - [Dependency Inversion/Injection(DIP)](#dependency-inversioninjectiondip)
    - [Code Reuse](#code-reuse)
- [Ch2 Creational Design Patterns](#ch2-creational-design-patterns)
  - [Builder](#builder)

<!-- /TOC -->

# Ch1. Design Principle
## 1.1 SOLID

**S** = [Single Responsibility Principle(SRP)](#single-responsibility-principlesrp)

**O** = [Open-Closed Principle(OCP)](#open-closed-principleocp)

**L** = [Liskov Substitution Principle(LSP)](#liskov-substitution-principlelsp)

**I** = [Interface Segregation Principle(ISP)](#interface-segregation-principleisp)

**D** = [Dependency Inversion/Injection(DIP)](#dependency-inversioninjectiondip)

[**註釋:** SOLID目的也就是讓你程式碼 低耦合 高內聚 降低程式碼壞味道](http://bit.ly/32pAN87)

### Single Responsibility Principle(SRP)
- A class should only have a single responsibility.
- 降低單一類別被「改變」所影響的機會，在《Refactoring: Improving the Design of Existing Code》這本書中所提到的Divergent Change（發散式變化）就是一種類別違反SRP所形成的壞味道，類別設計符合SRP便可避免Divergent Change。

  [**補充:** 單一職責原則](http://bit.ly/3c8cTlL)

  [**UML**](http://bit.ly/2HWaBZh)

### Open-Closed Principle(OCP)
- Entities should be open for extension but closed for modiﬁcation.
- 當新增需求的時候（功能變化），在不改變現有程式碼的前提之下，藉由增加新的程式碼來實作新的功能。

  **註釋:** 實體需要容易擴展且不易修改
  
  [**補充:**](http://bit.ly/3ah6of1)
  1. 對於擴展是開放的 — 當需求變更時模組行為可以新增的
  2. 對於修改是封閉的 — 當進行擴展時，不需修改既有的程式碼

### Liskov Substitution Principle(LSP)
- Objects should be replaceable with instances of their subtypes without altering program correctness.
- LSP談的是繼承，也就是透過繼承時子類別所造成的「行為變化」要如何設計，才可以讓程式在runtime透過多型所繫結（bind）到的子類別實作，不至於違反父類別介面的合約。

  [**補充:**](http://bit.ly/2VmEhXn)
  
  1. 假設你繼承實作並且override的時候，必須先思考修改內容的行為方向是否符合父類別，如果行為不符合就意味著其實一開始就不屬於該父類別

### Interface Segregation Principle(ISP)
- Many client-speciﬁc interfaces better than one general-purpose interface.
- 針對不同的客戶端，只開放其所需要的介面讓它使用，如此一來可避免與降低客戶端因為不相關介面改變也一起被迫需要改變的問題。假設你有一個擁有10個函數的類別，現在有三個不同的客戶端都需要使用這個類別其中的4、7、5個函數。如果直接把這個類別傳給三個不同的客戶端，則這些客戶端很可能會因為它所沒使用到的函數改變了，也被迫跟著改變（因為原本類別的介面改變了）。另一種作法，則是針對三個不同的客戶端，提供三個不同的Adapter。透過Adapter，只開放客戶端所需的函數給它們，以隔離因為不相關介面改變所造成的客戶端改變。

  [**補充:**](http://bit.ly/2vfwfos)
  1. 不應該強迫用戶去依賴他們未使用的方法，最小化類別與類別之間的介面

### Dependency Inversion/Injection(DIP)
- Dependencies should be abstract rather than concrete.
**Dependency Inversion Principle**
1. **High-level modules should not depend on low-level modules. Both should depend on abstractions.** Exam-
ple: reporting component should depend on a ConsoleLogger, but can depend on an ILogger.
2. **Abstractions should not depend upon details. Details should depend upon abstractions.** In other words,
dependencies on interfaces and supertypes is better than dependencies on concrete types.
**Inversion of Control (IoC)** – the actual process of creating abstractions and getting them to replace dependencies.
**Dependency Injection** – use of software frameworks to ensure that a component’s dependencies are satisﬁed.
- DIP談的是caller和callee之間的關係，在兩者之間增加一個（抽象）介面，避免caller（上層程式）因為callee（底層程式）改變而被迫改變。

  [**補充:**](http://bit.ly/3a9YaVB)
  1. 高層模組不應該依賴於低層模組。兩者皆應該依賴抽象。
  2. 抽象不應該依賴細節。細節應該依賴抽象。

### Code Reuse

- 主要分為三大類: Inheritance, Composition, Parameterized Types (Templates). 其中各自的優缺點是:

1. Inheritance:
優: 繼承關係程式碼可讀性高, 架構容易理解.
缺: 繼承關係是在 compile-time 就決定, flexibility 低.
缺: sub-class 有權更動 super-class 內部資料與行為, 打破封裝. 反之, super-class 的更動也會影響 sub-class 的行為.

2. Composition:
優: 可以在 run-time 再決定 composition objects, flexibility 高.
優: 維持 composition objects 的封裝性.
缺: 程式可讀性低.

3. Templates: compile-time 定義.
書中的意見是: Composition 是比 Inheritance 好的 reuse 技術, Templates 則提供了第三種選項. 個人認為: Inheritance 與 Composition 使用時機上並不同. 有需要更動到 class 內部的要選用 Inheritance. 無此需求的時候, 當然就是選擇 Composition 了.

# Ch2 Creational Design Patterns
## Builder

- 將復雜對象的構造與其表示分開，可以使同一構建過程創建不同的表示。

![Alt text](\builder.png)

- 面臨到建構過程本身就複雜的狀況，這時如何組裝出一個物件就可能是要解決的問題了，這裡要提到的是建構者模式(Builder pattern)。
1. 這次的重點在餐點的產生過程比較複雜，最好能夠將過程侷限在服務人員身上，點餐的學生只要享用該完成品，不需知道，也不需參予整個過程。

2. 仔細分析，櫃台人員都有相同的四個動作，抽象的描述為：
1.)放上餐盤、2.)放上餐具、3.)放上餐點、4.)放上附湯飲料。

3. 所以我們可以想像，整個模型有兩個部分：
1.)執行四個動作：這部分兩個櫃台都一樣，見下圖的點餐服務。2.) 對四個動作的各自表述：有一個抽象的餐點Builder與繼承它的炒飯Builder、麵食Builder

![Alt text](builder2.png)

故，Builder Pattern如下圖所示:

![Alt text](builder3.png)

