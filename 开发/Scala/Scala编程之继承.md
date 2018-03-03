# Scala编程之继承


###extends


// Scala中，让子类继承父类，与Java一样，也是使用extends关键字// 继承就代表，子类可以从父类继承父类的field和method；然后子类可以在自己内部放入父类所没有，子类特有的field和method；使用继承可以有效复用代码// 子类可以覆盖父类的field和method；但是如果父类用final修饰，field和method用final修饰，则该类是无法被继承的，field和method是无法被覆盖的```
class Person {  private var name = "leo"  def getName = name}class Student extends Person {  private var score = "A"  def getScore = score}
```### override和super// Scala中，如果子类要覆盖一个父类中的非抽象方法，则必须使用override关键字// override关键字可以帮助我们尽早地发现代码里的错误，比如：override修饰的父类方法的方法名我们拼写错了；比如要覆盖的父类方法的参数我们写错了；等等// 此外，在子类覆盖父类方法之后，如果我们在子类中就是要调用父类的被覆盖的方法呢？那就可以使用super关键字，显式地指定要调用父类的方法```
class Person {  private var name = "leo"  def getName = name}class Student extends Person {  private var score = "A"  def getScore = score  override def getName = "Hi, I'm " + super.getName}
```### override field// Scala中，子类可以覆盖父类的val field，而且子类的val field还可以覆盖父类的val field的getter方法；只要在子类中使用override关键字即可```class Person {  val name: String = "Person"  def age: Int = 0}class Student extends Person {  override val name: String = "leo"  override val age: Int = 30}
```### isInstanceOf和asInstanceOf// 如果我们创建了子类的对象，但是又将其赋予了父类类型的变量。则在后续的程序中，我们又需要将父类类型的变量转换为子类类型的变量，应该如何做？// 首先，需要使用isInstanceOf判断对象是否是指定类的对象，如果是的话，则可以使用asInstanceOf将对象转换为指定类型// 注意，如果对象是null，则isInstanceOf一定返回false，asInstanceOf一定返回null// 注意，如果没有用isInstanceOf先判断对象是否为指定类的实例，就直接用asInstanceOf转换，则可能会抛出异常```

class Personclass Student extends Personval p: Person =  new Studentvar s: Student = nullif (p.isInstanceOf[Student]) s = p.asInstanceOf[Student]
```### getClass和classOf// isInstanceOf只能判断出对象是否是指定类以及其子类的对象，而不能精确判断出，对象就是指定类的对象// 如果要求精确地判断对象就是指定类的对象，那么就只能使用getClass和classOf了// 对象.getClass可以精确获取对象的类，classOf[类]可以精确获取类，然后使用==操作符即可判断```
class Personclass Student extends Personval p: Person = new Studentp.isInstanceOf[Person]p.getClass == classOf[Person]p.getClass == classOf[Student]
```### 使用模式匹配进行类型判断// 但是在实际开发中，比如spark的源码中，大量的地方都是使用了模式匹配的方式来进行类型的判断，这种方式更加地简洁明了，而且代码得可维护性和可扩展性也非常的高// 使用模式匹配，功能性上来说，与isInstanceOf一样，也是判断主要是该类以及该类的子类的对象即可，不是精准判断的```
class Personclass Student extends Personval p: Person = new Studentp match {  case per: Person => println("it's Person's object")  case _  => println("unknown type")}
```###protected

// 跟java一样，scala中同样可以使用protected关键字来修饰field和method，这样在子类中就不需要super关键字，直接就可以访问field和method// 还可以使用protected[this]，则只能在当前子类对象中访问父类的field和method，无法通过其他子类对象访问父类的field和methodclass Person {  protected var name: String = "leo"  protected[this] var hobby: String = "game"} class Student extends Person {  def sayHello = println("Hello, " + name)  def makeFriends(s: Student) {    println("my hobby is " + hobby + ", your hobby is " + s.hobby)  }}### 调用父类的constructor// Scala中，每个类可以有一个主constructor和任意多个辅助constructor，而每个辅助constructor的第一行都必须是调用其他辅助constructor或者是主constructor；因此子类的辅助constructor是一定不可能直接调用父类的constructor的// 只能在子类的主constructor中调用父类的constructor，以下这种语法，就是通过子类的主构造函数来调用父类的构造函数// 注意！如果是父类中接收的参数，比如name和age，子类中接收时，就不要用任何val或var来修饰了，否则会认为是子类要覆盖父类的field```
class Person(val name: String, val age: Int)class Student(name: String, age: Int, var score: Double) extends Person(name, age) {  def this(name: String) {    this(name, 0, 0)  }  def this(age: Int) {    this("leo", age, 0)  }}
```### 匿名内部类// 在Scala中，匿名子类是非常常见，而且非常强大的。Spark的源码中也大量使用了这种匿名子类。// 匿名子类，也就是说，可以定义一个类的没有名称的子类，并直接创建其对象，然后将对象的引用赋予一个变量。之后甚至可以将该匿名子类的对象传递给其他函数。```
class Person(protected val name: String) {  def sayHello = "Hello, I'm " + name}val p = new Person("leo") {  override def sayHello = "Hi, I'm " + name}def greeting(p: Person { def sayHello: String }) {  println(p.sayHello)}
```### 抽象类// 如果在父类中，有某些方法无法立即实现，而需要依赖不同的子来来覆盖，重写实现自己不同的方法实现。此时可以将父类中的这些方法不给出具体的实现，只有方法签名，这种方法就是抽象方法。// 而一个类中如果有一个抽象方法，那么类就必须用abstract来声明为抽象类，此时抽象类是不可以实例化的// 在子类中覆盖抽象类的抽象方法时，不需要使用override关键字abstract class Person(val name: String) {  def sayHello: Unit}class Student(name: String) extends Person(name) {  def sayHello: Unit = println("Hello, " + name)}### 抽象field// 如果在父类中，定义了field，但是没有给出初始值，则此field为抽象field// 抽象field意味着，scala会根据自己的规则，为var或val类型的field生成对应的getter和setter方法，但是父类中是没有该field的// 子类必须覆盖field，以定义自己的具体field，并且覆盖抽象field，不需要使用override关键字```abstract class Person {  val name: String}class Student extends Person {  val name: String = "leo"}```<!--
create time: 2018-02-27 10:24:58
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

