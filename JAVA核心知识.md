#### 类和对象

~~~java
Hero h = new Hero();
~~~

- new Hero 代表创建了一个Hero对象

  h这个变量是Hero类型，又叫做引用

- 可以同时有多个引用指向同一个对象

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

- 访问修饰符

|           | 自身 | 同包子类 | 不同包子类 | 同包类 | 其他类 |
| :-------: | :--: | :------: | :--------: | :----: | :----: |
|  private  | 访问 |          |            |        |        |
|  package  | 访问 |   继承   |            |  访问  |        |
| protected | 访问 |   继承   |    继承    |  访问  |        |
|  public   | 访问 |   继承   |    继承    |  访问  |  访问  |

- 什么情况该用什么修饰符呢：
  - 属性通常用private封装
  - 方法一般使用public方便调用
  - 会被子类继承的方法，通常使用protected
- 定义为`public`的`class`、`interface`可以被其他任何类访问
- 定义为`public`的`field`、`method`可以被其他类访问，前提是首先有访问该类的权限
- 定义为`private`的`field`、`method`无法被其他类访问，推荐把`private`方法放到`public`方法后面
- `protected`作用于继承关系。定义为`protected`的字段和方法可以被子类访问，以及子类的子类
- 一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类。如果有`public`类，文件名必须和`public`类的名字相同



#### 转型

所谓的转型，是指当**引用类型**和**对象类型**不一致的时候，才需要进行类型转换

- 向上转型（子类转父类）

~~~java
Person p = new Student();
~~~

因为`Student`继承自`Person`，因此，它拥有`Person`的全部功能。`Person`类型的变量，如果指向`Student`类型的实例，对它进行操作，是没有问题的！

- 向下转型（父类转子类）

~~~java
Hero h = new ADHero();
ADHero ad = new ADHero();
ad = (ADHero)h;
~~~

子类转父类需要强制转换，有时候行，有时候不行

**当父类引用的实际对象是子类时，强制转换可行**

- instanceof

~~~java
//判断引用h1指向的对象，是否是ADHero类型
System.out.println(h1 instanceof ADHero);
         
//判断引用h2指向的对象，是否是APHero类型
System.out.println(h2 instanceof APHero);
         
//判断引用h1指向的对象，是否是Hero的子类型
System.out.println(h1 instanceof Hero);
~~~



#### 构造方法

~~~java
class Person {
    public Person() {
        
    }
}
~~~

- 作用：在创建对象实例时就把内部字段全部初始化为合适的值

- 构造方法的名称就是类名。构造方法的参数没有限制，在方法内部，也可以编写任意语句。但是，和普通方法相比，构造方法没有返回值（也没有`void`），调用构造方法，必须用`new`操作符。

- 仅当类没有提供任何构造方法的时候，系统才会提供一个默认的构造方法

- 构造方法可以重载

- 在方法重载中，返回类型并不是方法签名的一部分。也就是说，不能有两个名字相同、参数类型也相同，返回类型却不同的方法，重载方法的返回值类型应该相同

- 初始化优先级：先对字段进行初始化，然后使用初始化块进行初始化，最后在构造方法中对字段进行初始化

  ~~~java
  class Person {
      private String name = "Unamed";
      private int age = 10;
  
      public Person(String name, int age) {
          this.name = name;
          this.age = age;
      }
  }
  ~~~
  
- 可以通过`this`在一个构造方法内部调用另一个构造方法



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



#### 可变参数

~~~java
class Group {
    private String[] names;
    public void setNames(String... names) {
        this.names = names;
    }
}
Group g = new Group();
g.setNames("Xiao Ming", "Xiao Hong", "Xiao Jun");
g.setNames("Xiao Ming");
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
      public static int itemCapacity = 8; //声明的时候 初始化
       
      static{
          itemCapacity = 6;//静态初始化块 初始化
      }
       
      public Hero(){  // 调用构造方法的时候就会生成对象了，因此不能使用构造器初始化类属性
           
      }
  }
  ~~~

  ***注***：构造器初始化优先级大于初始化块大于属性声明




#### 多态

多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。

类的多态的条件：

- 父类（接口）引用指向子类对象
- 调用的方法有重写

