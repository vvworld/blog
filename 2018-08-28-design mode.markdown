---
layout: post
title: 菜鸟教程--设计模式的总结
date: 2018-08-28 00:00:00
tags: designMode
---
# 目录
* [工厂模式-创建型模式](#1)
* [抽象工厂模式-创建型模式](#2)
* [单例模式-创建型模式](#3)
* [建造者模式-创建型模式](#4)
* [原型模式-创建型模式](#5)
* [适配器模式-结构型模式](#6)
* [桥接模式--结构型模式](#7)
* [过滤器模式--结构型模式](#8)
* [组合模式--结构型模式](#9)
* [装饰器模式--结构型模式](#10)
* [外观模式--结构型模式](#11)
* [享元模式--结构型模式](#12)
* [代理模式--结构型模式](#13)
* [责任链模式--行为型模式](#14)
* [命令模式--行为型模式](#15)
* [解释器模式--行为型模式](#16)
* [迭代器模式--行为型模式](#17)
* [中介者模式--行为型模式](#18)
* [备忘录模式--行为型模式](#19)
* [观察者模式--行为型模式](#20)
* [状态模式--行为型模式](#21)
* [空对象模式](#22)
* [策略模式--行为型模式](#23)
* [模板模式--行为型模式](#24)
* [访问者模式--行为型模式](#25)

<h3 id='1'>工厂模式-创建型模式</h3>
[原文链接][1]{:target="_blank"}

在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。

```
public interface Shape{}
public class Rectangle implements Shape{}
public class Square implements Shape{}
public class Circle implements Shape{}
//工厂
public class ShapeFactory {
   //使用 getShape 方法获取形状类型的对象
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }        
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      return null;
   }
}
```

<h3 id='2'>抽象工厂模式-创建型模式</h3>
[原文链接][2]{:target="_blank"}

围绕一个超级工厂创建其他工厂,该超级工厂又称为其他工厂的工厂。
```
//形状接口
public interface Shape{}
public class Rectangle implements Shape{}
public class Square implements Shape{}
public class Circle implements Shape{}
//颜色接口
public interface Color{}
public class Red implements Color{}
public class Green implements Color{}
public class Blue implements Color{}
//为 Color 和 Shape 对象创建抽象类来获取工厂
public abstract class AbstractFactory {
   public abstract Color getColor(String color);
   public abstract Shape getShape(String shape) ;
}
//形状子工厂
public class ShapeFactory extends AbstractFactory {
   @Override
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }        
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      return null;
   }
   
   @Override
   public Color getColor(String color) {
      return null;
   }
}
//颜色子工厂
public class ColorFactory extends AbstractFactory {
   @Override
   public Shape getShape(String shapeType){
      return null;
   }
   
   @Override
   Color getColor(String color) {
      if(color == null){
         return null;
      }        
      if(color.equalsIgnoreCase("RED")){
         return new Red();
      } else if(color.equalsIgnoreCase("GREEN")){
         return new Green();
      } else if(color.equalsIgnoreCase("BLUE")){
         return new Blue();
      }
      return null;
   }
}
//创建一个工厂创造器/生成器类，通过传递形状或颜色信息来获取工厂。
public class FactoryProducer {
   public static AbstractFactory getFactory(String choice){
      if(choice.equalsIgnoreCase("SHAPE")){
         return new ShapeFactory();
      } else if(choice.equalsIgnoreCase("COLOR")){
         return new ColorFactory();
      }
      return null;
   }
}
```

<h3 id='3'>单例模式-创建型模式</h3>
[原文链接][3]{:target="_blank"}

单例类只能有一个实例 单例类必须自己创建自己的唯一实例 单例类必须给所有其他对象提供这一实例
1)懒汉式，线程不安全

```
public class Singleton {  
    private static Singleton instance;  
    private Singleton (){}
    public static Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
        return instance;  
    }  
}
```

2)懒汉式，线程安全

```
public class Singleton {  
    private static Singleton instance;  
    private Singleton (){}  
    public static synchronized Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
        return instance;  
    }  
}
```

3)饿汉式

```
public class Singleton {  
    private static Singleton instance = new Singleton();  
    private Singleton (){}  
    public static Singleton getInstance() {  
        return instance;  
    }  
}
```

4)双检锁/双重校验锁

```
public class Singleton {  
    private volatile static Singleton singleton;  
    private Singleton (){}  
    public static Singleton getSingleton() {  
        if (singleton == null) {  
            synchronized (Singleton.class) {  
                if (singleton == null) {  
                    singleton = new Singleton();  
                }
            }  
        }  
        return singleton;  
    }  
}
```

5)登记式/静态内部类

```
public class Singleton {  
    private static class SingletonHolder {  
        private static final Singleton INSTANCE = new Singleton();  
    }  
    private Singleton (){}  
    public static final Singleton getInstance() {  
        return SingletonHolder.INSTANCE;  
    }  
}
```

6)枚举

```
public enum Singleton {  
    INSTANCE;
}
```

<h3 id='4'>建造者模式-创建型模式</h3>
[原文链接][4]{:target="_blank"}

使用多个简单的对象一步一步构建成一个复杂的对像

```
public interface Item{}
//汉堡包
public abstract class Burger implements Item{}
public class VegBurger extends Burger{}
public class ChickenBurger extends Burger{}
//冷饮
public abstract class ColdDrink implements Item{}
public class Coke extends ColdDrink{}
public class Pepsi extends ColdDrink{}
//主食
public class Meal {
   private List<Item> items = new ArrayList<Item>();
   public void addItem(Item item){
      items.add(item);
   }
}
//创建一个MealBuilder类，实际的builder类负责创建 Meal 对象。
public class MealBuilder {
   public Meal prepareVegMeal (){
      Meal meal = new Meal();
      meal.addItem(new VegBurger());
      meal.addItem(new Coke());
      return meal;
   }
   public Meal prepareNonVegMeal (){
      Meal meal = new Meal();
      meal.addItem(new ChickenBurger());
      meal.addItem(new Pepsi());
      return meal;
   }
}
```

<h3 id='5'>原型模式-创建型模式</h3>
[原文链接][5]{:target="_blank"}

实现了一个原型接口，该接口用于创建当前对象的克隆。当直接创建对象的代价比较大时，则采用这种模式

```
//创建一个实现了Clonable接口的抽象类
public abstract class Shape implements Cloneable {
   public Object clone() {
      Object clone = null;
      try {
         clone = super.clone();
      } catch (CloneNotSupportedException e) {
         e.printStackTrace();
      }
      return clone;
   }
}
public class Rectangle extends Shape{}
public class Square extends Shape{}
public class Circle extends Shape{}
public class ShapeCache {
   private static Hashtable<String, Shape> shapeMap = new Hashtable<String, Shape>();
   public static Shape getShape(String shapeId) {
      Shape cachedShape = shapeMap.get(shapeId);
      return (Shape) cachedShape.clone();
   }
 
   // 对每种形状都运行数据库查询，并创建该形状
   // shapeMap.put(shapeKey, shape);
   // 例如，我们要添加三种形状
   public static void loadCache() {
      Circle circle = new Circle();
      circle.setId("1");
      shapeMap.put(circle.getId(),circle);
 
      Square square = new Square();
      square.setId("2");
      shapeMap.put(square.getId(),square);
 
      Rectangle rectangle = new Rectangle();
      rectangle.setId("3");
      shapeMap.put(rectangle.getId(),rectangle);
   }
}
```

<h3 id='6'>适配器模式-结构型模式</h3>
[原文链接][6]{:target="_blank"}

作为两个不兼容的接口之间的桥梁。

1）对象适配器模式--通过组合实现
```
//想在MediaPlayer接口实现类里播放AdvancedMediaPlayer中的mp4或vlc
public interface MediaPlayer {
   public void play(String audioType, String fileName);
}
public interface AdvancedMediaPlayer { 
   public void playVlc(String fileName);
   public void playMp4(String fileName);
}
public class VlcPlayer implements AdvancedMediaPlayer{
   @Override
   public void playVlc(String fileName) {
      System.out.println("Playing vlc file. Name: "+ fileName);      
   }
   @Override
   public void playMp4(String fileName) {}
}
public class Mp4Player implements AdvancedMediaPlayer{
   @Override
   public void playVlc(String fileName) {}
   @Override
   public void playMp4(String fileName) {
      System.out.println("Playing mp4 file. Name: "+ fileName);      
   }
}
//创建实现了MediaPlayer接口的适配器类
public class MediaAdapter implements MediaPlayer {
   AdvancedMediaPlayer advancedMusicPlayer;
   public MediaAdapter(String audioType){
      if(audioType.equalsIgnoreCase("vlc") ){
         advancedMusicPlayer = new VlcPlayer();       
      } else if (audioType.equalsIgnoreCase("mp4")){
         advancedMusicPlayer = new Mp4Player();
      }  
   }
 
   @Override
   public void play(String audioType, String fileName) {
      if(audioType.equalsIgnoreCase("vlc")){
         advancedMusicPlayer.playVlc(fileName);
      }else if(audioType.equalsIgnoreCase("mp4")){
         advancedMusicPlayer.playMp4(fileName);
      }
   }
}
```

2）类适配器模式--通过继承实现（忽略）
3）接口适配器模式
存在这样一个接口，其中定义了N多的方法，而我们却只想使用其中的一个到几个方法。

```
public interface A {
    void a();
    void b();
    void c();
    void d();
    void e();
    void f();
}
public abstract class Adapter implements A {
    public void a(){}
    public void b(){}
    public void c(){}
    public void d(){}
    public void e(){}
    public void f(){}
}
```

<h3 id='7'>桥接模式--结构型模式</h3>
[原文链接][7]{:target="_blank"}

提供抽象化和实现化之间的桥接结构，来实现二者的解耦

```
public interface DrawAPI{}
public class RedCircle implements DrawAPI{}
public class GreenCircle implements DrawAPI{}
public abstract class Shape {
   protected DrawAPI drawAPI;
   protected Shape(DrawAPI drawAPI){
      this.drawAPI = drawAPI;
   }
   public abstract void draw();  
}
public class Circle extends Shape {
   private int x, y, radius;
   public Circle(int x, int y, int radius, DrawAPI drawAPI) {
      super(drawAPI);
      this.x = x;  
      this.y = y;  
      this.radius = radius;
   }
   public void draw() {
      drawAPI.drawCircle(radius,x,y);
   }
}
```

定义抽象类，抽象类里含有别抽象类，以达到调用别抽象类的子类实例化对象。

<h3 id='8'>过滤器模式--结构型模式</h3>
[原文链接][8]{:target="_blank"}

使用不同的标准来过滤一组对象，通过逻辑运算以解耦的方式把它们连接起来

···
public class Person {}
public interface Criteria {
   public List<Person> meetCriteria(List<Person> persons);
}
public class CriteriaMale implements Criteria {
   @Override
   public List<Person> meetCriteria(List<Person> persons) {
      List<Person> malePersons = new ArrayList<Person>(); 
      for (Person person : persons) {
         if(person.getGender().equalsIgnoreCase("MALE")){
            malePersons.add(person);
         }
      }
      return malePersons;
   }
}
public class CriteriaFemale implements Criteria {
   @Override
   public List<Person> meetCriteria(List<Person> persons) {
      List<Person> femalePersons = new ArrayList<Person>(); 
      for (Person person : persons) {
         if(person.getGender().equalsIgnoreCase("FEMALE")){
            femalePersons.add(person);
         }
      }
      return femalePersons;
   }
}
public class CriteriaSingle implements Criteria {
   @Override
   public List<Person> meetCriteria(List<Person> persons) {
      List<Person> singlePersons = new ArrayList<Person>(); 
      for (Person person : persons) {
         if(person.getMaritalStatus().equalsIgnoreCase("SINGLE")){
            singlePersons.add(person);
         }
      }
      return singlePersons;
   }
}
public class AndCriteria implements Criteria {
   private Criteria criteria;
   private Criteria otherCriteria;
   public AndCriteria(Criteria criteria, Criteria otherCriteria) {
      this.criteria = criteria;
      this.otherCriteria = otherCriteria; 
   }
   @Override
   public List<Person> meetCriteria(List<Person> persons) {
      List<Person> firstCriteriaPersons = criteria.meetCriteria(persons);     
      return otherCriteria.meetCriteria(firstCriteriaPersons);
   }
}
public class OrCriteria implements Criteria {
   private Criteria criteria;
   private Criteria otherCriteria;
   public OrCriteria(Criteria criteria, Criteria otherCriteria) {
      this.criteria = criteria;
      this.otherCriteria = otherCriteria; 
   }
   @Override
   public List<Person> meetCriteria(List<Person> persons) {
      List<Person> firstCriteriaItems = criteria.meetCriteria(persons);
      List<Person> otherCriteriaItems = otherCriteria.meetCriteria(persons);
      for (Person person : otherCriteriaItems) {
         if(!firstCriteriaItems.contains(person)){
           firstCriteriaItems.add(person);
         }
      }  
      return firstCriteriaItems;
   }
}
···

<h3 id='9'>组合模式--结构型模式</h3>
[原文链接][9]{:target="_blank"}

表示部分以及整体层次，创建了对象组的树形结构。

```
public class Employee{
    private List<Employee> subordinates;
    public static void main(String[] args) {
        Employee CEO = new Employee("John","CEO", 30000);
        Employee headSales = new Employee("Robert","Head Sales", 20000);
        Employee headMarketing = new Employee("Michel","Head Marketing", 20000);
        Employee clerk1 = new Employee("Laura","Marketing", 10000);
        Employee clerk2 = new Employee("Bob","Marketing", 10000);
        Employee salesExecutive1 = new Employee("Richard","Sales", 10000);
        Employee salesExecutive2 = new Employee("Rob","Sales", 10000);
        
        CEO.add(headSales);
        CEO.add(headMarketing);
        
        headSales.add(salesExecutive1);
        headSales.add(salesExecutive2);
        
        headMarketing.add(clerk1);
        headMarketing.add(clerk2);
        
        //打印该组织的所有员工
        System.out.println(CEO); 
        for (Employee headEmployee : CEO.getSubordinates()) {
            System.out.println(headEmployee);
            for (Employee employee : headEmployee.getSubordinates()) {
                System.out.println(employee);
            }
        }
    }
}
```

<h3 id='10'>装饰器模式--结构型模式</h3>
[原文链接][10]{:target="_blank"}

允许向一个现有的对象添加新的功能，同时又不改变其结构

```
public interface Shape{
     void draw();
}
public class Rectangle implements Shape{
    @Override
    public void draw() {
        System.out.println("Shape: Rectangle");
    }
}
public class Circle implements Shape{
    @Override
    public void draw() {
    System.out.println("Shape: Circle");
    }
}
public class RedShapeDecorator() implements Shape {
   private Shape decoratedShape;
   public ShapeDecorator(Shape decoratedShape){ this.decoratedShape = decoratedShape;}
   @Override
   public void draw() {
      decoratedShape.draw();         
      System.out.println("Border Color: Red");
   }
   public static void main(String[] args){
        Shape redCircle  = new RedShapeDecorator(new Circle());
        Shape redRectangle = new RedShapeDecorator(new Rectangle());
   }
}
```

<h3 id='11'>外观模式--结构型模式</h3>
[原文链接][11]{:target="_blank"}

隐藏系统的复杂性，并向客户端提供了一个客户端可以访问系统的接口

```
public interface Shape{}
public class Rectangle implements Shape{}
public class Square implements Shape{}
public class Circle implements Shape{}
//外观类
public class ShapeMaker {
   private Shape circle;
   private Shape rectangle;
   private Shape square;
 
   public ShapeMaker() {
      circle = new Circle();
      rectangle = new Rectangle();
      square = new Square();
   }
 
   public void drawCircle(){
      circle.draw();
   }
   public void drawRectangle(){
      rectangle.draw();
   }
   public void drawSquare(){
      square.draw();
   }
}
```

<h3 id='12'>享元模式--结构型模式</h3>
[原文链接][12]{:target="_blank"}

主要用于减少创建对象的数量，以减少内存占用和提高性能。享元模式尝试重用现有的同类对象，如果未找到匹配的对象，则创建新对象。

```
public interface Shape{}
public class Circle implements Shape{}
public class ShapeFactory {
   private static final HashMap<String, Shape> circleMap = new HashMap<>();
   public static Shape getCircle(String color) {
      Circle circle = (Circle)circleMap.get(color);
      if(circle == null) {
         circle = new Circle(color);
         circleMap.put(color, circle);
         System.out.println("Creating circle of color : " + color);
      }
      return circle;
   }
}
```

<h3 id='13'>代理模式--结构型模式</h3>
[原文链接][13]{:target="_blank"}

<h3 id='14'>责任链模式--行为型模式</h3>
[原文链接][14]{:target="_blank"}

在这种模式中，通常每个接收者都包含对另一个接收者的引用。如果一个对象不能处理该请求，那么它会把相同的请求传给下一个接收者，依此类推。

```
public abstract class AbstractLogger {
   public static int INFO = 1;
   public static int DEBUG = 2;
   public static int ERROR = 3;
 
   protected int level;
   //责任链中的下一个元素
   protected AbstractLogger nextLogger;

   public void logMessage(int level, String message){
      if(this.level <= level){
         write(message);
      }
      if(nextLogger !=null){
         nextLogger.logMessage(level, message);
      }
   }
   protected abstract void write(String message);
}
```

<h3 id='15'>命令模式--行为型模式</h3>
[原文链接][15]{:target="_blank"}

请求包裹在命令对象中，并传给调用对象。调用对象寻找可以处理该命令的合适的对象，并把该命令传给相应的对象，该对象执行命令。

```
//命令接口
public interface Order {
   void execute();
}
//请求类
public class Stock {
   public void buy(){}
   public void sell(){}
}
public class BuyStock implements Order {
   private Stock abcStock;
   public BuyStock(Stock abcStock){
      this.abcStock = abcStock;
   }
   public void execute() {
      abcStock.buy();
   }
}
public class SellStock implements Order {
   private Stock abcStock;
   public SellStock(Stock abcStock){
      this.abcStock = abcStock;
   }
   public void execute() {
      abcStock.sell();
   }
}
//命令调用类
public class Broker {
   private List<Order> orderList = new ArrayList<Order>(); 
   public void takeOrder(Order order){
      orderList.add(order);      
   }
 
   public void placeOrders(){
      for (Order order : orderList) {
         order.execute();
      }
      orderList.clear();
   }
}
```
<h3 id='16'>解释器模式--行为型模式</h3>
[原文链接][16]{:target="_blank"}

提供了评估语言的语法或表达式的方式。实现了一个表达式接口，该接口解释一个特定的上下文。这种模式被用在 SQL 解析、符号处理引擎等。


<h3 id='17'>迭代器模式--行为型模式</h3>
[原文链接][17]{:target="_blank"}

用于顺序访问集合对象的元素，不需要知道集合对象的底层表示

```
public interface Iterator {
   public boolean hasNext();
   public Object next();
}
public interface Container {
   public Iterator getIterator();
}
//创建实现了Container接口的实体类。该类有实现了Iterator接口的内部类NameIterator。
public class NameRepository implements Container {
   public String names[] = {"Robert" , "John" ,"Julie" , "Lora"};
 
   @Override
   public Iterator getIterator() {
      return new NameIterator();
   }
 
   private class NameIterator implements Iterator {
      int index;
      @Override
      public boolean hasNext() {
         if(index < names.length){
            return true;
         }
         return false;
      }
      @Override
      public Object next() {
         if(this.hasNext()){
            return names[index++];
         }
         return null;
      }     
   }
}
```

<h3 id='18'>中介者模式--行为型模式</h3>
[原文链接][18]{:target="_blank"}

用来降低多个对象和类之间的通信复杂性。提供了一个中介类，该类通常处理不同类之间的通信，并支持松耦合，使代码易于维护。

```
//创建中介类
public class ChatRoom {
   public static void showMessage(User user, String message){
      System.out.println(new Date().toString() + " [" + user.getName() +"] : " + message);
   }
}
//创建 user 类
ublic class User {
   private String name;
   public void sendMessage(String message){
      ChatRoom.showMessage(this,message);
   }
}
```

<h3 id='19'>备忘录模式--行为型模式</h3>
[原文链接][19]{:target="_blank"}

保存一个对象的某个状态，以便在适当的时候恢复对象。

```
//Memento包含了要被恢复的对象的状态
public class Memento {
   private String state;
}
//发起者
public class Originator {
   private String state;
   public Memento saveStateToMemento(){
      return new Memento(state);
   }
}
//存储者
public class CareTaker {
   private List<Memento> mementoList = new ArrayList<Memento>();
 
   public void add(Memento state){
      mementoList.add(state);
   }
 
   public Memento get(int index){
      return mementoList.get(index);
   }
}
```

<h3 id='20'>观察者模式--行为型模式</h3>
[原文链接][20]{:target="_blank"}

当一个对象被修改时，则会自动通知它的依赖对象。对象间存在一对多关系。

```
//对象
public class Subject {
    private List<Observer> observers = new ArrayList<Observer>();
    private int state;
    //对象状态发生改变时要通知下观察者
    public void setState(int state) {
        this.state = state;
        notifyAllObservers();
    }
 
    public void attach(Observer observer){
        observers.add(observer);      
    }
    
    public void notifyAllObservers(){
        for (Observer observer : observers) {
            observer.update();
        }
    }  
}
//观察者的抽象类
public abstract class Observer {
    protected Subject subject;
    public abstract void update();
}
public class BinaryObserver extends Observer{
   public BinaryObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }
   @Override
   public void update() {
      System.out.println( "Binary String: " + Integer.toBinaryString( subject.getState() ) ); 
   }
}
public class OctalObserver extends Observer{
   public OctalObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }
   @Override
   public void update() {
     System.out.println( "Octal String: " + Integer.toOctalString( subject.getState() ) ); 
   }
}
public class HexaObserver extends Observer{
   public HexaObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }
   @Override
   public void update() {
      System.out.println( "Hex String: " + Integer.toHexString( subject.getState() ).toUpperCase() ); 
   }
}
```

<h3 id='21'>状态模式--行为型模式</h3>
[原文链接][21]{:target="_blank"}

类的行为是基于它的状态改变的

```
//状态对象
public interface State {
   public void doAction(Context context);
}
public class StartState implements State {
   public void doAction(Context context) {
      System.out.println("Player is in start state");
      context.setState(this); 
   }
   public String toString(){
      return "Start State";
   }
}
public class StopState implements State {
   public void doAction(Context context) {
      System.out.println("Player is in stop state");
      context.setState(this); 
   }
   public String toString(){
      return "Stop State";
   }
}
//一个行为随着状态对象改变而改变的 context 对象
public class Context {
   private State state;
}
```

<h3 id='22'>空对象模式</h3>
[原文链接][22]{:target="_blank"}

创建一个指定各种要执行的操作的抽象类和扩展该类的实体类，还创建一个未对该类做任何实现的空对象类，该空对象类将无缝地使用在需要检查空值的地方。

```
public abstract class AbstractCustomer{}
public class RealCustomer extends AbstractCustomer {}
public class NullCustomer extends AbstractCustomer{}
public class CustomerFactory {
   public static final String[] names = {"Rob", "Joe", "Julie"};
   public static AbstractCustomer getCustomer(String name){
      for (int i = 0; i < names.length; i++) {
         if (names[i].equalsIgnoreCase(name)){
            return new RealCustomer(name);
         }
      }
      return new NullCustomer();//如果没有相关的customer，那就返回一个默认的空对象customer
   }
}
```

<h3 id='23'>策略模式--行为型模式</h3>
[原文链接][23]{:target="_blank"}

一个类的行为或其算法可以在运行时更改。创建表示各种策略的对象和一个行为随着策略对象改变而改变的 context 对象。策略对象改变 context 对象的执行算法。

```
//策略接口
public interface Strategy {
   public int doOperation(int num1, int num2);
}
public class OperationAdd implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 + num2;
   }
}
public class OperationSubstract implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 - num2;
   }
}
public class OperationMultiply implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 * num2;
   }
}
public class Context {
   private Strategy strategy;
   public Context(Strategy strategy){
      this.strategy = strategy;
   }
   public int executeStrategy(int num1, int num2){
      return strategy.doOperation(num1, num2);
   }
}
```

<h3 id='24'>模板模式--行为型模式</h3>
[原文链接][24]{:target="_blank"}

一个抽象类公开定义了执行它的方法的方式/模板。它的子类可以按需要重写方法实现，但调用将以抽象类中定义的方式进行。

```
public abstract class Game {
   abstract void initialize();
   abstract void startPlay();
   abstract void endPlay();
   //模板
   public final void play(){
      //初始化游戏
      initialize();
      //开始游戏
      startPlay();
      //结束游戏
      endPlay();
   }
}
public class Football extends Game {
   @Override
   void endPlay() {
      System.out.println("Football Game Finished!");
   }
   @Override
   void initialize() {
      System.out.println("Football Game Initialized! Start playing.");
   }
   @Override
   void startPlay() {
      System.out.println("Football Game Started. Enjoy the game!");
   }
}
```

<h3 id='25'>访问者模式--行为型模式</h3>
[原文链接][25]{:target="_blank"}

1、一般被访问的东西所持有的、一般被访问的东西所持有的方法是固定的，就像账单只有收入和支出两个功能。而访问者是不固定的。

2、数据操作与数据结构相分离：频繁的更改数据，但不结构不变。比如：虽然每一天账单的数据都会变化（数据变化），但是只有两类数据，就是支出和收入（结构不变）。

```
//创建一个账单接口，有接收访问者的功能
public interface Bill {
    public void accept(AccountBookView viewer);
}
public class ConsumerBill implements Bill {
    private String item;
    private double amount;
    public void accept(AccountBookView viewer) {
        viewer.view(this);
    }
}
public class IncomeBill implements Bill {
    private String item;
    private double amount;
    public void accept(AccountBookView viewer) {
        viewer.view(this);
    }
}
//访问者接口
public interface AccountBookView {
    // 查看消费的单子
    void view(ConsumerBill consumerBill);
    // 查看收入单子
    void view(IncomeBill incomeBill);
}
// 老板类：访问者是老板，主要查看总支出和总收入
public class Boss implements AccountBookView {
    private double totalConsumer;
    private double totalIncome;
    // 查看消费的单子
    public void view(ConsumerBill consumerBill) {
        totalConsumer = totalConsumer + consumerBill.getAmount();
    }
    // 查看收入单子
    public void view(IncomeBill incomeBill) {
        totalIncome = totalIncome + incomeBill.getAmount();
    }
    public void getTotalConsumer() {
        System.out.println("老板一共消费了" + totalConsumer);
    }
    public void getTotalIncome() {
        System.out.println("老板一共收入了" + totalIncome);
    }
}
//会计类：访问者是会计，主要记录每笔单子
class CPA implements AccountBookView {
    int count = 0;
    // 查看消费的单子
    public void view(ConsumerBill consumerBill) {
        count++;
        if (consumerBill.getItem().equals("消费")) {
            System.out.println("第" + count + "个单子消费了：" + consumerBill.getAmount());
        }
    }
    // 查看收入单子
    public void view(IncomeBill incomeBill) {
        if (incomeBill.getItem().equals("收入")) {
            System.out.println("第" + count + "个单子收入了：" + incomeBill.getAmount());
        }
    }

}
//账单类：用于添加账单，和为每一个账单添加访问者
public class AccountBook {
    private List<Bill> listBill = new ArrayList<Bill>();
    // 添加单子
    public void add(Bill bill) {
        listBill.add(bill);
    }
    // 为每个账单添加访问者
    public void show(AccountBookView viewer) {
        for (Bill b : listBill) {
            b.accept(viewer);
        }
    }
}
public class Test {
    public static void main(String[] args) {
        // 创建消费和收入单子
        Bill consumerBill = new ConsumerBill("消费", 3000);
        Bill incomeBill = new IncomeBill("收入", 5000);
        Bill consumerBill2 = new ConsumerBill("消费", 4000);
        Bill incomeBill2 = new IncomeBill("收入", 8000);
        // 添加单子
        AccountBook accountBook = new AccountBook();
        accountBook.add(consumerBill);
        accountBook.add(incomeBill);
        accountBook.add(consumerBill2);
        accountBook.add(incomeBill2);
        // 创建访问者
        AccountBookView boss = new Boss();
        AccountBookView cpa = new CPA();
        // 接受访问者
        accountBook.show(boss);
        accountBook.show(cpa);
        // boss查看总收入和总消费
        ((Boss) boss).getTotalConsumer();
        ((Boss) boss).getTotalIncome();
    }
}
```

  [1]: http://www.runoob.com/design-pattern/factory-pattern.html
  [2]: http://www.runoob.com/design-pattern/abstract-factory-pattern.html
  [3]: http://www.runoob.com/design-pattern/singleton-pattern.html
  [4]: http://www.runoob.com/design-pattern/builder-pattern.html
  [5]: http://www.runoob.com/design-pattern/prototypetern.html
  [6]: http://www.runoob.com/design-pattern/adapter-pattern.html
  [7]: http://www.runoob.com/design-pattern/bridge-pattern.html
  [8]: http://www.runoob.com/design-pattern/filter-pattern.html
  [9]: http://www.runoob.com/design-pattern/composite-pattern.html
  [10]: http://www.runoob.com/design-pattern/decorator-pattern.html
  [11]: http://www.runoob.com/design-pattern/facade-pattern.html
  [12]: http://www.runoob.com/design-pattern/flyweight-pattern.html
  [13]: http://www.runoob.com/design-pattern/proxy-pattern.html
  [14]: http://www.runoob.com/design-pattern/chain-of-responsibility-pattern.html
  [15]: http://www.runoob.com/design-pattern/command-pattern.html
  [16]: http://www.runoob.com/design-pattern/interpreter-pattern.html
  [17]: http://www.runoob.com/design-pattern/iterator-pattern.html
  [18]: http://www.runoob.com/design-pattern/mediator-pattern.html
  [19]: http://www.runoob.com/design-pattern/memento-pattern.html
  [20]: http://www.runoob.com/design-pattern/observer-pattern.html
  [21]: http://www.runoob.com/design-pattern/state-pattern.html
  [22]: http://www.runoob.com/design-pattern/null-object-pattern.html
  [23]: http://www.runoob.com/design-pattern/strategy-pattern.html
  [24]: http://www.runoob.com/design-pattern/template-pattern.html
  [25]: https://www.jianshu.com/p/80b9cd7c0da5