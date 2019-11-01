#### 类和对象

~~~java
Hero h = new Hero();
~~~

new Hero 代表创建了一个Hero对象

h这个变量是Hero类型，又叫做引用



可以同时有多个引用指向同一个对象

~~~java
//使用一个引用来指向这个对象
Hero h1 = new Hero();
Hero h2 = h1;  //h2指向h1所指向的对象
Hero h3 = h1;
Hero h4 = h1;
Hero h5 = h4;
//h1,h2,h3,h4,h5 五个引用，都指向同一个对象
~~~



#### 继承

~~~java
public class Item {
    String name;
    int price;
}
public class Weapon extends Item{
    int damage; //攻击力
     
    public static void main(String[] args) {
        Weapon infinityEdge = new Weapon();
        infinityEdge.damage = 65; //damage属性在类Weapon中新设计的
         
        infinityEdge.name = "无尽之刃";//name属性，是从Item中继承来的，就不需要重复设计了
        infinityEdge.price = 3600;  
    }
}
~~~



#### 构造方法

- 仅当类没有提供任何构造器的时候，系统才会提供一个默认的构造器

- 在方法重载中，返回类型并不是方法签名的一部分。也就是说，不能有两个名字相同、参数类型也相同，返回类型却不同的方法



#### 关键字this

- 表示隐式参数，代表这个类

  ~~~java
  //通过this访问属性
  public void setName(String name){
      //name代表的是参数name
      //this.name代表的是属性name
      this.name = name;
  }
  ~~~

- 调用该类下其他构造方法

  ~~~java
  //带一个参数的构造方法
  public Hero(String name){
  	System.out.println("一个参数的构造方法");
      this.name = name;
  }
        
  //带两个参数的构造方法
  public Hero(String name,float hp){
  	this(name);
      System.out.println("两个参数的构造方法");
      this.hp = hp;
  }
  ~~~



#### 方法参数

- 一个方法不能修改一个基本数据类型的参数

  ~~~java
  public static void tripleValue(double x) {
  	x = 3 * x;
  	// doesn't work
  }
  ~~~

- 一个方法可以改变一个对象参数的状态

  ~~~java
  public static void tripleValue(Employee x) {
  	x.raiseSalary(200);
  	// works
  }
  ~~~

- 一个方法不能让对象参数引用一个新的对象

  ~~~java
  public static void swap(Employee x, Employee y) {
  	Employee temp = x;
  	x = y;
  	y = temp;
  	// doesn't work
  }
  ~~~



#### 类属性

当一个属性被 **static** 修饰的时候，就叫做类属性，又叫做静态属性

当一个属性被声明称类属性，那么所有的对象，都共享一个值

~~~java
public class Hero {
    public String name; //实例属性，对象属性，非静态属性
    static String copyright;//类属性,静态属性   
}
~~~

访问类属性有两种方式：

- 对象.类属性
- 类.类属性（推荐，更符合语义） Hero.copyright



#### 类方法

类方法，又叫做静态方法

对象方法：又叫做实例方法，非静态方法

访问一个对象方法，必须建立在有一个对象的前提的基础上

访问类方法，不需要对象的存在，直接就访问

~~~java
public class Hero {
    public String name;
    protected float hp;
 
    //实例方法,对象方法，非静态方法
    //必须有对象才能够调用
    public void die(){
        hp = 0;
    }
     
    //类方法，静态方法
    //通过类就可以直接调用
    public static void battleWin(){
        System.out.println("battle win");
    }
     
    public static void main(String[] args) {
           Hero garen =  new Hero();
           garen.name = "盖伦";
           //必须有一个对象才能调用
           garen.die();
           //无需对象，直接通过类调用
           Hero.battleWin();
    }
}
~~~

调用类方法也有两种方式：

- 对象.类方法
- 类.类方法（推荐，更符合语义）

***warning***：不能在一个类方法中，直接调用一个对象方法，因为此时很可能没有任何具体对象



#### 属性初始化

##### 对象属性初始化

- 声明该属性的时候初始化

- 构造方法中初始化

- 初始化块

  ~~~java
  public class Hero {
      public String name = "some hero"; //声明该属性的时候初始化
      protected float hp;
      float maxHP;
       
      {
          maxHP = 200; //初始化块
      }  
       
      public Hero(){
          hp = 100; //构造方法中初始化 
      }
  }
  ~~~

##### 类属性初始化

- 声明该属性的时候初始化

- 静态初始化块

  ~~~java
  public class Hero {
      public String name;
      protected float hp;
      float maxHP;
       
      //物品栏的容量
      public static int itemCapacity=8; //声明的时候 初始化
       
      static{
          itemCapacity = 6;//静态初始化块 初始化
      }
       
      public Hero(){  // 调用构造方法的时候就会生成对象了，因此不能使用构造器初始化类属性
           
      }
  }
  ~~~

  ***注***：构造器初始化优先级大于初始化块

  
#### 关键字super

- 子类显式调用父类带参构造方法

  ```java
  public class Hero {
      public Hero(String name) {
          this.name = name;
      }
  }
  public class ADHero extends Hero {
      public class ADhero(String name) {
          super(name);
      }
  }
  ```

- 调用父类属性

  ~~~java
  public class Hero {
      int moveSpeed = 500;
  }
  public class ADHero extends Hero {
  	int moveSpeed = 600;
      public int getMoveSpeed() {
          return this.moveSpeed;
      }
      public int getMoveSpeed() {
          return super.moveSpeed;
      }
  }
  ~~~

- 调用父类方法

  ~~~~java
  public class Hero {
      public void useItem(Item i){
          System.out.println("hero use item");
      }
  }
  public class ADHero extends Hero {
      @Override
      public void useItem(Item i){
          System.out.println("ADHero use item");
          super.useItem(i);
      }
  }
  ~~~~

  

#### 关键字final

- 修饰类：类无法被继承

- 修饰方法：方法无法被重写

- 修饰基本类型变量：基本变量只有一次赋值机会

  ~~~java
  final int hp;
  hp = 5；
  hp = 6;  // 无法进行二次赋值
  ~~~

- 修饰引用：表示该引用只有一次指向对象的机会

  ~~~java
  final Hero h;
  h = new Hero();
  h = new ADHero();  // 无法指向该对象；
  ~~~

- 修饰常量：类似于const，常量无法改变



#### 抽象类

抽象类体现了数据抽象的思想，是实现多态的一种机制，它的作用就是在于继承。

使用抽象类时需要注意以下几点：

- 抽象类不能被实例化，实例化由它的子类来完成
- 抽象方法必须由子类来重写
- 只要包含一个抽象方法的类，必须定义为抽象类
- 抽象类中也可以包含具体方法，甚至可以不包含抽象方法
- 子类中的抽象方法不能和父类的抽象方法同名
- abstract 和 final 不能同时修饰一个类
- abstract 不能和 private, static, final, native 同时修饰一个方法



#### 接口

接口是抽象类的延伸，java 为了保证数据安全规定不能多重继承，也就是说一个子类只能继承一个抽象类，但是一个子类可以实现多个接口，所以接口弥补了抽象类不能多重继承的缺陷。

  

  

  

  

  

  

  

  

  

  

  