## 构造方法

构造方法的作用就是在 JVM 构造对象的时候，进行成员变量的初始化。  

其定义的原则是：  
- 方法名称与类名相同。

- 没有返回值类型声明。

- 构造方法可以进行重载。  

示例：
```java
class Student {
    // 成员变量
    String name;
    boolean isMale;
    int age;
    long id;

    // 构造方法
    public Student(String nameValue, boolean isMaleValue) {
        name = nameValue;
        isMale = isMaleValue;
    }
}
```

当我们没有在类中定义任何一个构造方法的时候，JVM 会自动添加一个默认的构造方法（无参构造方法）。一旦我们自己在类中定义了哪怕只有一个构造方法，JVM 就不会再自动添加无参构造方法。
> 这和 JVM 执行固定流程有关系：开辟对象的存储空间 -> 给对象成员变量赋予默认初值 -> 使用构造方法，初始化对象成员变量的值。

## this 关键字

成员变量的隐藏问题：当方法中定义了和类中同名的成员变量时，在方法体中，通过同名的变量名来访问变量值，只能访问到方法中的那个局部同名变量的值，而访问不到，同名成员变量的值。在方法体中，就好像同名成员变量，被同名局部变量给隐藏起来了。

this 关键字：代表对象自身的引用。  
- 构造方法中的 this 指代当前对象（构造方法执行时，JVM 正在创建的对象）。

- 对于普通成员方法而言，this 指代的对象就是被调用的对象。  

示例：
```java
class Student {
    // 成员变量
    String name;
    boolean isMale;
    int age;
    long id;

    // 构造方法
    public Student(String name, boolean isMale) {

        this.name = name;
        this.isMale = isMale;
    }

    public Student(String name, boolean isMale, int age) {
        
        // 访问对象的构造方法
        this(name, isMale); // 必须处在该构造方法体的第一条语句的位置。
        this.age = age;
    }
}
```

this 关键字的作用：
- 解决成员变量的隐藏的问题。

- 访问对象的成员（访问当前对象的成员变量值，访问当前对象的成员方法）。

- 访问对象的构造方法。
  - 只能在某个构造方法体中使用 this 调用构造方法。  
  - 必须处在该构造方法体的第一条语句的位置。

## static 关键字

static 关键字：可以修饰成员变量和成员方法。  

!> 按照严格意义上的面向对象思想，static 修饰的成员变量和成员方法都不能算做类中定义的成员，只是习惯上的称呼称 static 修饰的变量为静态成员变量，修饰的方法为静态成员方法。

示例：
```java
public class TestDemo {
    public static void main(String[] args) {
        HelloWorld.print();
    }
}

class HelloWorld {
    static void print() {
        System.out.println("HelloWorld");
    }
}
```

static 关键字的特点：  
- 被类的所有成员所共享（这点判定是否使用 static）。   
  - 当 static 修饰了成员变量，该成员变量的值就不存在其存储对象中，而是单独存储一份，被类的所有对象所共享。  

  - 当 static 修饰了成员方法，该方法被当前类的所有方法共享。

- 可以通过类名访问。  
  - 可以通过类名直接访问 static 成员变量的值。

  - 通过类名直接调用 static 成员方法。  

- 随着类的加载而加载。  
  - static 成员变量，随着类加载过程，其实就已经在方法区中，分配了内存。

  - static 成员方法，一旦类加载完毕，就可以直接访问，而不必创建对象，再用 `对象名.访问方法` 的方式。

- 优先于对象而存在。  
  - 不依赖于对象而存在。  
     - 成员变量的角度来理解：  
     static 修饰的成员变量的值不再存储在该类的每个对象中。  
     作为对比，没有被 static 修饰的成员变量都依赖于对象而存在，因为他们的值都存储在对象中。

     - 成员方法角度：  
     被 static 修饰的成员方法，在没有对象存在的情况下，也可以直接通过类名来调用方法。  
     作为对比，没有被 static 修饰的普通成员方法，它依赖于对象而存在。原因是普通成员方法中，可以访问普通成员变量的值，而普通对象的值又是依赖于对象而存在的。

  - 先出现在内存。  
    静态成员变量，一定先于没有被 static 修饰的普通成员变量出现在内存中。


static 关键字注意事项：  
- 非静态成员变量（非静态的成员方法）不能在静态上下文（静态方法的方法体）中被访问。  

- 静态方法的方法体中是没有 static 关键字的。

- 静态方法的使用场景：  
  - 静态方法和非静态方法除了访问方式不同，最大的区别就是静态方法可以访问到的数据集合（静态方法方法体中，不能直接访问非静态的成员变量）。所以通常静态方法所访问的数据要么是静态的成员变量，要么是方法的形式参数。  

  - **通常定义静态方法都是为了方便使用该方法的功能（工具方法），使其不用创建对象就可以使用。**


## 代码块

在 Java 中，使用 `{}` 括起来的代码被称为代码块，根据其位置和声明的不同，可以分为局部代码块、构造代码块、静态代码块和同步代码块（多线程相关）。  

局部代码块（开发中不会用）：方法体中用 `{}` 括起来的一段代码。  
- 执行时机：随着方法的执行而执行。  

- 优点：限定变量生命周期，及早释放，提高内存利用率。  
  这个优点理论上确实存在，但在现在 JVM 中的效果微乎其微，甚至可以忽略不计。

