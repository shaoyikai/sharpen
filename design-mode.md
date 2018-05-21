

[面向接口编程](https://blog.csdn.net/legend404/article/details/52813476)

### 六大原则

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

5. 模板方法模式

定义一个操作中的算法的框架，而将一些步骤延迟到子类中。使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

```java

public abstract class AbstractClass { 
	// 基本方法 
	protected abstract void doSomething(); 
	// 基本方法 
	protected abstract void doAnything(); 
	// 模板方法 
	public void templateMethod(){ 
		/* 调用基本方法，完成相关的逻辑 */ 
		this.doAnything(); 
		this.doSomething(); 
	} 
}

public class ConcreteClass1 extends AbstractClass { 
	// 实现基本方法
	protected void doAnything() { 
		// 业务逻辑处理 
	} 
	protected void doSomething() { 
		// 业务逻辑处理 
	} 
}
public class ConcreteClass2 extends AbstractClass { 
	// 实现基本方法
	protected void doAnything() { 
		// 业务逻辑处理 
	} 
	protected void doSomething() { 
		// 业务逻辑处理 
	} 
}
```

抽象模板中的基本方法尽量设计为protected类型，符合迪米特法则，不需要暴露的属性或方法尽量不要设置为protected类型。实现类若非必要，尽量不要扩大父类中年的访问权限。

6. 建造者模式

在建造者模式中，有4个角色：
- product 产品类
- builder 抽象建造者
- concreteBuilder 具体建造者
- director 导演类
