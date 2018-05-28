

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

PHP示例
```php
<?php
/**
 * 建造者模式
 */
//Product：产品角色
class Product
{
    public function Add($part){
        $this->parts[]=$part;
    }
    public function show(){
        var_dump('产品创建');
        foreach ($this->parts as $part){
            var_dump($part);
        }
    }
}

//Builder：抽象建造者
interface Builder
{
    public function BuildPartA();
    public function BuildPartB();
    public function GetResult();
}

//ConcreteBuilder：具体建造者
class ConcreteBuilder1 implements Builder
{
    public function __construct(){
        $this->product=new Product();
    }
    public function BuildPartA()
    {
        $this->product->Add('部件A');
    }
    public function BuildPartB()
    {
        $this->product->Add('部件B');
    }
    public function GetResult()
    {
        return $this->product;
    }
}
class ConcreteBuilder2 implements Builder
{
    public function __construct(){
        $this->product=new Product();
    }
    public function BuildPartA()
    {
        $this->product->Add('部件X');
    }
    public function BuildPartB()
    {
        $this->product->Add('部件Y');
    }
    public function GetResult()
    {
        return $this->product;
    }
}

//Director：指挥者
class Director
{
    public function Construct($builder){
        $builder->BuildPartA();
        $builder->BuildPartB();
    }
}

$a=new Director();
$b1=new ConcreteBuilder1();
$b2=new ConcreteBuilder2();

$a->Construct($b1);
$p1=$b1->GetResult();
$p1->show();

$a->Construct($b2);
$p2=$b2->GetResult();
$p2->show();
```

运行结果

```ini
string '产品创建' (length=12)
string '部件A' (length=7)
string '部件B' (length=7)
string '产品创建' (length=12)
string '部件X' (length=7)
string '部件Y' (length=7)
```

7. 代理模式也叫委托模式

PHP示例
```php

//定义RealSubject和Proxy共同具备的东西
interface Subject{
    function say();
    function run();
}

class RealSubject implements Subject{
    private $name;

    function __construct($name){
        $this->name = $name;
    }

    function say(){
        echo $this->name."在吃饭<br>";
    }
    function run(){
        echo $this->name."在跑步<br>";
    }
}
class Proxy implements Subject{
    private $realSubject = null;
    function __construct(RealSubject $realSubject){
        $this->realSubject = $realSubject;
    }
    function say(){
        $this->realSubject->say();
    }
    function run(){
        $this->realSubject->run();
    }
}

//测试
$subject = new RealSubject("张三");
$proxy = new Proxy($subject);
$proxy->say();
$proxy->run();
```
我相信第一次接触到代理模式的读者肯定很郁闷，为什么要用代理呀？想想现实世界吧，打官司为什么要找个律师？因为你不想参与中间过程的是是非非，只要完成自己的答辩就成，其他的比如事前调查、事后追查都由律师来搞定，这就是为了减轻你的负担。代理模式的使用场景非常多，大家可以看看SpringAOP，这是一个非常典型的动态代理。

8. 原型模式

![Alt text](/images/2018/mode-prototype.png "原型模式")

【原型模式中主要角色】
抽象原型(Prototype)角色：声明一个克隆自身的接口

具体原型(Concrete Prototype)角色：实现一个克隆自身的操作

```php
<?php
/**
 * 原型模式 2010-06-27 sz
 * @author phppan.p#gmail.com  http://www.phppan.com
 * 哥学社成员（http://www.blog-brother.com/）
 * @package design pattern
 */
 
/**
 * 抽象原型角色
 */
interface Prototype {
    public function copy();
}
 
/**
 * 具体原型角色
 */
class ConcretePrototype implements Prototype{
 
    private  $_name;
 
    public function __construct($name) {
        $this->_name = $name;
    }
 
    public function setName($name) {
        $this->_name = $name;
    }
 
    public function getName() {
        return $this->_name;
    }
 
    public function copy() {
       /* 深拷贝实现
        $serialize_obj = serialize($this);  //  序列化
        $clone_obj = unserialize($serialize_obj);   //  反序列化                                                     
        return $clone_obj;
        */
        return clone $this;     //  浅拷贝
    }
}
 
/**
 * 测试深拷贝用的引用类
 */
class Demo {
    public $array;
}
 
class Client {
 
     /**
     * Main program.
     */
    public static function main() {
 
        $demo = new Demo();
        $demo->array = array(1, 2);
        $object1 = new ConcretePrototype($demo);
        $object2 = $object1->copy();
 
        var_dump($object1->getName());
        echo '<br />';
        var_dump($object2->getName());
        echo '<br />';
 
        $demo->array = array(3, 4);
        var_dump($object1->getName());
        echo '<br />';
        var_dump($object2->getName());
        echo '<br />';
 
    }
 
}
 
Client::main();
?>
```

浅拷贝

被拷贝对象的所有变量都含有与原对象相同的值，而且对其他对象的引用仍然是指向原来的对象。
即 浅拷贝只负责当前对象实例，对引用的对象不做拷贝。

深拷贝

被拷贝对象的所有的变量都含有与原来对象相同的值，除了那些引用其他对象的变量。那些引用其他对象的变量将指向一个被拷贝的新对象，而不再是原有那些被引用对象。

深拷贝要深入到多少层，是一个不确定的问题。

利用序列化来做深拷贝,把对象写到流里的过程是序列化（Serilization）过程，但在业界又将串行化这一过程形象的称为“冷冻”或“腌咸菜”过程；

而把对象从流中读出来的过程则叫做反序列化（Deserialization）过程，也称为“解冻”或“回鲜”过程。

在PHP中使用serialize和unserialize函数实现序列化和反序列化

[浅copy与深copy]https://shaoyikai.github.io/2018/05/24/2018-05-24-php-copy/

9. 中介者模式
10. 命令模式 
11. 责任链模式
12. 装饰模式
13. 策略模式
14. 适配器模式
15. 迭代器模式
16. 组合模式
17. 观察者模式
18. 门面模式
19. 备忘录模式
20. 访问者模式
21. 状态模式
22. 解释器模式
23. 享元模式
24. 桥梁模式