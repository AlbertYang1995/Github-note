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



#### 数组

##### 声明数组

~~~java
int[] arr;
~~~

##### 创建数组，要指明数组的长度

~~~~java
int[] arr;  // 声明一个引用
arr = new int[5];  // 创建一个长度为5的整型数组，并且使用引用 a 指向该数组
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
int a [] = new int[]{101,102,103,104,105};      
int b[] = new int[3];  // 分配了长度是3的空间，但是没有赋值       
//通过数组赋值把，a数组的前3位赋值到b数组  
//方法一： for循环
for (int i = 0; i < b.length; i++) {
    b[i] = a[i];
}
        
//方法二: System.arraycopy(src, srcPos, dest, destPos, length)
//src: 源数组
//srcPos: 从源数组复制数据的起始位置
//dest: 目标数组
//destPos: 复制到目标数组的启始位置
//length: 复制的长度       
System.arraycopy(a, 0, b, 0, 3);

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
int[] a = new int[] { 18, 62, 68, 82, 65, 9 };
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



#### ArrayList

- 声明

  ~~~~java
  import java.util.ArrayList;
  ArrayList heros = new ArrayList();
  ~~~~

- 增加

  ~~~java
  import java.util.ArrayList;
  ArrayList heros = new ArrayList();
  heros.add(new Hero("盖伦"))；
  Hero specialHero = new Hero("special hero");
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

  















