# 一、概述

## 1. 概念

- Extensible Markup Language 可扩展标记语言
  - 可扩展：标签都是自定义的。<user> <student>

## 2. 功能

- 存储数据
  1. 配置文件(.xml)
  2. 在网络中传输

## 3. xml和html区别

- 是亲兄弟，都是w3c（万维网联盟）旗下的
- xml标签都是自定义的，html是预定义的
- xml语法严格，html语法松散
- xml是存储数据，html是展示数据

# 二、语法

### 1. 基本语法

1. xml文档的后缀名 .xml
2. xml第一行必须定义为文档声明
3. xml文档中有且只有一个根标签
4. 属性值必须使用引号引起来
5. 标签必须正确关闭（开始和结束标签、自结束标签）
6. xml标签名称区分大小写

### 2. 快速入门

```xml
<?xml version='1.0'?>

<users>
	<user id='1'>
        <name>cjj</name>
        <age>18</age>
        <gender>male</gender>
    </user>    
    
    <user id='2'>
       <name>jcc</name>
       <age>18</age>
       <gender>male</gender>
    </user>    
</users>
```

### 3. 组成部分

1. 文档声明

   1. 格式：`<?xml 属性列表 ?>`
   2. 属性列表
      - version：版本号，必须的属性
      - encoding：编码方式，告知解析引擎当前文档使用的字符集，默认值：ISO-8859-1
      - standalone：是否独立
        - 取值
          - yes：不依赖其他文件
          - no：依赖其他文件

2. 指令（结合css）

   `<?xml-stylesheet type='text/css' href='a.css' ?>`

3. 标签
   - 规则
     - 名称不能包含字母、数字以及其他字符
     - 名称不能以数字或者标点符号开始
     - 名称不能以字母 xml或者XML与Xml开头
     - 名称不能包含空格
4. 属性
   - id属性值唯一
5. 文本
   - CDATA区：在该区域的数据会被原样展示
   - 格式：`<![CDATA[ 数据 ]]>`

# 三、xml约束

# 四、解析

- 操作xml文档
  1. 解析（读取）：将文档中数据读取到内存中
  2. 写入：将内存中的数据保存到xml文档中，持久化的存储
- 解析xml的方式
  1. DOM：将标记语言文档一次性加载进内存，在内存中形成一颗dom树
     - 优点：操作方便，可以对文档进行CRUD操作
     - 缺点：占内存
  2. SAX：逐行读取，读一行释放一行，基于事件驱动
     - 优点：不占内存
     - 缺点：不能增删改
- xml常见的解析器
  - JAXP：sun公司提供的解析器，支持dom和sax两种思想
  - DOM4J：一款非常优秀的解析器，基于DOM思想
  - Jsoup：Jsoup 是一款Java 的HTML解析器，可直接解析某个URL地址、HTML文本内容。它提供了一套非常省力的API，可通过DOM，CSS以及类似于jQuery的操作方法来取出和操作数据。
  - PULL：Android操作系统内置的解析器，sax思想

# 五、Jsoup解析器

## 1. 快速入门

- 步骤

  1. 导入jar包
  2. 获取Document对象（整个DOC文档的树形结构）
  3. 获取对应的标签Element对象
  4. 获取数据（文本内容）

  ```java
  package cn.cjj.xml.jsoup;
  
  import org.jsoup.Jsoup;
  import org.jsoup.nodes.Document;
  import org.jsoup.nodes.Element;
  import org.jsoup.select.Elements;
  
  import java.io.File;
  import java.io.IOException;
  
  // Jsoup快速入门
  public class JsoupDemo1 {
      public static void main(String[] args) throws IOException {
          // 1.导入jar包
          // 2.获取Document对象，根据xml文档来获取
          // 2.1获取student.xml的字符串表示形式的Path路径
          String Path = JsoupDemo1.class.getClassLoader().getResource("a.xml").getPath();
          // 2.2解析xml文档，加载文档进内存，获取dom树
          Document document = Jsoup.parse(new File(Path), "UTF-8");
          // 3.获取元素对象Element，是一个Elemnts数组
          Elements elements = document.getElementsByTag("name");//
          // 3.1获取第一个name的Element对象
          Element element = elements.get(0);
          // 3.2获取文本内容数据
          String name = element.text();
          System.out.println(name);
      }
  }
  ```

## 2. 对象的使用

1. Jsoup：工具类，可以解析html或xml对象，返回Document文档对象

   ```java
   static Document parse();方法
   // 1. 解析xml或html文件
   parse(File in, String charsetName);
   // 2. 解析xml或html字符串（源码）
   parse(String html);
   // 3. 通过网络路径URL来获取指定的html或xml文档对象
   parse(URL url, int timeoutMillis);
   ```

2. Document：文档对象，代表内存中的dom树

   - 获取Element对象

   ```java
   // 根据id属性值获取唯一的element对象
   getElementsById(String id)
   // 根据标签名
   getElementsByTag(String tagName)
   // 根据属性名
   getElementsByAttribute(String key)
   // 根据对应属性名和属性值
   getElementsByAttributeValue(String key, String value)
   ```

3. Elements：元素Element对象的集合，可以当做 ArrayList<Element>来使用

4. Element：元素对象

   1. 获取子元素对象

      ```java
      // 根据id属性值获取唯一的element对象
      getElementsById(String id)
      // 根据标签名
      getElementsByTag(String tagName)
      // 根据属性名
      getElementsByAttribute(String key)
      // 根据对应属性名和属性值
      getElementsByAttributeValue(String key, String value)
      ```

   2. 获取属性值

      ```java
      // 根据属性名称获取
      String attr(String key)
      ```

   3. 获取文本内容

      ```java
      // 获取文本内容
      String text()
      // 获取标签体的所有内容，包括子标签的字符串内容
      String html()
      ```

5. Node：节点对象
   - 是Document和Element的父类

## 3. 快速查询方式

1. selector：选择器
   - 使用方法：`Elements select(String cssQuery)`
   - 语法参考`Selector`类中定义的语法
2. XPath：
   - 