~~~java
public class Item {
    String name;
    int price;

    public void effect() {
        System.out.println("物品使用后，可以有效果 ");
    }
     
    public static void main(String[] args) {
        Item i1= new LifePotion();
        Item i2 = new MagicPotion();
        i1.effect();
        i2.effect();
    }
}
 
public class LifePotion extends Item {
    @override
    public void effect(){
        System.out.println("血瓶使用后，可以回血");
    }
}
 
public class MagicPotion extends Item{
 	@override
    public void effect(){
        System.out.println("蓝瓶使用后，可以回魔法");
    }
}
~~~

**TIPS**：虽然引用类型都为`Item`，但是对象类型分别为`Item`的子类`LifePotion`和`MagicPotion`

​			调用`effect`函数时，分别调用各子类的重写函数



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

  - 如果父类没有默认的构造方法，子类就必须显式调用`super()`并给出参数以便让编译器定位到父类的一个合适的构造方法

  ~~~java
  public class Main {
      public static void main(String[] args) {
          Student s = new Student("Xiao Ming", 12, 89);
      }
  }
  
  class Person {
      protected String name;
      protected int age;
  
      public Person(String name, int age) {
          this.name = name;
          this.age = age;
      }
  }
  
  class Student extends Person {
      protected int score;
  
      public Student(String name, int age, int score) {
          super(name, age); // 调用父类的构造方法Person(String, int)
          this.score = score;
      }
  }
  ~~~

  - 这里还顺带引出了另一个问题：即子类**不会继承**任何父类的构造方法。子类默认的构造方法是编译器自动生成的，不是继承的

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

- 使用抽象类时需要注意以下几点：
  - 抽象类不能被实例化，实例化由它的子类来完成
  - 抽象方法必须由子类来重写
  - 只要包含一个抽象方法的类，必须定义为抽象类
  - 抽象类中也可以包含具体方法，甚至可以不包含抽象方法
  - 子类中的抽象方法不能和父类的抽象方法同名
  - abstract 和 final 不能同时修饰一个类
  - abstract 不能和 private, static, final, native 同时修饰一个方法
- 面向抽象编程：引用高层类型，避免引用实际子类型的方式
  - 上层代码只定义规范
  - 不需要子类就可以实现业务逻辑（正常编译）
  - 具体的业务逻辑由不同的子类实现，调用者并不关心



#### 接口

- 接口是抽象类的延伸，java 为了保证数据安全规定不能多重继承，也就是说一个子类只能继承一个抽象类，但是一个子类可以实现多个接口，所以接口弥补了抽象类不能多重继承的缺陷。
- 接口定义的所有方法默认都是`public abstract`的，所以这两个修饰符不需要写出来（写不写效果都一样）。
- 一个类可以同时`implement`多个`interface`，接口中不能有实例字段，可以定义`default`方法

~~~java
class Student implements Person, Hello { // 实现了两个interface
    ...
}
~~~

- 一个`interface`可以继承自另一个`interface`，相当于拓展了接口的方法

~~~java
interface Hello {
    void hello();
}

interface Person extends Hello {
    void run();
    String getName();
}
~~~

- 实现类可以不必覆写`default`方法。`default`方法的目的是，当我们需要给接口新增一个方法时，会涉及到修改全部子类。如果新增的是`default`方法，那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。
- `default`方法和抽象类的普通方法是有所不同的。因为`interface`没有字段，`default`方法无法访问字段，而抽象类的普通方法可以访问实例字段。
- 接口中可以定义静态字段，并且`interface`中的字段必须是`public static final`类型



#### 数组

##### 声明数组

~~~java
int[] arr;
~~~

##### 创建数组，要指明数组的长度

~~~~java
int[] arr;  // 声明一个引用
arr = new int[5];  // 创建一个长度为5的整型数组，并且使用引用 arr 指向该数组
int[] arr = new int[5];  // 声明的同时，指向一个数组
~~~~

##### 数组长度

~~~java
int[] arr = new int[5];
System.out.println(arr.length);  // 打印数组长度
~~~

##### 赋值

~~~java
// 方法一
int[] arr1 = new int[]{101,102,103,104,105};
// 方法二
int[] arr2 = {101,102,103,104,105};
// 错误方法
int[] arr3 = new int[5]{101,102,103,104,105}  // 声明了数组长度则不能直接赋值
~~~

