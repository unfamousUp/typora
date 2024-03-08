# Lambda表达式

## 一、什么是Lambda表达式

Lambda表达式，也可称为闭包。类似于JavaScript中的闭包，它是推动Java8发布的最重要的新特性。

> Lambda表达式中的【闭包】是指可以访问其周围范围内变量的`函数`或`代码块`。简而言之，闭包允许一个函数捕获和操作其外部范围内的变量，即使这些变量在函数被定义之后已经超出了其作用域。这使得Lambda表达式非常灵活，可以在其周围范围内使用外部变量。

> Lambda表达式和基于接口的匿名类都可以用于`创建没有名字的实现类对象`，并实现接口的抽象方法。它们有相似的功能，但在语法和代码清晰度上有一些区别。
>
> Lambda表达式和基于接口的匿名类之间的主要区别在于语法和代码简洁性：
>
> Lambda表达式：Lambda表达式的语法更为紧凑，通常更易读写。Lambda表达式通常比匿名类更简洁，因为它们不需要声明接口类型或方法名称，只需要提供参数和方法体。

### 1. Lambda表达式语法

- 可选类型声明：不需要声明参数类型，编译器可以统一识别参数值。
- 可选的参数圆括号：一个参数无需定义圆括号，但无参数或多个参数需要定义圆括号。
- 可选的大括号：如果主体包含了一个语句，就不需要使用大括号。
- 可选的返回关键字：如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定明表达式返回了一个数值。

> 语法格式一：无参，无返回值，Lambda体只需一条语句。如下：

```java
@Test
public void test01(){
    Runnable runnable=()-> System.out.println("Runnable 运行");
    runnable.run();//结果：Runnable 运行
}
```

> 语法格式二：Lambda需要一个参数，无返回值。

```java
@Test
public void test02(){
    Consumer<String> consumer=(x)-> System.out.println(x);
    consumer.accept("Hello Consumer");//结果：Hello Consumer
}
```

> 语法格式三：Lambda只需要一个参数时，参数的小括号可以省略

```java
public void test02(){
    Consumer<String> consumer=x-> System.out.println(x);
    consumer.accept("Hello Consumer");//结果：Hello Consumer
}
```

```java
@Test
public void test04(){
    // 当Lambda表达式只有一条表达式时，这个表达式的值将被隐式地作为Lambda函数的返回值。
    Predicate<Integer> isEven = n -> n % 2 == 0;
    boolean test = isEven.test(1);
    System.out.println(test);
}
```

> 语法格式四：Lambda需要两个参数，并且Lambda体中有多条语句。

```java
@Test
public void test04(){
    Comparator<Integer> com=(x, y)->{
      System.out.println("函数式接口");
      return Integer.compare(x,y);
    };
    System.out.println(com.compare(2,4));//结果：-1
}
```

> 语法格式五：有两个以上参数，有返回值，若Lambda体中只有一条语句，return和大括号都可以省略不写

```java
@Test
public void test05(){
    Comparator<Integer> com=(x, y)-> Integer.compare(x,y);
    System.out.println(com.compare(4,2));//结果：1
}
```

> Lambda表达式的参数列表的数据类型可以省略不写，因为JVM可以通过上下文推断出数据类型，即“类型推断”

```java
@Test
public void test06(){
    Comparator<Integer> com=(Integer x, Integer y)-> Integer.compare(x,y);
    System.out.println(com.compare(4,2));//结果：1
}
```

类型推断：在执行javac编译程序时，JVM根据程序的上下文推断出了参数的类型。Lambda表达式依赖于上下文环境。

语法背诵口诀：左右遇一括号省，左侧推断类型省，能省则省。

### 2. 函数式接口

>  public interface Consumer<T>

`Consumer<T>` 是 Java 中的一个函数式接口，它属于 Java 8 引入的 java.util.function 包。这个接口定义了一个单一的抽象方法 `void accept(T t)`，该方法接受一个参数 `t`，并对其执行某种操作，通常是消费或处理该参数。`Consumer<T>` 用于表示接受一个参数并且没有返回值的操作。它可以在很多情况下用于简化代码，特别是在集合操作和函数式编程中。

#### 4.1 什么是函数式接口

==只包含一个抽象方法的接口，就称为函数式接口。==我们可以通过Lambda表达式来创建该接口的实现对象。我们可以在任意函数式接口上使用`@FunctionalInterface`注解，这样做可以用于检测它是否是一个函数式接口，同时javadoc也会包含一条声明，说明这个接口是一个函数式接口。

#### 4.2 自定义函数式接口

按照函数式接口的定义，自定义一个函数式接口，如下：

```java
public interface MyFunctionInterface<T> {

    T getValue(String origin);

}
```

定义一个方法将函数式接口作为方法参数。

```java
public String toLowerString(MyFunctionInterface<String> mf, String origin){
    return mf.getValue(origin);
}
```

将Lambda表达式实现的接口作为参数传递。

```java
@Test
public void testFuncInter(){
    // 第一个参数是一个实现了抽象接口方法的lambda表达式，第二个参数是传递给lambda表达式的参数值
    String value = toLowerString((str) -> str.toLowerCase(), "ABC");
    System.out.println(value);
}
```

#### 4.3 Java内置函数式接口

![img](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202310231501391.png)

##### 4.3.1 Consumer

Consumer:消费型接口 `void accept(T t)`

```java
@Test
public void testMakeMoney(){
    makeMoney(money -> System.out.println("今天赚了"+money+"元"),100);
}

public void makeMoney(Consumer<Integer> consumer, Integer money){
    consumer.accept(money);
}
```

##### 4.3.2 Supplier

Supplier：供给型接口 `T get()`