注：在嵌套的代码块中，不能定义同名变量。

构造代码块：类中方法外用 `{}` 包含的代码。  
- 执行特征：创建对象的时候执行。  
  注：构造方法和构造代码块，都是在创建对象的时候执行，但是构造代码块先执行，构造方法后执行。

- 构造代码块的使用场景：
  - 可以把多个构造方法中相同的代码提取放到构造代码块中。

  - 可以用构造代码块来初始化对象的成员变量的值。  
  注：成员变量初始化语句和构造代码块的执行顺序按顺序结构。  

静态代码块：类中方法外，被 static 修饰。  
- 执行特征：随着类加载而执行，所以静态代码块还有一个特征 —— 至多执行一次。


## package 关键字

在 Java 源程序文件的第一行使用 package 声明，可以使文件中定义的类成为指定包的成员。  

声明语法：
```java
package 包名;
```  

包名通常由多个字符串名构成，中间用 `.` 分隔。每个字符串名表示的包称为其前面的名字表示的包的子包。  

通常以组织机构的域名反转形式作为其所有包的通用前缀，比如：`com.somecompany.apps`

注：Java 中定义的任何一个类，都是在某一个包下的。如果代码没声明所属的包，此时类会在 Java 中的一个默认包中。

## import 关键字

在类名前面加上类所属的包名，中间用 `.` 分隔，称为类的完全限定名（Full Qualified Name），简称类的限定名。  

当在类体中使用了与当前类不同包的类名时，编译器编译时会因为无法找到该类的定义而报错。对此有两个解决办法：  
- 使用不同包类的完全限定名。

- 使用 import 声明，为编译器提供该类的定义信息。

声明语法：
```java
import <类的完全限定名>;
```

示例：
```java
import java.util.Scanner;
```

import 声明一般紧跟在 package 声明之后，且必须在类声明之前。

Java 语言核心包 java.lang 包中的类被隐式导入，可以直接使用。

import 声明提供了一种包的智能导入方式：`import <包名>.*;`。包中的类将根据需要导入，避免使用多条 import 声明。  

!> 智能导包不会递归导入子包中的类，即只能导入 `xx.*` 不能导入 `xx.*.*`。

## 访问权限修饰符

在 Java 语言中，一切事物（类所有成员）都具有或显示定义或隐式定义的访问权限，而这种语言层面的访问权限控制，是由访问权限修饰符实现。  

访问权限修饰符的访问控制分为 2 个层面：
- 修饰类中成员（field & method）。  
  控制类中的成员对其他类可见性（其他类是否可以直接使用到）。

- 修饰类。  
  通常用来限定类库中的类（自定义数据类型）对于外部使用者的可见性（是否能使用该类型）。

### 修饰类中成员

各权限修饰符对成员的可见性的影响：  

| | public | protected | default | private |  
| :- | :-: | :-: | :-: | :-: |  
| 同一类 | √ | √ | √ | √ |  
| 同包子类 / 其它类 | √ | √ | √ | |  
| 不同包子类 | √ | √ |  |  |  
| 不同包其它类 | √ |  |  |  |   


修饰类中成员的访问权限修饰符有 4 种：
- public：任意类均访问，实际就是没有限制访问权限。  
  - 类体中，可以直接访问。
  
  - 同包的其他类中，也可以访问。
  
  - 不同包的的类中，也可以访问。

- protected：同包中的其他类，和不同包的（可见）子类均可见。
  - 类体中，可以直接访问。  
  
  - 同包其他类中，可以访问。  
  
  - 在非同包的一部分类中，访问不到（有一部分子类中可以访问到：父类中被 protected）。

- default：默认权限，隐式定义。
  - 类体中，可以直接访问。
  
  - 同包其他类中，可以访问。
  
  - 不同包的类中，访问不了。

- private：仅对同类中的其他成员可见。  
  使用户不要触碰他们「不该」触碰的代码（private），类库设计者可以安全地修改实现细节。
  - 类体中，可以访问
  
  - 同包其他类中，无法访问
  
  - 非同包中，无法访问  

针对 private 成员变量，专门定义 public 的方法，让外部能够访问私有成员变量的值。
- Getter 方法让外部读取到私有成员变量值。

- Setter 方法让外部通过该方法，修改私有成员变量的值。

同时，为了清楚地表明 Getter 和 Setter 分别获取和改变的是哪个成员变量的值，命名规则为：
getXxx、setXxx。

示例：
```java
class Student {

    private String name;
    private int age;
    private boolean isMale;
    
    public Student(String name, int age, boolean isMale) {
        this.name = name;
        this.age = age;
        this.isMale = isMale;
    }
    
    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        if (name == null || name.equals("")) {
            return;
        }
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

提供 Getter 和 Setter 方法的好处：
-  可以满足某些场景下，需要访问私有成员变量值的需求。

-  通过定义 Getter 和 Setter 方法来实现对私有成员变量的读写分离。

-  一旦一个成员变量被 private 修饰，别人就无法直接通过 `对象名.成员变量` 访问。此时在 Setter 方法中，就可以通过代码控制别人的修改。

### 修饰类

能够修饰类的访问权限修饰符只有 2 种：
- public：对其他任意类可见。  
  任何其他地方都可以访问到这个类（同一个 module 下），比如同包中的其他类以及非同包的其他类。

- default：对同包中的其他类可以访问，非同包的类不可访问。