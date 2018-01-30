 

# 和飞信平台移动端编码规范

（Android）

 

| **文件编号：和飞信平台移动端编码规范****(Android)** |                 |                  |
| ---------------------------------- | --------------- | ---------------- |
| **密级：秘密******                      | **版本：****V1.0** | **修改状态：审核中****** |
| **编制：******                        | **审核：**** **    | **批准：**** **     |

 

和飞信

2017年4月

 

**修订历史记录**

| **序号****** | **日期****** | **描述****** | **版本****** | **作者****** | **审核者****** |
| ---------- | ---------- | ---------- | ---------- | ---------- | ----------- |
| 1          | 2017-04-19 | 文档初建       | 1.0        | 王晓红        |             |
|            |            |            |            |            |             |
|            |            |            |            |            |             |
|            |            |            |            |            |             |

本文档中所包含的信息属于机密信息，如无新媒传信的书面许可，任何人都无权复制或利用。

[TOC]



# 1        引言 

## 1.1    编写目的 

* 定义和飞信Android平台开发规范
* 约束与指导各项目开发人员按照该规范进行编码。

## 1.2 文档对象 

和飞信的相关负责人、项目经理、开发工程师。

## 1.3 术语约定 

暂无

## 1.4 参考资料 

* 本文档参考了[Sun MicroSystems 公司的编码规范①，结合本公司情况，做出了相应调整。
* [《CodeConventions for the JavaTM Programming Language》](http://java.sun.com/docs/codeconv/html/CodeConvTOC.doc.html) Sun Microsystems,Inc.

## 1.5 介绍 

* 为什么要有编码规范

编码规范对编程人员相当重要，有如下原因：

1)  一个软件的生命周期中，80％的时间用于维护

2)  维护工作不太可能始终由一个人来完成

3)  编码规范可以改善代码的可读性，便于自己和他人理解

4)  编码规范可以降低代码出错的可能性

基于以上原因，为了给自己和同事带来便利，为了提高代码质量，编程人员在编码中必须要遵守编码规范。

* 规范组织结构

因为Android是基于Java语言开发，所以前半部分是对Java语言的规范，后半部分是对Android开发的规范。

# 2  Java 开发规范

## 2.1 文件组织 

一个有效的文件，是由空行加各个可选内容段构成。应尽量避免超过1500行的代码，因为在这个长度以上，代码通常难以阅读，此时尽可能按照功能分解成多个文件。

### 2.1.1  Java 源文件