##### 遍历数组

~~~java
// 传统遍历
int[] arr = {101,102,103,104,105};
for(int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
// for each 方法 (只能够读取值，不能修改数组中的值)
int arr = {101,102,103,104,105};
for(int i : arr) {
    System.out.println(i);
}
~~~

##### 复制数组

~~~java
int[] a = new int[]{101,102,103,104,105};      
int[] b = new int[3];  // 分配了长度是3的空间，但是没有赋值       
// 通过数组赋值把，a数组的前3位赋值到b数组  
// 方法一： for循环
for (int i = 0; i < b.length; i++) {
    b[i] = a[i];
}
        
// 方法二: System.arraycopy(src, srcPos, dest, destPos, length)
// src: 源数组
// srcPos: 从源数组复制数据的起始位置
// dest: 目标数组
// destPos: 复制到目标数组的启始位置
// length: 复制的长度       
System.arraycopy(a, 0, b, 0, 3);

// 方法三
import java.util.Arrays;
int[] a = new int[]{101,102,103,104,105};
int[] b = Arrays.copyOfRange(a, 0, 3); 
// copyOfRange(int[] original, int from, int to)
// 第一个参数表示源数组
// 第二个参数表示开始位置(取得到)
// 第三个参数表示结束位置(取不到)
~~~

##### 转换为字符串

~~~java
import java.util.Arrays;
int[] a = new int[]{ 18, 62, 68, 82, 65, 9 };
String content = Arrays.toString(a);
System.out.println(content);
~~~

##### 排序

~~~java
import java.util.Arrays;
int[] a = new int[]{101,102,103,104,105};
Arrays.sort(a);
~~~

##### 判断两个数组是否相同

~~~java
import java.util.Arrays;
int[] a = {101,102,103,104,105};
int[] b = {101,102,103,104,105};
System.out.println(Arrays.equals(a, b));
~~~

##### 填充

~~~java
import java.util.Arrays;
int[] a = new int[10];
Arrays.fill(a, 5);
~~~

##### 二维数组

~~~java
int[][] a = new int[2][3]; //有两个一维数组，每个一维数组的长度是3
a[1][2] = 5;  //可以直接访问一维数组，因为已经分配了空间
          
//只分配了二维数组
int[][] b = new int[2][]; //有两个一维数组，每个一维数组的长度暂未分配
b[0]  =new int[3]; //必须事先分配长度，才可以访问
b[0][2] = 5;
        
//指定内容的同时，分配空间
int[][] c = new int[][]{
    {1,2,4},
    {4,5},
    {6,7,8,9}
};
// 遍历二维数组
for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[0].length; j++) {
        System.out.println(arr[i][j]);
    }
}
~~~



#### 集合

