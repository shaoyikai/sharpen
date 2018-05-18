### 面向对象是对面向过程的一种扩展

面向过程，程序由“数据结构” + “算法”组成。
“数据结构”的种类是特定的，对于某种语言个数是固定的，比如C语言有int，string，bool等；
“算法”由各种函数与流程控制语句组成，它定义了对数据进行的操作。

面向对象主要比面向过程多了“类”的概念，可是这却导致了编程风格的巨大差异。“类”的基本构成是“属性” + “方法”。
“属性”类比于面向过程中的“数据结构”，“方法”类比于面向过程中的“算法”，“类”类比于面向过程中的特殊的“数据类型”，“对象”类比于用这种特殊类型创建的变量的值。

所以，面向对象中的“数据结构”不是固定的哪几种，而是通过“类”可以无限的扩展。而大千世界千姿百态，通过特定的几种“数据结构”来表示，难免会牵强附会。面向对象编程思想最终被业界广泛接受，正是因为它更好的抽象了世间万物。

>
面向对象的本质是设计并扩展自己的数据类型。设计自己的数据类型就是让类型与数据匹配。如果正确做到了这一点，将会发现以后使用数据时会容易得多。然而，在创建自己的类型之前，必须了解并理解C++内置的类型，因为这些类型是创建自己类型的基本组件。（引用自：《C++ Primer Plus (第6版) 中文版》）


套用古语：道生一，一生二，二生三，三生万物。

![Alt text](/images/2018/oop01.png "oop")


### 设计模式

[面向接口编程](https://blog.csdn.net/legend404/article/details/52813476)

#### 六大原则

1. 单一职责：接口职责单一，引起类的变化原因只有一个。职责不能无限制的划分，要根据实际业务需求来定。
2. 里氏替换：只要父类能出现的地方子类就可以出现，有子类出现的地方，父类未必就能适应。
3. 依赖倒置：高层模块不应该依赖低层模块，两者都应该依赖其抽象。模块间的依赖通过接口或抽象类发生，而不是通过实现类。
4. 接口隔离：把一个臃肿的接口变为多个独立的接口。通过分散定义多个接口，可以预防未来变更的扩散，提高系统的灵活性和可维护性。根据接口隔离拆分接口时，首先必须满足单一职责原则。
5. 迪米特法则：也称最少知识原则，一个对象应该对其他对象有最少的了解。
6. 开闭原则：对扩展开放，对修改关闭。要在设计之初考虑到所有可能变化的因素，然后留下接口，等待“可能”转变为“现实”。

### 主要设计模式

1. 单例模式

线程安全单例
```java
public class Singleton{
	private static final Singleton singleton = new Singleton();

	//限制产生多个对象
	private Singleton(){

	}
	//通过该方法获得示例对象
	public static Singleton getSingleton(){
		return singleton;
	}
	//类中其他方法，尽量是static
	public static void doSomething(){

	}
}
```

线程不安全单例
```java
public class Singleton{
	private static Singleton singleton = null;

	//限制产生多个对象
	private Singleton(){

	}
	//通过该方法获得示例对象
	public static Singleton getSingleton(){
		if(singleton == null){
			singleton = new Singleton();
		}
		return singleton;
	}
	//类中其他方法，尽量是static
	public static void doSomething(){

	}
}
```
该单例模式在低并发的情况下尚不会出现问题，若系统压力增大，并发量增加时则可能在内存中出现多个实例，破坏了最初的预期。


2. 简单工厂模式

```php
/**
*简单工厂模式与工厂方法模式比较。
*简单工厂又叫静态工厂方法模式，这样理解可以确定，
*简单工厂模式是通过一个静态方法创建对象的。
*/
interface people {
	function jiehun();
}
class man implements people{
	function jiehun() {
		echo '送玫瑰，送戒指！<br>';
	}
}
 
class women implements people {
	function jiehun() {
		echo '穿婚纱！<br>';
	}
}
 
class SimpleFactoty {
	// 简单工厂里的静态方法
	static function createMan() {
		return new man;
	}
	static function createWomen() {
		return new women;
	}
}
 
$man = SimpleFactoty::createMan();
$man->jiehun();
$man = SimpleFactoty::createWomen();
$man->jiehun();
```

3. 工厂方法模式

```php
<?php
/*
*工厂方法模式：
*定义一个创建对象的接口，让子类决定哪个类实例化。 
*他可以解决简单工厂模式中的封闭开放原则问题。
*/
interface people {
	function jiehun();
}

class man implements people {
	function jiehun() {
		echo '送玫瑰，送戒指！<br>';
	}
}
 
class women implements people {
	function jiehun() {
		echo '穿婚纱！<br>';
	}
}
 
//注意了，这里是简单工厂本质区别所在，将对象的创建抽象成一个接口。
interface createMan { 
	function create();
}

class FactoryMan implements createMan {
	function create() {
		return new man();
	}
}

class FactoryWomen implements createMan {
	function create() {
		return new women();
	}
}
 

$Factory = new FactoryMan();
$man = $Factory->create();
$man->jiehun();

$Factory = new FactoryWomen();
$man = $Factory->create();
$man->jiehun();

```

4. 抽象工厂模式

```php
<?php
/*
*抽象工厂：提供一个创建一系列相关或相互依赖对象的接口。
*注意：这里和工厂方法的区别是：一系列，而工厂方法则是一个。
*那么，我们是否就可以想到在接口create里再增加创建“一系列”对象的方法呢？
*/
interface people {
	function jiehun();
}
class Oman implements people{
	function jiehun() {
		echo '美女，我送你玫瑰和戒指！<br>';
	}
}
class Iman implements people{
	function jiehun() {
		echo '我偷偷喜欢你<br>';
	}
}
 
class Owomen implements people {
	function jiehun() {
		echo '我要穿婚纱！<br>';
	}
}
 
class Iwomen implements people {
	function jiehun() {
		echo '我好害羞哦！！<br>';
	}
}
 
//注意了，这里是本质区别所在，将对象的创建抽象成一个接口。
interface createMan { 
	function createOpen(); //外向
	function createIntro(); //内向
}

class FactoryMan implements createMan{
	function createOpen() {
		return new Oman();
	}

	function createIntro() {
		return new Iman();
	}
}
class FactoryWomen implements createMan {
	function createOpen() {
		return new Owomen();
	}
	function createIntro() {
		return new Iwomen();
	}
}

$Factory = new FactoryMan;
$man = $Factory->createOpen();
$man->jiehun();
$man = $Factory->createIntro();
$man->jiehun();

$Factory = new FactoryWomen;
$man = $Factory->createOpen();
$man->jiehun();
$man = $Factory->createIntro();
$man->jiehun();

```

区别：

- 简单工厂模式：用来生产同一等级结构中的任意产品。对与增加新的产品，无能为力
- 工厂模式：用来生产同一等级结构中的固定产品。（支持增加任意产品）   
- 抽象工厂：用来生产不同产品族的全部产品。（对于增加新的产品，无能为力；支持增加产品族）  

无论是简单工厂模式、工厂模式还是抽象工厂模式，它们本质上都是将不变的部分提取出来，将可变的部分留作接口，以达到最大程度上的复用。究竟用哪种设计模式更适合，这要根据具体的业务需求来决定。