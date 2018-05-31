---
layout: post
title: Object Oriented Design
date: 2018-05-27 15:32:24.000000000 +09:00
---

Why Learning OOD?
- Practical problems -> Model -> Code
- Better understanding of OOP
- Code details

1. Effective Java
2. Java Source Code 

What is good code? What is good design?
1. Complete functionality
2. Easy to use (by others)
    - clear, elegant, easy to understand, no ambiguilty
    - Prevent users from making mistakes
3. Easy to evolve

### Motivation of OOP

1. Structured programming: code + data
2. 如果只想暴露有限接口? (interface, API)
3. 重用某部分程序，同时根据不同情况进行扩展
4. 如何更好进行测试? (modular testing)

OOP优点: **Modular, Reusable** software systems

1. increased understanding. 通过class/object来描述系统，体现对系统的理解
2. Easy of maintenance. 封装encapsulation, 隐藏实现细节
3. Easy of evolution. inheritance继承重用已有代码，针对不同特例进行实现

`final 关键字：reference immutable`
- 如果是int, double修饰，无法改变
- 如果是List<>, Map<> 等class类型修饰，仍旧可以改变值

## Access Modifier

Modifier | class | package | sub-class | world
-------- | ----- | ------- | --------- | ----- 
public | Yes | Yes | Yes | Yes
protected | Yes | Yes | Yes | No
package-private(default) | Yes | Yes | No | No
private | Yes | No | No | No

## Inheritance

**Base class** (父类) **Derived class** (子类)

Employee -> Manager/Programmer

### 什么时候虽然没有 **abstract method** 但是仍然定义 **abstract class**？

当不希望new出这个abstract class的时候。比如 abstract class Employee {}

这个公司的所有Emoloyee规定必须有一个身份，要么是manager,要么是programmer,没有中间状态。那么就需要 abstract class。class Programmer extends Employee {}

### In java, 怎么选择 Interface vs. Abstract class

某些API在不同的情况下有不同的implement方法. 比如文件系统在不同情况下的读写创建文件夹操作是完全不一样的，此时base class只能提供Signature, 不能提供general default实现细节，此时就不能选择Concrete Class. 

Interface: 功能集合. 很抽象，只包含核心API
- **No data fields**
- **No method implementation**
- helps (or enforces) you to focus on the API signature definition
- sub-classes share **No** common implementation at all.
- **Multiple Inheritance**: 多继承，implement 多个 interface 
- The relationship between the interface itself and the class implementing the interface is not necessarily strong.
    - class House implements AirConditioning
    - class ArrayList implements Serializable

Abstract Class: 本身带有implementation 
- Use Abstract class for base class if the sub classes share common implementation.
    - Base class: House. 
    - Sub-classes: TownHouse, SingleFamilyHouse.
    - Share common implementation/attributes. hasYard(), level, age,... 

default 关键字 在interface中

给定default implementation，防止版本更新后此前implement interface操作全需要更改带来的overhead.

### Polymorphism and Overriding

A call to member function will cause a different function to be executed depending on the type of object that invokes the function. 

Example:

``` java
Employee e1 = new Coder(...);
e1.calculateSalary(0.95);

Employee e2 = new Manager(...);
e2.calculateSalary(0.85);
```

Compiling 阶段，e1, e2是一样的，都是Employee

**Runtime** 阶段, e1, e2找到对应的不同实现的过程，叫做Polymorphism

OOD Steps:
1. Understand/Analyze the use case. 明确这个程序/系统干什么
2. Classes and their relationships
    - Single-responsibility Principle
3. 搭建Skeleton. First focus on public methods(APIs): 别人如何调用程序？
    - discuss implementation details later

# Example: Design a parking lot
1. Describe the parking lot building? Vector monitoring? What kind of parking lot?
    - **Use cases** -> **APIs**: Always ask yourself: **input/output**
    - case: Parking
        - input: Given vehicle
        - output: canPark/parking ticket(记录起始时间，哪个车位)
    - Questions may affect your design:
        - One level or multiple levels?
        - Parking lot size / Vehicle sizes?
        - Track the location of each vehicles?
2. Vehicle, Parking spot, Level, Parking lot...
    - 当陈述一个design的时候，同时陈述潜在的理由
    - 思考什么时候适合引入Level. 当需要track每个Level有不同的性质
3. Functionality
    - Basic: Given vehicle, tell whether there is available spot in the parking lot.


## Design Pattern

### Builder Pattern

解决的问题：
1. too many member fields leads to a complicated construction method.
2. if we accept null value, leads to too many construction methods.
3. if we use setter method, it will demolish encapsulation

How to:
1. Create a static class named classBuilder()
2. implement set method in the builder class, return this
3. implement build() method, return class, with all value stored in Builder class

### Factory Pattern

Let **Object Creation** and **Object Usage** loose the coupling.

Benefit:
1. Separate instance/object creation logic from usage.
2. More clean especially when we have complicated instance creation logic
3. Easier to extend the instance creation logic. (extend the factory only)
4. Provide chances to have different object allocation strategies. (if we want to reuse shape objects)

#### Abstract Factory Pattern
针对不同factory有更好的extendability. 区别是有没有扩展factory的需求。 

### Singleton pattern

Ensure a class has only one instance, and provide a global point of access to it.

只有当明确只能有一个Object创建出来的时候，才使用SIngleton

Example: **Runtime** class in Java

``` java
public class Singleton {
    // 注意两个private一个public
    private static final Singleton INSTANCE = new Singleton();

    private Singleton() {} // private 构造函数，确保不被别人新建新的object

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```