![TIM图片20200422132444.png](http://ww1.sinaimg.cn/large/bc48dba3gy1ge2h8u9eknj20in0940uh.jpg)

##### ArrayList

- 声明

  ~~~~java
  import java.util.ArrayList;
  ArrayList heros = new ArrayList();
  ~~~~

- 增加

  ~~~java
  import java.util.ArrayList;
  ArrayList heros = new ArrayList();
  // 添加在末尾
  heros.add(new Hero("盖伦"))；
  Hero specialHero = new Hero("special hero");
  // 添加在指定位置
  heros.add(3, specialHero);
  ~~~

- 判断一个对象是否存在容器中

  ~~~java
  System.out.println(heros.contains(specialHero));
  ~~~

- 获取指定位置的对象

  ~~~java
  System.out.println(heros.get(5));
  ~~~

- 获取对象所处位置

  ~~~java
  System.out.println(heros.indexOf(specialHero));
  ~~~

- 删除对象

  ~~~java
  heros.remove(2);
  heros.remove(specialHero);
  ~~~

- 替换对象

  ~~~java
  heros.set(5, new Hero("提莫"));
  ~~~

- 获取ArrayList大小

  ~~~java
  System.out.println(heros.size());
  ~~~

- ArrayList转换为数组

  ~~~java
  ArrayList heros = new ArrayList();
  Hero[] hs = (Hero[])heros.toArray(new Hero[]{});
  ~~~

- 把另一个容器所有对象加进来

  ~~~java
  ArrayList heros = new ArrayList();
  ArrayList anotherHeros = new ArrayList();
  heros.addAll(anotherHeros);
  ~~~

- 清空ArrayList

  ~~~java
  heros.clear();
  ~~~

- 实现了接口List

  ~~~java
  import java.util.ArrayList;
  import java.util.List;
  // ArrayList实现了接口List
  // 接口引用指向子类对象(多态)
  List heros = new ArrayList();
  heros.add(new Hero("盖伦"));
  ~~~

- 在ArrayList上使用泛型

  ~~~java
  // 引入泛型Generic
  // 声明容器的时候，就指定了这种容器，只能放Hero或者Hero的子类，放其他的就会出错
  List<Hero> genericheros = new ArrayList<Hero>();
  genericheros.add(new Hero("盖伦"));
  ~~~

- 遍历ArrayList

  ~~~java
  // 第一种 使用for循环
  for (int i = 0; i < heros.size(); i++) {
      Hero h = heros.get(i);
      System.out.println(h);
  }
  
  // 第二种 使用迭代器
  Iterator<Hero> it = heros.iterator();
  // 迭代器的while写法
  // 从最开始的位置判断"下一个"位置是否有数据
  // 如果有就通过next取出来，并且把指针向下移动
  // 直到"下一个"位置没有数据
  while(it.hasNext()){
      Hero h = it.next();
      System.out.println(h);
  }
  // 迭代器的for写法
  for (Iterator<Hero> iterator = heros.iterator(); iterator.hasNext();) {
      Hero hero = (Hero) iterator.next();
      System.out.println(hero);
  }
  
  // 第三种 使用增强型for循环
  for (Hero h : heros) {
      System.out.println(h);
  }
  ~~~

##### LinkedList

- 双向链表

  ~~~java
  import java.util.LinkedList;
  // LinkedList 是一个双向链表结构的 list
  LinkedList<Hero> ll = new LinkedList<Hero>();
  // 可以很方便的在头部和尾部插入数据
  ll.addFirst(new Hero("盖伦"));
  ll.addLast(new Hero("提莫"));
  // 查看最前面和最后面的数据
  ll.getFirst();
  ll.getLast();
  // 取出最前面和最后面的数据
  ll.removeFirst();
  ll.removeLast();
  // 替换目标处元素
  ll.set(5, new Hero("提莫"))；
  ~~~

- 队列

  ~~~java
  import java.util.LinkedList;
  import java.util.Queue;
  Queue<Hero> q = new LinkedList<Hero>();
  // 加在队列的最后面
  q.offer(new Hero("盖伦"));
  // 取出第一个元素
  Hero h = q.poll();
  // 查看第一个元素
  Hero h = q.peek();
  ~~~

- 栈

  ~~~java
  import java.util.LinkedList;
  LinkedList<Hero> heros = new LinkedList<Hero>();
  Hero h = new Hero("盖伦")；
  // 压栈
  heros.addLast(h);
  // 弹栈
  heros.removeLast(h);
  // 查看栈顶元素
  heros.getLast(h);
  ~~~

##### HashMap

~~~java
import java.util.HashMap;
HashMap<String, String> dictionary = new HashMap<String, String>();
dictionary.put("adc", "jinx");

// key是唯一的，值可以不唯一
HashMap<String, Hero> heroMap = new HashMap<>();
Hero gareen = new Hero("gareen");
heroMap.put("hero1", gareen);
heroMap.put("hero2", gareen);

// 获取Map长度
dictionary.length
// 询问是否包含某一Key
dictionary.containsKey()
// 取得某一Key对应的键值
dictionary.get()
// 删除某一key对应键值
dictionary.remove()
~~~

##### HashSet

~~~java
import java.util.HashSet;
HashSet<Integer> numbers = new HashSet<String>();
numbers.add(4);  // set 中没有顺序
// 两种遍历方式
// 使用迭代器iterator
for (Iterator<Integer> iterator = numbers.iterator(); iterator.hasNext();) {
    Integer i = (Integer) iterator.next();
}
// 使用增强型for循环
for (Integer i : numbers) {
    System.out.println(i);
}
~~~



#### Java DataBase Connectivity

- 打开MySQL服务器命令

  mysql -u root -p

- 使用Java程序访问数据库时，Java代码并不是直接通过TCP连接去访问数据库，而是通过JDBC接口来访问，而JDBC接口则通过JDBC驱动来实现真正对数据库的访问

![图片1.png](http://ww1.sinaimg.cn/large/bc48dba3gy1gd0qbepjj6j207g085q2u.jpg)

- 例如，我们在Java代码中如果要访问**MySQL**，那么必须编写代码操作**JDBC**接口，**JDBC**接口是**Java**标准库自带的，所以可以直接编译。而具体的**JDBC**驱动是由数据库厂商提供的，因此，访问某个具体的数据库，我们只需引入该厂商提供的**JDBC**驱动，就可以通过**JDBC**接口来访问。这样保证了**Java**程序编写的是一套数据库访问代码 ，却可以访问不同的数据库，因为他们都提供了标准的**JDBC**驱动

![图片2.png](http://ww1.sinaimg.cn/large/bc48dba3gy1gd0qbq58bej207u08d3yf.jpg)

- 从代码来看，Java标准库自带的JDBC接口其实就是定义了一组接口，而某个具体的JDBC驱动其实就是实现了这些接口的类

![图片3.png](http://ww1.sinaimg.cn/large/bc48dba3gy1gd0qbu26i2j20at06mjra.jpg)

- 实际上，一个MySQL的JDBC的驱动就是一个jar包，它本身也是纯Java编写的。我们自己编写的代码只需要引用Java标准库提供的java.sql包下面的相关接口，由此再间接地通过MySQL驱动的jar包通过网络访问MySQL服务器，所有复杂的网络通讯都被封装到JDBC驱动中，因此，Java程序本身**只需要引入一个MySQL驱动的jar包**就可以正常访问MySQL服务器。

- 使用JDBC的好处是：
  - 各数据库厂商使用相同的接口，Java代码不需要针对不同数据库分别开发
  - Java程序编译器仅依赖java.sql包，不依赖具体数据库的jar包
  - 可随时替换底层数据库，访问数据库的Java代码基本不变

##### JDBC连接

Connection代表一个JDBC连接，它相当于Java程序到数据库的连接（通常是TCP连接）

打开一个Connection时，需要准备URL、用户名以及口令才能成功连接到数据库

- URL是由数据库厂商定义的格式

  ~~~java
  jdbc:mysql://<hostname>:<port>/<db>?key1=value1&key2=value2
  ~~~

- 数据库连接示例

  ~~~java
  // JDBC连接的URL, 不同数据库有不同的格式:
  String JDBC_URL = "jdbc:mysql://localhost:3306/test";
  String JDBC_USER = "root";
  String JDBC_PASSWORD = "password";
  // 获取连接:
  Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD);
  // TODO: 访问数据库...
  // 关闭连接:
  conn.close();
  ~~~

##### JDBC查询

- 通过<code>Connection</code>提供的<code>createStatement()</code>方法创建一个<code>Statement</code>对象，用于执行一个查询；
- 执行<code>Statement</code>对象提供的<code>executeQuery("SELECT * FROM students")</code>并传入SQL语句，执行查询并获得返回的结果集，使用<code>ResultSet</code>来引用这个结果集
- 反复调用<code>ResultSet</code>的<code>next()</code>方法并读取每一行结果

~~~java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD) {
    try (Statement stmt = conn.createStatement()) {
        try (ResultSet rs = stmt.executeQuery("SELECT id, grade, name, gender FROM students WHERE gender=\'M\'")) {
            while (rs.next()) {
                long id = rs.getLong(1); // 注意：索引从1开始
                long grade = rs.getLong(2);
                String name = rs.getString(3);
                String gender = rs.getString(4);
            }
        }
    }
}
~~~

TIPS：

- <code>Statement</code>和<code>ResultSet</code>都是需要关闭的资源，因此嵌套使用<code>try (resource)</code>确保及时关闭；
- <code>rs.next()</code>用于判断是否有下一行记录，如果有，将自动把当前行移动到下一行（一开始获得<code>ResultSet</code>时当前行不是第一行）
- <code>ResultSet</code>获取列时，索引从**1**开始而不是**0**
- 必须根据<code>SELECT</code>的列的对应位置来调用<code>getLong(1)</code>，<code>getString(2)</code>这些方法，否则对应位置的数据类型不对，将报错

- 使用<code>PreparedStatement</code>避免SQL注入问题

~~~java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD) {
    try (PreparedStatement ps = conn.prepareStatement("SELECT id, grade, name, gender FROM students WHERE gender=? AND grade=?")) {
        ps.setObject(1, "M"); // 注意：索引从1开始
        ps.setObject(2, 3);
        try (ResultSet rs = ps.executeQuery()) {
            while (rs.next()) {
                long id = rs.getLong("id");
                long grade = rs.getLong("grade");
                String name = rs.getString("name");
                String gender = rs.getString("gender");
            }
        }
    }
}
~~~

TIPS：

- 必须调用<code>setObject()</code>设置每个占位符<code>?</code>的值
- 从<code>ResultSet</code>读取列时，使用<code>String</code>类型的列名比索引要易读，不易出错

##### JDBC更新

增删改查 CRUD

Create Retrieve Update Delete

- 插入

~~~java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD) {
    try (PreparedStatement ps = conn.prepareStatement(
            "INSERT INTO students (id, grade, name, gender) VALUES (?,?,?,?)")) {
        ps.setObject(1, 999); // 注意：索引从1开始
        ps.setObject(2, 1); // grade
        ps.setObject(3, "Bob"); // name
        ps.setObject(4, "M"); // gender
        int n = ps.executeUpdate(); // 表示插入记录的数目，always = 1
    }
}
~~~