每一个Java源文件都包含一个单一的公共类或接口。[若私有类和接口与一个公共类相关联，可以将它们和公共类放入同一个源文件中。公共类或接口必须 放在前面。

#### 2.1.1.1  开头注释

所有的Java源文件在开头有一个注释，包括类名、版本信息、日期、版权信息和修改记录。![versioninfo.png](http://git.feinno.com/pub/guidelines/uploads/56d50c4d1dd609cc0a912e8e28e9d926/versionInfo.png)

#### 2.1.1.2  package和import语句

第一行非注释语句必须是package语句。要求所有的java类必须要有package语句。例：package cn.com.fetion;

package语句后是import语句，建议首先引入Java的基本API类，然后再引入第三方API类，最后是本工程类。例：import java.util.Hashtable;

#### 2.1.1.3  类和接口申明

下表按照在源码中出现的顺序，列出类或接口的各个组成部分。

|      | 内容     | 备注                                       |
| ---- | ------ | ---------------------------------------- |
| 1    | 类文档注释  |                                          |
| 2    | 类或接口语句 |                                          |
| 3    | 类变量    | 即类实例共享变量（static变量）。按照public、protected、package  level、private的顺序排列。 |
| 4    | 实例变量   | 按照public、protected、package level、private的顺序排列。 |
| 5    | 构造方法   |                                          |
| 6    | 方法     | 只要读起来方便，可以按照功能或其它方式来组织各方法的位置。如公共方法在前，为公共方法提供子功能的私有方法在后等等。 |

## 2.2 注释 

Java中有两种注释，一种是实现注释（/*…*/， //…），一种是文档注释（/**…*/），这两种注释的区别在于，实现注释仅仅是作为注释而存在，文档注释可以使用JavaDoc直接生成文档。

实现注释的目的在于说明程序中没有表达的信息，帮助程序员阅读代码，必须要有，但是不易过多（30%为宜）。代码中涉及到算法的部分，尽量给出详尽的实现注释。

实现注释的格式:

*   类中有文件名说明和功能描述

*   提交修改时要写注释

*   方法注释：写明方法功能、参数、返回值、异常描述

*   条件分支需要有注释

*   关键变量要写注释

*   去掉无故的TODO，没做完的代码需要加上TODO写清楚意思

### 2.2.1   实现注释的格式  

程序可以有四中注释的风格：块、单行、尾端、行末。

#### 2.2.1.1  块注释

```
/*
   * 这是一个块注释的例子。
   */
```

块注释通常用于提供对文件、方法、数据结构和算法的描述。块注释被置于每个文件的开始处以及方法之前。它们也可以被用于其他地方，比如方法内部。在功能和方法内部的块注释应该和它们所描述的代码具有一样的缩进格式。 

对private方法应该采取块注释。

块注释之前应该有一个空行，用于把块注释和代码分割开来。

#### 2.2.1.2  单行注释

```
if (condition) {
    /* Handle the condition. */
    ...
  }
```

短注释可以显示在一行内，并与其后的代码具有一样的缩进层级。如果一个注释不能在一行内写完，就该采用块注释 。单行注释之前应该有一个空行。

#### 2.2.1.3  尾端注释

```
if (a == 2) {
    return true; /* special case */
  } else {
    return isPrime(a); /* works only for odd a */
  }
```

[极短的注释可以与它们所要描述的代码位于同一行，但是应该有足够的空白来分开代码和注释。若有多个短注释出现于大段代码中，它们应该具有相同的缩进 。

#### 2.2.1.4  行末注释

```
if (foo > 1) {
      // Do a double-flip.
      ...
    } else {
      return false; // Explain why here.
    }

//    if (bar > 1) {
//      Do a triple - flip.
//        ...
//    } else {
//      return false;
//    }
```

[注释界定符"//"，可以注释掉整行或者一行中的一部分。它一般不用于连续多行的注释文本；然而，它可以用来注释掉连续多行的代码段 。如上面的例子所示，如果要注释掉连续多行的代码段，注释符顶行开始。

### 2.2.2   文档注释 

有关文档注释的部分，可以参考以下文档来了解更多：

[http://java.sun.com/javadoc/writingdoccomments/index.html](http://java.sun.com/javadoc/writingdoccomments/index.html)

[http://java.sun.com/javadoc/index.html](http://java.sun.com/javadoc/index.html)

[http://download.oracle.com/javase/1.5.0/docs/tooldocs/windows/javadoc.html](http://download.oracle.com/javase/1.5.0/docs/tooldocs/windows/javadoc.html)

文档注释描述Java的类、接口、构造器、方法以及字段(field)。每个文档注释都会被置于注释定界符/**...*/之中，一个注释对应一个类、接口或成员。该注释应位于声明之前。

#### 2.2.2.1  类注释

类注释模板 

/**

 * 版权所有新媒传信科技有限公司。保留所有权利。<br>

 * 作者：${USER} on ${DATE} ${HOUR}:${MINUTE}

 * 项目名：和飞信 - Android客户端<br>

 * 描述：

 *@version 1.0

 *@since JDK1.7.0_51

 */

USER默认取的当前计算机名，如果要变成自己的名字，在Android Studio下需要修改![](http://git.feinno.com/pub/guidelines/uploads/f740c0b3f20a87956489d6a6bbe1003d/studioconfig.png)

在文件末尾添加[-Duser.name=yourname 即可。

#### 2.2.2.2  方法注释

以实际用到的方法为例

```
/**
   * 创建菜单--组合权根菜单数据
   * @param groupUserMenuEntitys 角色对应的菜单集合
   * @param normalMenuList 缓存菜单数据集合
   * @return 组合权根菜单后的数据
   * @exception EOSException 判断菜单时效性时，有可能出现的异常
   */
public List getUserRoleMenu(Object[] groupUserMenuEntitys, List normalMenuList) throws EOSException {
```



## 2.3 申明 

### 2.3.1  每行变量申明个数 

推荐一行只申明一个变量，因为这样有利于写注释；不允许不同类型的变量在一行申明。如果是相同类型的申明临时变量，可以允许一行申明多个。

```
int level;  // 推荐
  int size;   //

  int level, size; //不推荐

  int tmp1, tmp2; //允许

  int foo, fooarray[]; //禁止

```



### 2.3.2   变量初始化 

尽量在声明局部变量的同时初始化。有两个理由可以例外：

*   变量的初始值依赖于某些先前发生的计算。

*   变量的初始化很耗费资源或时间，而这个变量在代码中有不会用到的可能性。

这两个情况下，也需要把变量申明为null或0。

```
int[] x = null;
if(condition){
  return;
}
x = new int[60000];
```



### 2.3.3  变量申明布局 

只在代码块的开始处声明变量。（一个块是指任何被包含在大括号"{" 和"}"中间的代码。）不要在首次用到该变量时才声明。这会把注意力不集中的程序员搞糊涂，同时会妨碍代码在该作用域内的可移植性。

避免声明的局部变量覆盖上一级声明的变量，避免子类中申明的变量覆盖了父类的变量。例如，不要在内部代码块中声明相同的变量名。

### 2.3.4   类和接口申明 

当编写类和接口时，应该遵守以下格式规则：

*   左大括号"{"位于声明语句同行的末尾

*   右大括号"}"另起一行，与相应的声明语句对齐，除非是一个空语句，"}"应紧跟在"{"之后 



*   方法与方法之间用空行分割

    ```
    class Sample extends Object {
      int ivar1;
      int ivar2;

      Sample(int i, int j) {
        ivar1 = i;
        ivar2 = j;
      }

      int emptyMethod() {}
      ...
    }

    //这种方式也可以，但是必须保证整个工程具有统一风格，不得混用，此处这种方式只是说可用，编码过程中还是尽量统一用第一种
    class Sample extends Object 
    {
      int ivar1;
      int ivar2;

      Sample(int i, int j) 
    {
        ivar1 = i;
        ivar2 = j;
      }

      int emptyMethod() {}
      ...
    }

    ```

    ​

## 2.4 语句 

### 2.4.1  简单语句 

每行至多包含一条语句，不允许多条语句包含在一行之中。

### 2.4.2  复合语句 

复合语句是包含在大括号中的语句序列，形如"{  语句 }"。如下几段所示：

*   被括其中的语句应该较之复合语句缩进一个层次。

*   左大括号"{"应位于复合语句起始行的行尾；右大括号"}"应另起一行并与复合语句首行对齐。

*   大括号可以被用于所有语句，包括单个语句，只要这些语句是诸如if-else或for控制结构的一部分。这样便于添加语句而无需担心由于忘了加括号而引入bug。

### 2.4.3  返回语句 

返回语句返回的值不需要使用括号括起来。

### 2.4.4    if,if-else,ifelse-if else 语句

if-else语句有如下几种格式：

```
class Sample extends Object {
  int ivar1;
  int ivar2;

  Sample(int i, int j) {
    ivar1 = i;
    ivar2 = j;
  }

  int emptyMethod() {}
  ...
}

//这种方式也可以，但是必须保证整个工程具有统一风格，不得混用，此处这种方式只是说可用，编码过程中还是尽量统一用第一种
class Sample extends Object 
{
  int ivar1;
  int ivar2;

  Sample(int i, int j) 
{
    ivar1 = i;
    ivar2 = j;
  }

  int emptyMethod() {}
  ...
}

```



### 2.4.5   for  语句

for语句有以下格式

```
//正常for语句
for(initialization; condition; update) {
    statements;
  }

  //所有的操作都在initialization; condition; update中完成
for(initialization; condition; update);

```



### 2.4.6  while 语句

While语句有如下格式

```
//正常while语句
while(condition) {
    statements;
  }

  //所有的操作都在condition中完成
while(condition);
```



### 2.4.7   do-while 语句

Do-while有如下格式

```
do {
    statements;
  } while(condition);

```



### 2.4.8   switch 语句

一个switch语句有如下格式

```
switch (condition) {
  case ABC://
    statements;
    /* 不结束，继续下一个语句 */
  case DEF:
    statements;
    break;
  case XYZ:
    statements;
    return; //返回，不需要break
  default:
    statements;
    break;
  }

```



### 2.4.9   try-catch 语句

```
//正常try语句
try {
    statements;
  } catch (ExceptionClass e) {
    statements;
  }

  //带finally的try语句
  try {
    statements;
  } catch (ExceptionClass e) {
    statements;
  } finally {
    statements;
  }

  /*
* 对于需要显示释放资源的对象，为了避免资源泄漏，
* 必须要放入try-finally语句中释放
*/
  SomeObj so = null;
  try{
    so = getObj();
    so.doSomething();
  }finally{
    if(so != null){
      so.close();
}
}

```



## 2.5 空白 

### 2.5.1   空行 

空行将逻辑相关的代码段分割开，以提高代码的可读性。

下列情况应该总是使用两个空行：

*   一个源文件的两个片段(section)之间

*   类声明和接口声明之间 

下列情况应该总是使用一个空行：

*   两个方法之间

*   方法内的局部变量和方法的第一条语句之间

*   块注释或单行注释之前

*   一个方法内的两个逻辑段之间，用以提高可读性 

## 2.6 命名规范 

规范的命名使得程序更易读，从而更易于理解。它们也可以提供一些有关标识符功能的信息，以助于理解代码 。

好的命名，可以使我们看到它即可以知道其功能和功用。

| **标示符类型** | **命名约定**                                 | **例子**                                   |
| --------- | ---------------------------------------- | ---------------------------------------- |
| 包         | 全部小写。    标识符用点号分隔开来。为了使包的名字更易读，Sun 公司建议包名中的标识符用点号来分隔。    Sun 公司的标准 java 分配包用标识符 .java 开头。   全局包的名字用你的机构的 Internet 保留域名开头 。 | 局部包：  interface.screens  全局包：  com.rational.www.** **interface.screens |
| 类，接口      |  类的名字应该使用名词。   每个单词第一个字母应该大写。    避免使用单词的缩写，除非它的缩写已经广为人知，如HTTP。 | Class Hello ;  Class HelloWorld ;  Interface Apple ; |
| 方法        |  第一个单词一般是动词。  第一个字母是小些，但是中间单词的第一个字母是大写。   如果方法返回一个成员变量的值，方法名一般为get+成员变量名，如若返回的值是bool变量，一般以is作为前缀。   如果方法修改一个成员变量的值，方法名一般为：set + 成员变量名。 | getName();  setName();  isFirst();       |
| 变量        |  第一个字母小写，中间单词的第一个字母大写。   不要用_或&作为第一个字母。   尽量使用短而且具有意义的单词。    单字符的变量名一般只用于生命期非常短暂的变量。i,j,k,m,n一般用于integers；c,d,e一般用于characters。   如果变量是集合，则变量名应用复数。    命名组件采用匈牙利命名法，所有前缀均应遵循同一个组件名称缩写列表。 | String myName;  int[] students;  int i;  int n;  char c;  btNew;  (bt是Button的缩写) |
| 常量        |   所有常量名均全部大写，单词间以‘_’隔开。                 | int MAX_NUM;                             |

## 2.7 编程惯例 

### 2.7.1  提供对变量的访问控制 

若没有足够理由，不要把实例或类变量声明为public。通常提供setXXX和getXXX方法来访问。

当类只作为数据结构，没有行为的时候，可以把变量申明为public。

### 2.7.2   引用类变量和类方法 

避免用一个对象访问一个类的静态变量和方法。应该用类名替代。例如：

```
public class Example{
  public static int a = 100;
  public static void test(){
    
  }

  public static void main(String[] args){
    int k;

    //这种方式可以
    test();
    k = a;

    //这种方式可以
    Example.test();
    k = Example.a;

    //不允许这种访问方式
    Example e = new Example();
    e.test();
    k = e.a;
  }
}

```



### 2.7.3   常量 

位于for循环中作为计数器值的数字常量，除了-1,0和1之外，不应被直接写入代码。如：

```
public final static int MAX_COUNT = 100;
  public static void test(){
    //避免这种情况
    for(int i=0; i<100; i++){
      //...do something...
    }
    
    //推荐这种使用方法
    for(int i=0; i<MAX_COUNT; i++){
      //...do something...
    }
  }

```



### 2.7.4   变量赋值 

```
//不要在一条语句中给多个变量赋相同的值
    a = b = 5;
    
    //不要写出难以理解或复杂的赋值语句
    e = (a = b + c) + d;//避免
    a = b + c;//写成这样就可以了
    e = a + d;

```



### 2.7.5   其他惯例 



#### 2.7.5.1  圆括号

一般而言，在含有多种运算符的表达式中使用圆括号来避免运算符优先级问题，是个好方法。即使运算符的优先级对你而言可能很清楚，但对其他人未必如此。你不能假设别的程序员和你一样清楚运算符的优先级。

```
 if (a == b && c == d)     // 避免
  if ((a == b) && (c == d))  // 正确
```



#### 2.7.5.2  返回值

设法让你的程序返回直接表述目的而且优雅。如：

```
//不妥1
  if (booleanExpression) {
      return true;
  } else {
      return false;
  }

  //这样更好
return booleanExpression;


  //不妥2
  if (condition) {
      return x;
  }
  return y;

  //这样更好
return (condition ? x : y);

```



#### 2.7.5.3  条件运算符”?”前的表达方式

如果一个包含二元运算符的表达式出现在三元运算符" ? : "的"?"之前，那么应该给表达式添上一对圆括号。例如：

  (x>= 0) ? x : -x;

## 2.8 Java 代码范例

### 2.8.1   [Java 源文件代码范例

下面范例展示了一个有单一公共类的Java源文件。Java接口类和这个类似。

```
package com.feinno.rcs;

import java.util.Hashtable;

/**
* @Description: 这是一个Java编码规范的演示代码
 * @author authorname
 * @data：2015-12-9上午11:25:10
 * Copyright (c) 新媒传信科技有限公司。保留所有权利

*/

public class Example extends Hashtable {

/** 类变量1注释 */
  public static int classVar1;

  /** 
   * 超过一行的类变量注释。这个注释是超过
   * 一行的
   */
private static Object classVar2;

  /** 类变量注释 */
  public Object instanceVar1;

  /** 类变量注释 */
  protected int instanceVar2;

  /** 私有变量注释 */
  private Object[] instanceVar3;

  /** 
   * 构造方法注释
   */
  public Blah() {
    // ...内容实现...
  }

  /**
   * ...方法注释...
   */
  public void doSomething() {
    // ...内容实现...
  }

  /**
   * ...带参数的方法...
   * @param someParam Object
   * 输入参数
   */
  public void doSomethingElse(Object someParam) {
    // ...内容实现... 
  }

  /**
   * ...有返回值的方法...
   * @return int
   * 当...的时候返回0
   */
  public int doSomething2(){
    //...内容实现...
    return 0;
  }

  /**
   * ...有异常的方法...
   * @throws Exception
   * 当...时抛出异常
   */
  public void doSomething3() throws Exception {
    //...内容实现...
  }
}

```



# 3  Android 开发规范

安卓应用开发除遵循以上Java开发规范外还要注意以下安卓相关开发规范：

## 3.1    开发工具和编译环境统一规范 

### 3.1.1  开发工具 

Android的开发工具统一使用Android Studio，包括对接的业务部门，必须使用Android Studio，团队内Android Studio的版本号必须一致。

### 3.1.2  编译环境 

在Android Studio中编译环境配置团队内配置必须统一，包括：Gradle版本、compileSdkVersion、buildToolsVersion、minSdkVersion、targetSdkVersion、引用的Android SDK统一、JDK版本号、Git版本号。

## 3.2    应用架构规范 

### 3.2.1  工具集及通用能力封装 

客户端需具备一个工具类moudle，这个工具类与业务无关，主要为业务开发提供帮助，包括文件、String、bitmap、正则、日期、系统信息等，成员在开发过程中如遇一些工具类的帮助，需参考项目开发文档中的工具类DOC，不得随意自己二次开发。

通用能力封装是指在业务逻辑层或者视图层，不同的业务或者视图用到了相同的基本能力或者组件，那么这类型业务称之为通用业务或者通用组件，比如BaseActivity，BaseFragment，二维码，自定义组件、Activity管理器、Fragment管理器等。成员需要在通用能力模块之上进行开发，并不断抽取具备通用能力的SDK。

### 3.2.2  业务逻辑层 

业务逻辑层只能执行跟业务相关的代码，不得有和UI相关的任何代码，业务逻辑层应该是一个独立的moudle，方便业务的移植或拓展。业务模块之间要尽量解耦，核心业务模块和扩展业务模块之间必须完全解耦，通过接口封装调用来实现二者之间的交互。

业务逻辑层中的协议层和数据层要和业务完全解耦，只为业务提供基础能力，不得在协议层或者数据封装层写与业务强相关的代码。

### 3.2.3  视图层 

视图层主要是页面相关的代码逻辑，主要包括四大组件的应用，所有视图都要继承自一个基类，方便对所有界面的统一管理。

视图层需要有自己的基础组件库，包括自定义的控件，所有界面对控件的调用遵循统一调用原则，成员不得二次开发已经存在的控件，不得随意更改公有控件的属性。

### 3.2.4  模块依赖 

根据业务的实际情况，尽量对项目进行分模块管理，不能只在一个模块中进行应用的开发，从而造成后期项目无法维护和拓展。Android Studio中模块依赖是有传递性的，模块间要避免交叉依赖和重复依赖。比如Moudle A依赖Moudle B，MoudleB依赖Moudle C，那么A 自然也能引用C。

### 3.2.5  第三方包管理 

第三方主要分为Jar包，.so包，aar包。所有第三方包的命名必须见名知义，不能随意命名。包的位置需要和对应的模块匹配，不能随意挪动包的位置和修改包的版本。如果客户端中的包比较多，上层模块的包要放置在一个单独的lib模块中统一管理，并且这个模块只能放置第三方包。对于每个包的介绍都需要在客户单开发文档中有详细记录，方便后期维护。比如下表：

| 三方包名称 | 三方包类型 | 功能描述 |
| ----- | ----- | ---- |
|       |       |      |
|       |       |      |

 

### 3.2.6  OpenAPI 

客户端对外提供的API必须是在端内经过二次封装对外暴露，禁止直接暴露客户端业务逻辑底层代码。API要有详细的描述和对应的DOC，方便调用方集成和客户端对API的维护升级工作。

## 3.3    方法的封装规范 

方法的封装首先必须得遵循JAVA代码开发规范，在Android项目中，对于视图层中的方法封装需要遵循统一原则。

### 3.3.1  Activity 类内部的方法封装

禁止在Activity的生命周期方法内堆积代码，将所有的代码按照功能逻辑进行封装抽成方法并写明注释，在所有的Activity的写作过程中，都要有一个统一的代码模板

以下是某Act的代码模板

```
package com.feinno.rcsfetion.activity.call;

import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;

import com.feinno.rcsfetion.activity.BaseAct;

/**
 * 版权所有 新媒传信科技有限公司。保留所有权利。<br>
 * 作者：wangxiaohong on 2017/04/19 14:33
 * 项目名：和飞信 - Android客户端<br>
 * 描述：
 *
 * @version 1.0
 * @since JDK1.7.0_51
 */
public class CallAct extends BaseAct implements View.OnClickListener,AdapterView.OnItemClickListener{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        init();
    }

    /**
     * 初始化
     */
    private void init() {
        initView();
        initData();
    }

    /**
     * 初始化视图
     */
    private void initView() {
        //TODO findviewById setOnClick
    }

    /**
     * 初始化数据
     */
    private void initData() {
        //TODO 数据初始化
        getSomeStingFromNet();
    }
    /**
     * 通过从服务器获取到的Bean或者其他异步线程获取的Bean更新UI视图
     * @param obj
     */
    private void setValues(Object obj) {

    }

    /**
     * 后退事件处理
     */
    private void backAction() {
        //TODO 按下物理后退键或者点击后退按钮需要出现的事件，比如释放资源，finish()等
    }

    @Override
    protected void onResume() {
        super.onResume();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();

    }

    @Override
    public void onClick(View v) {
        
        
    }

    @Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {

    }
}

```



### 3.3.2   fragment 类内部的封装

其方法封装和Activity写法一致，但是由于其特有的生命周期，需要在其OnViewCreate中通过传递rootview来initView(rootview),在OnActivityCreate中进行initData()。

### 3.3.3   其他类内部的封装 

全部遵循一个原则，方法尽量封装，避免好几百行的方法实现，这样别的团队成员就无法短时间熟悉对方写的代码。

## 3.4    命名规范 

### 3.4.1  统一命名规则

1.      包名小写。

cn.com.fetion

cn.com.fetion.utils

2.      类名大小写混合组成，遵循驼峰标识，头字母大写。

class SessionManager

class FrescoConfig

3.      接口大小写混合组成，遵循驼峰标识，头字母大写。

Interface ILoginEventListener

Interface ISessionObserver

4.      方法大小学混合组成，首字母小写，其后单词的首字母大写。

List<Contact> getContactList()

void clear()

5.      变量、参数小写，不推荐使用下划线，简短明晰。

int openType;

boolean isfinish;

6.      集合数组应该从命名中体现复数的含义，例如加后缀s或者后缀List。

List<Contact> contactList

List<Selector> selectList;

7.      在定义类时，应该按照访问权限的大小分别排列属性和方法。

8.      public

9.      protected

10.      包级别(没有访问修饰符的，默认为friendly)

11.      private

### 3.4.2  控件命名规则

控件的变量命名统一遵循m+控件类型+控件功能描述，必须见名知义，对于某些布局复杂的界面，控件会比较多，在控件初始化时需要成员通过对应的控件ID去初始化控件，这个时候控件的命名就必须见名知义，对于不能完全表述的，需要加注释加以说明，比如

```
private SGridView mGridViewMember;

private RelativeLayout mLayoutGroupName;

private TextView mTextViewGroupCount;

private TextView mTextViewGroupName;

private RelativeLayout mLayoutBarCode;

private LinearLayout mLayoutNotice;

private TextView mTextViewNotice;

private TextView mTextViewfile;//群内文件

private TextView mTextViewChatlog;

private UISwitchButton mSwitchDnd;//是否开启群主验证
```

 

## 3.5    Res 文件资源规范

资源文件主要分为drawable、layout、icon、color、dimen、string、style。资源命名统一遵循的规范模板为：

1.        前缀_资源类型/控件类型_模块名_逻辑名称1_逻辑名称2…

2.        对于统一调用的资源，统一加common前缀,模板为前缀_common_资源类型_逻辑名称_自定义控件描述

3.        对于layout布局文件，结尾统一加layout,比如

rf_activity_group_groupinfo_layout.xml

禁止出现数字，只能用字母下划线组合。

| 资源类型                                     | 描述          |
| ---------------------------------------- | ----------- |
| rf_drawable_contact_edit_bg.xml          | 自定义drawable |
| rf_activity_group_groupinfo_layout.xml   | Layout布局文件  |
| rf_img_session_add_contact.png           | Icon命名      |
| rf_dimens_calllog_calldetail_bottom_height | 尺寸命名        |
| rf_color_calllog_xxx_xxx                 | 颜色命名        |
| rf_str_xxx_xxxx                          | 文本命名        |
| rf_style_xxx_xxx                         | 风格命名        |

### 3.5.1  Style XML

将layout中不断重现的style提炼出通用的style通用组件，放到styles.xml中。

比如共用的TextView、Tab、RadioButton、RatingBar等等，根据项目的实际情况进行提炼，也不可过度提炼，并在每个style前标明详尽的注释。抽出来的这个组件一定是公用的。

### 3.5.2  Color XML

一个APP的颜色其实并没有多少，但是因为项目成员变更和协作的过程中不断的调整和新增颜色，会导致整个APP的color xml杂乱无章，后期在UI调整上时间成本较高，所以在项目开始初期就得制定好颜色的相关规范，整个规范是和设计的UI规范是相对应的。按照谷歌IO的最新设计规范，那么一个应用中主要用的颜色规范主要有如下几类：

```
<!-- 系统主色-->
<color name="primary_color">#a82424</color>
<!-- 系统主色中的深色-->
<color name="primary_dark_color">#7e181c</color>
<!-- 系统辅色-->
<color name="rf_color_accent">#30a8dc</color>
<!-- 系统背景颜色-->
<color name="rf_color_app_common_background ">#EFEFEF</color>
<!-- 主标题字体颜色，常规是黑色-->
<color name=" rf_color_common_black_text ">#494949</color>
<!-- 浅灰色字体颜色，一般用在hint内-->
<color name=" rf_color_common_dark_gray_text ">#666666</color>
<!-- 淡灰色，更浅的一种字体颜色，一般是list中的副标题-->
<color name=" rf_color_common_light_gray_text">#898989</color>
<!-- 分割线颜色-->
<color name=" rf_color_split_line ">#d8d8d8</color>
<!-- 透明色-->
<color name=" rf_color_transparent ">#00000000</color>
<!-- 列表Item常规颜色-->
<color name=" rf_color_list_item_normal_bg">#ffffff</color>
<!-- 列表Item点击颜色-->
<color name=" rf_color_list_item_normal_pressed_bg">#d3d6d7</color>
<!-- 半透明色-->
<color name=" rf_color_translucence ">#5f000000</color>
<!-- 图片默认占位色-->
<color name=" rf_color_icon_placeholder ">#e1e4eb</color>
<!-- 红色按钮点击颜色-->
<color name=" rf_color_btn_red_pressed">#962024</color>
<!-- 红色按钮常规颜色-->
<color name=" rf_color_btn_red_normal">#a82424</color>
<!-- 按钮不可点击色-->
<color name=" rf_color_btn_enable_color">#d3d6d7</color>
<!-- 白色-->
<color name=" rf_color_white">#ffffff</color>

```



这只是一部分，可以按照实际开发情况再定义别的颜色，命名规则同前。

### 3.5.3  String XML 

Java代码中不出现中文，最多注释中可以出现中文，中文统一写在strings.xml中strings.xml中id的命名模式统一按照上表执行。里面的代码块按照模块区分出来，需要写明注释，不可全部写一起。

### 3.5.4  Dimens XML

控件单位大小，layout xml中的出现的控件的宽高或者间隙，其大小全部通过dimens定义，不可出现在layout.xml布局将控件写死的情况。

## 3.6    客户端与H5 统一交互规范

Native与H5的统一交互具体请参照客户端与H5交互统一协议。与H5的统一交互协议必须是与业务无关的，所有的WebView调用的都是继承自这个基本协议的扩展。

## 3.7    日志的使用规范 

代码中禁止出现对系统日志的直接调用，需要打印日志数据时要调用项目封装的带日志开关的日志类，便于生产上线时将所有日志关闭。

​         对于写入本地文件的日志，必须严格按照日志规定的级别进行写入，只能是i和e级别。不得在for循环中打印大量的数据，对于测试日志，在发布版本之前必须清除掉。

每条日志必须都得有对应的TAG，这个TAG值都必须是统一在配置文件中进行调用，禁止在项目中硬编码TAG值，比如”xuechenji”。

## 3.8    避免OOM 的代码规范

在android项目开发过程中，当一个对象已经不需要再使用了，本该被回收时，而另外一个正在使用的对象持有它的引用从而导致它不能被回收，这就导致本该被回收的对象不能被回收而停留在堆内存中，内存泄漏就产生了。由于Android中上下文Context的存在，所以一般在开发过程中，某个操作会一直持有某个Act的引用导致其不能被GC回收，从而OOM。

内存泄漏有什么影响呢？它是造成应用程序OOM的主要原因之一。由于android系统为每个应用程序分配的内存有限，当一个应用中产生的内存泄漏比较多时，就难免会导致应用所需要的内存超过这个系统分配的内存限额，这就造成了内存溢出而导致应用Crash。

### 3.8.1  单例造成的内存泄漏 

Android的单例模式在开发过程中广泛使用，不过使用的不恰当的话也会造成内存泄漏。因为单例的静态特性使得单例的生命周期和应用的生命周期一样长，这就说明了如果一个对象已经不需要使用了，而单例对象还持有该对象的引用，那么这个对象将不能被正常回收，这就导致了内存泄漏。

错误的写法：

```
public class AppManager {
    private static AppManager instance;
    private Context context;
    private AppManager(Context context) {
        this.context = context;
    }
    public static AppManager getInstance(Context context) {
        if (instance != null) {
            instance = new AppManager(context);
        }
        return instance;
    }
}

```



这是一个普通的单例模式，当创建这个单例的时候，由于需要传入一个Context，所以这个Context的生命周期的长短至关重要：

1、传入的是Application的Context：这将没有任何问题，因为单例的生命周期和Application的一样长；

2、传入的是Activity的Context：当这个Context所对应的Activity退出时，由于该Context和Activity的生命周期一样长（Activity间接继承于Context），所以当前Activity退出时它的内存并不会被回收，因为单例对象持有该Activity的引用。

正确的单例应该修改为下面这种方式

```
public class AppManager {
    private static AppManager instance;
    private Context context;
    private AppManager(Context context) {
        this.context = context.getApplicationContext();
    }
    public static AppManager getInstance(Context context) {
        if (instance != null) {
            instance = new AppManager(context);
        }
        return instance;
    }
}
	
```

这样不管传入什么Context最终将使用Application的Context，而单例的生命周期和应用的一样长，这样就防止了内存泄漏。

### 3.8.2  非静态内部类创建静态实例造成的内存泄漏 

有的时候我们可能会在启动频繁的Activity中，为了避免重复创建相同的数据资源，会出现这种写法：

```
public class MainActivity extends AppCompatActivity {
    private static TestResource mResource = null;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        if(mResource == null){
           mResource = new TestResource();
        }
        //...
    }
    class TestResource {
        //...
    }
}
```

这样就在Activity内部创建了一个非静态内部类的单例，每次启动Activity时都会使用该单例的数据，这样虽然避免了资源的重复创建，不过这种写法却会造成内存泄漏，因为非静态内部类默认会持有外部类的引用，而又使用了该非静态内部类创建了一个静态的实例，该实例的生命周期和应用的一样长，这就导致了该静态实例一直会持有该Activity的引用，导致Activity的内存资源不能正常回收。正确的做法为：

将该内部类设为静态内部类或将该内部类抽取出来封装成一个单例，如果需要使用Context，请使用ApplicationContext 。

### 3.8.3  Handler 造成的内存泄漏

```
public class MainActivity extends AppCompatActivity {
    private Handler mHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            //...
        }
    };
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        loadData();
    }
    private void loadData(){
        //...request
        Message message = Message.obtain();
        mHandler.sendMessage(message);
    }
}
```

这种写法在Eclipse中编译器不会任何提示，但是在Android Studio中编译器会直接告诉你这么写可能会导致OOM。

这种创建Handler的方式会造成内存泄漏，由于mHandler是Handler的非静态匿名内部类的实例，所以它持有外部类Activity的引用，我们知道消息队列是在一个Looper线程中不断轮询处理消息，那么当这个Activity退出时消息队列中还有未处理的消息或者正在处理消息，而消息队列中的Message持有mHandler实例的引用，mHandler又持有Activity的引用，所以导致该Activity的内存资源无法及时回收，引发内存泄漏，所以另外一种做法为：

```
public class MainActivity extends AppCompatActivity {
    private MyHandler mHandler = new MyHandler(this);
    private TextView mTextView ;
    private static class MyHandler extends Handler {
        private WeakReference<Context> reference;
        public MyHandler(Context context) {
            reference = new WeakReference<>(context);
        }
        @Override
        public void handleMessage(Message msg) {
            MainActivity activity = (MainActivity) reference.get();
            if(activity != null){
                activity.mTextView.setText("");
            }
        }
    }
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mTextView = (TextView)findViewById(R.id.textview);
        loadData();
    }
 
    private void loadData() {
        //...request
        Message message = Message.obtain();
        mHandler.sendMessage(message);
    }
}
```

 创建一个静态Handler内部类，然后对Handler持有的对象使用弱引用，这样在回收时也可以回收Handler持有的对象，这样虽然避免了Activity泄漏，不过Looper线程的消息队列中还是可能会有待处理的消息，所以我们在Activity的Destroy时或者Stop时应该移除消息队列中的消息，更准确的做法如下：

```
@Override
    protected void onDestroy() {
        super.onDestroy();
        mHandler.removeCallbacksAndMessages(null);
    }
```

使用mHandler.removeCallbacksAndMessages(null);是移除消息队列中所有消息和所有的Runnable。当然也可以使用mHandler.removeCallbacks();或mHandler.removeMessages();来移除指定的Runnable和Message。

### 3.8.4  线程造成的内存泄露 

对于线程造成的内存泄漏，也是平时比较常见的，如下这两个示例可能每个人都这样写过：

```
//——————test1
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                SystemClock.sleep(10000);
                return null;
            }
        }.execute();
//——————test2
        new Thread(new Runnable() {
            @Override
            public void run() {
                SystemClock.sleep(10000);
            }
        }).start();

```



上面的异步任务和Runnable都是一个匿名内部类，因此它们对当前Activity都有一个隐式引用。如果Activity在销毁之前，任务还未完成，那么将导致Activity的内存资源无法回收，造成内存泄漏。正确的做法还是使用静态内部类的方式，如下：

```
static class MyAsyncTask extends AsyncTask<Void, Void, Void> {
        private WeakReference<Context> weakReference;
 
        public MyAsyncTask(Context context) {
            weakReference = new WeakReference<>(context);
        }
 
        @Override
        protected Void doInBackground(Void... params) {
            SystemClock.sleep(10000);
            return null;
        }
 
        @Override
        protected void onPostExecute(Void aVoid) {
            super.onPostExecute(aVoid);
            MainActivity activity = (MainActivity) weakReference.get();
            if (activity != null) {
                //...
            }
        }
    }
    static class MyRunnable implements Runnable{
        @Override
        public void run() {
            SystemClock.sleep(10000);
        }
    }
//——————
    new Thread(new MyRunnable()).start();
    new MyAsyncTask(this).execute();

```



这样就避免了Activity的内存资源泄漏，当然在Activity销毁时候也应该取消相应的任务AsyncTask::cancel()，避免任务在后台执行浪费资源。

### 3.8.5  资源未关闭造成的内存泄漏 

对于使用了BraodcastReceiver，ContentObserver，File，Cursor，Stream，Bitmap等资源的使用，应该在Activity销毁时及时关闭或者注销，否则这些资源将不会被回收，造成内存泄漏。

上述引起OOM的代码编程习惯必须改正，OOM并不是一味地由于BitMap引起，如果小细节的OOM的处理不好，导致分配给应用的内存吃紧，那么在长时间操作APP的时候，在某个时间节点就会导致OOM，引起Crash。

 

## 3.9    开源框架的引入规范 

引用的开源框架必须是当前市场上最为成熟最为稳定的版本，不得使用beta版，对于某些框架的引用，如果需要修改源代码，那就需要集成源码，并需要做好开源框架的版本更新。

## 3.10       第三方开源jar 的引入规范

Jar包必须稳定成熟，不得在项目中集成不同版本号相同功能的jar包，每个jar包的命名必须见名知义，并需要在开发文档中有对jar的项目描述。

## 3.11       扩展业务SDK 集成规范

所有集成进主项目的SDK必须以aar包的形式提供，aar包的命名必须见名知义，不得出现release或者debug字样，不接受非Android Studio环境编译出来的SDK。所有SDK的接入需要遵循统一的接入规范，受约于主端制定的标准，方便SDK的统一管理。接入准则主要通过数据、UI、资源、日志、文件、基础能力等维度进行制定。

### 3.11.1  数据初始化

各SDK需提供数据初始化方法，主端在应用初始化时统一进行初始化配置。

### 3.11.2  清单数据一致性 

客户端需对外提供一份详细的依赖和引用开源架构的清单文件，包括Jar包引用、框架引用、源码引用，扩展业务部门在开发前需对照清单文件，引入和主端一致的第三方资源，从而保证了多方在引用数据方面的一致性。

| 客户端Jar包引用清单及简介                      |
| ----------------------------------- |
| 客户端Gradle complie  dependience清单及简介 |
| 客户端引用的开源框架清单及简介                     |
| 客户端源码引用介绍                           |
| 客户端.so包引用清单及介绍                      |

 

### 3.11.3  数据统一性 

主端会与各方SDK进行数据交互，每个扩展业务可能需要的数据不尽相同，主端需要提供统一的OpenAPI供各扩展AS SDK进行调用，从而保证了主端和各扩展服务SDK的数据统一性。

| OpenAPI                                  | 描述              |
| ---------------------------------------- | --------------- |
| getToken(String asName)                  | 获取当前用户登录后的token |
| getSSOToken(String asName)               | 获取登录后的SSO认证     |
| getContactList()                         | 获取联系人列表         |
| getContactLabelInfo()                    | 获取联系人标签信息       |
| getCarrier()                             | 获取当前运营商         |
| getEpid()                                | 获取登录用户的手机基本信息   |
| isLogin()                                | 用户是否登录          |
| getLocationInfo()                        | 获取用户位置信息        |
| getUserInfo(int userId, String  phone, String groupUri, String asName) | 获取当前登录用户的基本信息   |
| getGroupList()                           | 获取群组列表          |
| getGroupInfo(String groupUri)            | 获取群组信息          |

 

### 3.11.4  业务交互统一性 

主端某些功能的交互形式可能会与扩展服务SDK中的交互形式会有很大的相似性，为了保证整体风格的统一，避免二次开发，主端对外暴露出界面相关的OpenAPI。

| OpenAPI             | 描述       |
| ------------------- | -------- |
| openImgSelector     | 打开图片选择器  |
| openPreViewImg      | 打开预览图片   |
| openMapView         | 打开地图界面   |
| openSmartVideo      | 打开小视频    |
| openFileSelector    | 打开文件选择器  |
| openContactSelector | 打开联系人选择器 |

 

### 3.11.5  通用能力统一性 

在开发过程中，各业务都会用到统一的工具类，为了提高代码的复用性和多端技术的统一性，主端对外暴露了完整的通用能力OpenAPI。

| OpenAPI         | 描述       |
| --------------- | -------- |
| FileUtils       | 文件操作     |
| StorageUtils    | 存储操作     |
| SystemUtils     | 系统相关信息获取 |
| SimUtils        | Sim卡相关信息 |
| DateUtils       | 日期操作     |
| BitMapUtils     | 图片操作     |
| StringUtils     | 文本操作     |
| permissionUtils | 权限操作     |
| XMLUtils        | XML解析操作  |
| JsonUtils       | Json解析操作 |
| logUtils        | 日志操作     |
| channelUtils    | 渠道相关信息   |

 

### 3.11.6  资源命名统一性 

各扩展服务SDK提供的资源文件需要遵守统一的规范，方便主端进行管理，并避免因为命名不规范而引起的资源文件名冲突。所有资源文件必须带上扩展服务的名称缩写。

| 资源类型  | 命名规范                |
| ----- | ------------------- |
| 图片    | xxx_img_模块名_zzz     |
| 布局    | xxx _layout_模块名_zzz |
| 动画    | xxx _anim_模块名_zzz   |
| 文本    | xxx _str_模块名_zzz    |
| 尺寸    | xxx _dimen_模块名_zzz  |
| 颜色    | xxx _color_模块名_zzz  |
| 风格    | xxx _style_模块名_zzz  |
| 自定义组件 | xxx _drawable_zzz   |

 

### 3.11.7  日志记录统一性 

主端对外提供完整的日志记录openAPI，包括日志打印和日志写入文件，各AS统一使用主端提供的日志API，不得私自调用系统提供的日志API，方便日志的统一管理和实时跟踪。

日志中需要为每个业务模块设置TAG标签，TAG标签统一常量配置，不可在代码中硬编码写入，只有info级别和error级别的日志才能写入文件，info一般是对重要业务数据的打印，error是对异常数据或者发生异常的方法处进行打印。

| 日志级别    | 日志控制台打印 | 日志本地文件记录 |
| ------- | ------- | -------- |
| info    | [√      | √        |
| debug   | √       |          |
| warning | √       |          |
| error   | √       | √        |

 

### 3.11.8  文件缓存与存储统一性 

扩展服务需遵循主端文件缓存路径的规范，需将扩展服务的文件缓存放置在主端指定的位置，方便主端进行文件的统一管理和缓存大小统计及清除工作。

SDK不可将文件存储时的路径在代码中硬编码，需要通过获取包名进行存储，需要存储在SD卡中的文件，需要调用主端提供的父路径，将文件和主端的文件放置在一个父目录下集中管理。

## 3.12       禁止操作 

1.      禁止私自创建包名，如果需要新建包名，需要上报。

2.      禁止私自修改包名和包结构

3.      禁止私自引入jar包，引入前必须上报，并在开发文档中补充jar的描述

4.      禁止私自修改他人的代码，如需修改，需要和他人一起合作或者指出问题让代码责任人去修改

5.      禁止私开线程池

6.      禁止私自修改gradle配置，需上报修改

7.      禁止私自调整数据初始化时各业务模块的顺序

8.      禁止随意在业务模块中写工具类相关的方法，需要先查工具集中是否已有，如果没有进行补充，如果有，直接调用。

9.      禁止随便修改或替换删除jar包

10.      禁止私自修改公有res资源的属性，比如内容描述或者颜色值

11.      禁止私自引入开源的框架，需上报评审

12.      禁止在项目中引入超过200kb的图片，需上报评审

## 3.13       异常处理 

### 3.13.1              不能忽视异常处理

如果像下面的代码这样，对catch后的异常作空处理，就像埋下地雷一样让人感觉到毛骨悚然：

错误的做法：

```
1	    void setServerPort(String value) {  
2	    try {  
3	        serverPort = Integer.parseInt(value);  
4	    } catch (NumberFormatException e) {  
5	    }  
6	}  
```



正确的做法(1)

在方法声明时抛出异常，由客户程序员去负责消化这个异常。

```
1	void setServerPort(String value) throws NumberFormatException {  
2	    serverPort = Integer.parseInt(value);  
}   
```

正确的做法（2）

```
1	  /** Set port. If value is not a valid number, 80 is substituted. */  
2	void setServerPort(String value) {  
3	    try {  
4	        serverPort = Integer.parseInt(value);  
5	    } catch (NumberFormatException e) {  
6	        serverPort = 80;  // default port for server  
7	    }  
    
```



正确的做法（3）

```
 /** Set port. If value is not a valid number, die. */  
9	void setServerPort(String value) {  
10	    try {  
11	        serverPort = Integer.parseInt(value);  
12	    } catch (NumberFormatException e) {  
13	        throw new RuntimeException("port " + value " is invalid, ", e);  
14	    }  
```

正确的做法（4）

```
15	void setServerPort(String value) throws ConfigurationException {  
16	    try {
17	        serverPort = Integer.parseInt(value);  
18	    } catch (NumberFormatException e) {  
19	        Logfeinn.e(TAG,”发生异常”，e)
20	    }
21	} 

```



### 3.13.2               不要偷懒而捕捉一般异常

下面代码一概捕捉Exception异常，大小通吃是不对的，这样会让你在错误出现时难以定位到错误原因，一般异常无法用统一方法进行异常处理。

错误的做法：

```
1	 try {
2	    someComplicatedIOFunction();        // may throw IOException  
3	    someComplicatedParsingFunction();   // may throw ParsingException  
4	    someComplicatedSecurityFunction();  // may throw SecurityException  
5	    // phew, made it all the way  
6	} catch (Exception e) {   	           // I'll just catch all exceptions  
7	    handleError();                      // with one generic handler!  
8	}

```



## 3.14       代码提交及管理 

代码管理器全部统一用git，上传到公司的GITLAB上[http://git.feinno.com](http://git.feinno.com)，建议每半个小时更新一次代码，提交一次代码，不得在本地堆积大量代码然后提交，容易造成代码冲突造成不必要的麻烦。

## 3.15       代码安全 

项目代码在发布前必须进行代码混淆，应用签名的证书文件和秘钥不得随意泄漏，只能由团队leader进行保管。团队内任何人禁止私自将项目源码外传，禁止泄漏项目中安全相关的技术，禁止随意打包debug版本给团队以外成员使用，如遇特殊情况，需上报评估备案。