- 更新

~~~java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD) {
    try (PreparedStatement ps = conn.prepareStatement("UPDATE students SET name=? WHERE id=?")) {
        ps.setObject(1, "Bob"); // 注意：索引从1开始
        ps.setObject(2, 999);
        int n = ps.executeUpdate(); // 返回更新的行数(0或者正数)
    }
}
~~~

- 删除

~~~java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD) {
    try (PreparedStatement ps = conn.prepareStatement("DELETE FROM students WHERE id=?")) {
        ps.setObject(1, 999); // 注意：索引从1开始
        int n = ps.executeUpdate(); // 删除的行数
    }
}
~~~

<code>INSERT</code>、<code>UPDATE</code>、<code>DELETE</code>都视为更新操作

更新操作使用<code>PrepareStatement</code>的<code>executeUpdate()</code>，返回受影响的行数

##### JDBC事务

~~~java
Connection conn = openConnection();
try {
    // 关闭自动提交:
    conn.setAutoCommit(false);
    // 执行多条SQL语句:
    insert(); update(); delete();
    // 提交事务:
    conn.commit();
} catch (SQLException e) {
    // 回滚事务:
    conn.rollback();
} finally {
    conn.setAutoCommit(true);
    conn.close();
}
~~~

- 默认情况下，我们获取到<code>Connection</code>连接之后，总是处于“自动提交”模式

##### JDBC Batch

同一个SQL但参数不同的若干次操作合并为一次**Batch**执行

~~~java
try (PreparedStatement ps = conn.prepareStatement("INSERT INTO students (name, gender, grade, score) VALUES (?, ?, ?, ?)")) {
    // 对同一个PreparedStatement反复设置参数并调用addBatch():
    for (String name : names) {
        ps.setString(1, name);
        ps.setBoolean(2, gender);
        ps.setInt(3, grade);
        ps.setInt(4, score);
        ps.addBatch(); // 添加到batch
    }
    // 执行batch:
    int[] ns = ps.executeBatch();
    for (int n : ns) {
        System.out.println(n + " inserted."); // batch中每个SQL执行的结果数量
    }
}
~~~

##### JDBC连接池

如果每一次操作都来一次打开连接，操作，关闭连接，那么创建和销毁JDBC连接的开销就太大了。为了避免频繁地创建和销毁JDBC连接，我们可以通过连接池（Connection Pool）复用已经创建好的连接。

~~~java
try (Connection conn = ds.getConnection()) { // 在此获取连接
    ...
} // 在此“关闭”连接
~~~



















