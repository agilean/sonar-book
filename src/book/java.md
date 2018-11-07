# JAVA 

## 1 概述

这是我们推荐的一份 Java™ 编程语言风格规范，在参考了Google Style的基础之上编辑而成。涉及格式化、编码标准等等的基础的普遍需要遵循的规则。



### 1.1 术语说明

为避免歧义，Java语言中的术语使用原文，例如：import、class、package、method、enum、interface，Javadoc

## 2 文件要求

### 2.1 文件名

文件名大小写敏感

Note: 在Windows平台开发时特别注意，光改变文件名大小写不会被版本控制系统识别到，此时使用版本控制系统的内建的改名指令

    git mv
    svn rename

### 2.2 编码：UTF-8

文件编码字符集首选是：UTF-8，全部平台兼容

### 2.3 特殊字符

#### 2.3.1 空白符（Whitespace characters）

使用空格 (0x20) 缩进，空格比tab有更好的工具兼容性。

#### 2.3.2 非ASCII字符

在引用unicode字符时，直接输入字符(e.g. ∞) 亦或者使用unicode转义(e.g. \u221e) 都是可以的，使用哪一种取决于可读性。

Tip: In the Unicode escape case, and occasionally even when actual Unicode characters are used, an explanatory comment can be very helpful.

Examples:

| Example | Discussion |
| ------------- | ------------- |
|      String unitAbbrev = "μs"; | 最好的方法: perfectly clear even without a comment. |
| String unitAbbrev = "\u03bcs"; // "μs" |  Allowed, but there's no reason to do this. |
| String unitAbbrev = "\u03bcs"; // Greek letter mu, "s"  | Allowed, but awkward and prone to mistakes. |
| String unitAbbrev = "\u03bcs"; | Poor: the reader has no idea what this is. |
| return '\ufeff' + content; // byte order mark | Good: use escapes for non-printable characters, and comment if necessary. |
     
**Tip:** Never make your code less readable simply out of fear that some programs might not handle non-ASCII characters properly. If that should happen, those programs are broken and they must be fixed.

## 3 源文件结构

源文件内容的组成顺序：

* License or copyright information, if present
* Package statement
* Import statements
* Exactly one top-level class
* Exactly one blank line separates each section that is present.

### 3.1 版权声明

如果需要版权声明，就把它写上，使用一般的注释风格，例如 apache license：

    /*
     * Copyright 2002-2014 the original author or authors.
     *
     * Licensed under the Apache License, Version 2.0 (the "License");
     * you may not use this file except in compliance with the License.
     * You may obtain a copy of the License at
     *
     *      http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

### 3.2 Package statement

package statement 写在一行里，不要换行。

### 3.3 Import statements

#### 3.3.1 通配符

仅限使用静态和通配符导入静态的工具方法，枚举类型

例如：

    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod.*;
    @RequestMapping(method = POST)
    public Object some(String input) {
        if (hasText(input)) {
            //do 
        } else {
            throw new IllegalArgumentException("参数必填");
        }
    }

不使用`import package.*`；

    import com.foo.*;//坑

例外： `src/test`里面不限制，使用静态导入可以让测试代码流畅。

#### 3.3.2 不要换行

Import 语句不要换行

#### 3.3.3 顺序

Imports are ordered as follows:

* All static imports in a single block.
* All non-static imports in a single block.

使用IDE的整理功能来自动的整理import顺序；


#### 3.3.4 No static import for classes

Static import is not used for static nested classes. They are imported with normal imports.

### 3.4 Class declaration

#### 3.4.1 只能写一个顶层类

Each top-level class resides in a source file of its own.

内部使用的静态类，匿名类可以存在

#### 3.4.2 成员顺序

class 成员的顺序按照逻辑顺序来排，以方便阅读和理解。 

没有唯一正确的方法来排序内容; 不同的类可以以不同的方式排序其内容。最重要的是有逻辑顺序，如果被问到，维护者可以解释清楚。



##### 3.4.2.1 Overloads: never split

When a class has multiple constructors, or multiple methods with the same name, these appear sequentially, with no other code in between (not even private members).

## 4 格式化

Terminology Note: block-like construct refers to the body of a class, method or constructor. Note that, by Section 4.8.3.1 on array initializers, any array initializer may optionally be treated as if it were a block-like construct.

宗旨：自然美，阅读方便，兼容性好

### 4.1 大括号

#### 4.1.1 大括号在需要的地方使用

大括号与if，else，for，do和while语句一起使用，即使里面为空或仅包含单个语句。

    if(condition1)doJob();//不要这样。
    if(condition2){
      doJob();//这样，为了预防增补代码时忘记
    }

#### 4.1.2 非空语句块采用K&R风格

对于非空语句块，花括号遵循K&R风格：
 
- 左括号前不换行。
- 左括号后换行。
- 右括号前换行。
- 如果右括号结束一个语句块或者函数体、构造函数体或者有命名的类体，则需要换行。例如，当右括号后面接else或者逗号时，不换行。

例子：

    return () -> {
      while (condition()) {
        method();
      }
    };

    return new MyClass() {
      @Override public void method() {
        if (condition()) {
          try {
            something();
          } catch (ProblemException e) {
            recover();
          }
        } else if (otherCondition()) {
          somethingElse();
        } else {
          lastThing();
        }
      }
    };
    
A few exceptions for enum classes are given in Section 4.8.1, Enum classes.

#### 4.1.3 空语句块：使代码更简洁

空块或块状结构可以采用K＆R样式（如第4.1.2节所述）。 或者不换行写在一起“{}”

例子：

      // This is acceptable
      void doNothing() {}
    
      // This is equally acceptable
      void doNothingElse() {
      }
      // This is not acceptable: No concise empty blocks in a multi-block statement
      try {
        doSomething();
      } catch (Exception e) {}
      
### 4.2 语句块的缩进：2空格

每当一个新的语句块产生，缩进就增加两个空格。当这个语句块结束时，缩进恢复到上一层级的缩进格数。缩进要求对整个语句块中的代码和注释都适用。（例子可参考之前4.1.2节中的例子）。

### 4.3 每行一个表达式

每个语句后面都有一个换行符。

提示：限制每行的语句数量，可以帮助准确定位异常。即使做不好每行一个语句，也要能保证这一行出NullPointException时定位到那个表达式。

### 4.4 每行字符数限制：100

常规语句不要超过这个限制，可以提前换行。超长的语句会影响阅读，必须横向移动滚动条。

提示：当分屏对照、diff模式时，就能体现出优势来了。

例外：

* 按照行长度限制，无法实现地方（例如：javadoc中超长的URL地址， 或者一个超长的JSNI方法的引用）；
* package和import语句不受长度限制。（见3.2、3.3节）；
* 注释中的命令行指令行，将被直接复制到shell中执行的。

### 4.5 断行

术语注意：当原本占用一行的代码分为多行时，称为断行。

没有确定性的公式来指导在每种情况下进行换行。 通常有几种有效的方法来断行。

注意：虽然换行的典型原因是为了避免超过列数限制，但即使是实际符合列限制的代码也可能由作者自行决定是否换行。

提示：提取方法或局部变量可以解决问题，而无需断行。

#### 4.5.1 在何处断行


当在非赋值运算符处断行时，换行要出现在符号之前。 （请注意，这与其他语言（例如C ++和JavaScript）在Google风格中使用的做法不同。）
这也适用于以下“类似运算符”的符号：

- 点分隔符（.）
- 方法引用的两个冒号（::)
- 类型绑定中的&符号（<T extends Foo & Bar>）
- catch块中的管道（catch（FooException | BarException e））

当在赋值运算符处断行时，断点通常出现在符号之后，但任何一种方式都是可接受的。
这也适用于增强型for（“foreach”）语句中的“赋值操作符”冒号。

- 方法或构造函数名称保持附加到其后面的左括号（（））。
- 逗号（，）保持附加到其前面的标记。
- 在lambda中箭头旁边的线条永远不会被破坏，除非如果lambda的主体由单个无支撑表达式组成，则箭头后面可能会出现断点。

例子：

    MyLambda<String, Long, Object> lambda =
        (String label, Long value, Object obj) -> {
            ...
        };

    Predicate<String> predicate = str ->
        longExpressionInvolving(str);
        
提示：断行是为了保持代码清晰。

#### 4.5.2 断行的缩进：至少4个字符

当断行之后，在第一行之后的行，我们叫做延续行。每一个延续行在第一行的基础上至少缩进四个字符。
当原行之后有多个延续行的情况，缩进可以大于4个字符。如果多个延续行之间由同样的语法元素断行，它们可以采用相同的缩进。
 
4.6.3节介绍水平对齐中，解决了使用多个空格与之前行缩进对齐的问题。

例子：

    public static final String NL = "\n";
    StringBuilder sb = new StringBuilder()
            .append("Hello").append(NL)
            .append("World")
            ;

### 4.6 空白

#### 4.6.1 Vertical Whitespace

A single blank line always appears:

Between consecutive members or initializers of a class: fields, constructors, methods, nested classes, static initializers, and instance initializers.
Exception: A blank line between two consecutive fields (having no other code between them) is optional. Such blank lines are used as needed to create logical groupings of fields.
Exception: Blank lines between enum constants are covered in Section 4.8.1.
As required by other sections of this document (such as Section 3, Source file structure, and Section 3.3, Import statements).
A single blank line may also appear anywhere it improves readability, for example between statements to organize the code into logical subsections. A blank line before the first member or initializer, or after the last member or initializer of the class, is neither encouraged nor discouraged.

Multiple consecutive blank lines are permitted, but never required (or encouraged).

以下情况需要使用一个空行：

* 成员之间：fields, constructors, methods, nested classes, static initializers, and instance initializers.
* 例外：两个连续字段之间的空行是可选的，用于字段的空行主要用来对字段进行逻辑分组。
* 在method内，语句的逻辑分组间使用空行。
* class内的第一个成员前或最后一个成员后的空行是可选的(既不鼓励也不反对这样做，视个人喜好而定)。
* 要满足本文档中其他节的空行要求(比如3.3节：import语句)

多空几行没有问题，但也没有必要这样做。

#### 4.6.2 水平空白

Beyond where required by the language or other style rules, and apart from literals, comments and Javadoc, a single ASCII space also appears in the following places only.

1. 关键字和后续左括号，if, for, catch
2. 关键字和其前面的右大括号，如 else, catch
3. 在任何左大括号前({)，两个例外：
    * `@SomeAnnotation({a, b}) (no space is used)`
    * `String[][] x = {{"foo"}}; `
4. 在任何二元或三元运算符的两侧。这也适用于以下“类运算符”符号：
    * the ampersand in a conjunctive type bound: <T extends Foo & Bar>
    * the pipe for a catch block that handles multiple exceptions: catch (FooException | BarException e)  
    * the colon (:) in an enhanced for ("foreach") statement
    * the arrow in a lambda expression: (String str) -> str.length()
    * but not
    * the two colons (::) of a method reference, which is written like Object::toString
    * the dot separator (.), which is written like object.toString()
5. 在`,:;`之后或表示cast的右括号之后
6. 如果在一条语句后做注释，则双斜杠(//)两边都要空格。这里可以允许多个空格，但没有必要。
7. Between the type and variable of a declaration: List<String> list
8. Optional just inside both braces of an array initializer
    * new int[] {5, 6} and new int[] { 5, 6 } are both valid
9. Between a type annotation and [] or ....

This rule is never interpreted as requiring or forbidding additional space at the start or end of a line; it addresses only interior space.

4.6.3 垂直对齐：不要求

术语说明：垂直对齐指的是通过增加可变数量的空格来使某一行的字符与上一行的相应字符对齐。

这是允许的(而且在不少地方可以看到这样的代码)，但Google编程风格对此不做要求。即使对于已经使用水平对齐的代码，我们也不需要去保持这种风格。

看这个对照例子：

    private int x; // this is fine
    private Color color; // this too
    
    private int   x;      // permitted, but future edits
    private Color color;  // may leave it unaligned

Tip：对齐可增加代码可读性，但它为日后的维护带来问题。考虑未来某个时候，我们需要修改一堆对齐的代码中的一行。 这可能导致原本很漂亮的对齐代码变得错位。很可能它会提示你调整周围代码的空白来使这一堆代码重新水平对齐(比如程序员想保持这种水平对齐的风格)， 这就会让你做许多的无用功，增加了reviewer的工作并且可能导致更多的合并冲突。

### 4.7 小括号分组：推荐

推荐，运算符的优先级只有编译器自己门清，推荐增加额外的小括号来减少阅读难度。

特别是逻辑运算符的左边和右边

除非作者和reviewer都认为去掉小括号也不会使代码被误解，或是去掉小括号能让代码更易于阅读，否则我们不应该去掉小括号。 我们没有理由假设读者能记住整个Java运算符优先级表。

### 4.8 Specific constructs

#### 4.8.1 Enum classes

枚举常量间用逗号隔开，换行可选。
看这个例子：

    private enum Answer {
      YES {
        @Override public String toString() {
          return "yes";
        }
      },
    
      NO,
      MAYBE
    }

    public enum AccountType {
      /**用户故事卡*/
      Story,
      /**特性卡*/
      Feature
    }


没有方法和文档的枚举类可写成数组初始化的格式：

    private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }

由于枚举类也是一个类，因此所有适用于其它类的格式规则也适用于枚举类。

#### 4.8.2 变量声明

##### 4.8.2.1 每次只声明一个变量

不要使用组合声明，比如int a, b;。

例外：for循环的head里面可以声明多个变量

例外：成组出现的变量，同时使用同时回收

##### 4.8.2.2 需要时声明

需要的时候声明，就近声明，就近使用。

有些书习惯性的教人们在语句块一开始的时候声明，

好的做法是在第一次使用的地方最近的地方声明，以减小它们的使用范围。
局部变量应该在声明的时候就进行初始化。如果不能在声明时初始化，也应该尽快完成初始化。


#### 4.8.3 数组

##### 4.8.3.1 数组初始化：可以类似块代码处理

所有数组的初始化，都可以采用和块代码相同的格式处理。例如以下格式都是允许的：

    new int[] {           new int[] {
      0, 1, 2, 3            0,
    }                       1,
                            2,
    new int[] {             3,
      0, 1,               }
      2, 3
    }                     new int[]
                              {0, 1, 2, 3}
##### 4.8.3.2 不能像C风格一样声明数组

方括号应该是变量类型的一部分，使用：`String[] args`, 不用:`String args[]`

#### 4.8.4 switch语句

术语说明：switch语句是指在switch花括号中，包含了一组或多组语句块。每组语句块都由一个或多个switch标签（例如case FOO：或者 default：）打头。

##### 4.8.4.1 缩进

和其他语句块一样，switch花括号之后缩进两个字符。
每个switch标签之后，后面紧接的非标签的新行，按照花括号相同的处理方式缩进两个字符。在标签结束后，恢复到之前的缩进，类似花括号结束。

##### 4.8.4.2 Fall-through: commented

在switch语句中，每个标签对应的代码执行完后，都应该通过语句结束（例如：break、continue、return 或抛出异常），否则应该通过注释说明，代码需要继续向下执行下一个标签的代码。注释说明文字只要能说明代码需要继续往下执行都可以（通常是 //fall through）。这个注释在最后一个标签之后不需要注释。例如：

    switch (input) {
      case 1:
      case 2:
        prepareOneOrTwo();
        // fall through
      case 3:
        handleOneTwoOrThree();
        break;
      default:
        handleLargeNumber(input);
    }

Notice that no comment is needed after case 1:, only at the end of the statement group.

##### 4.8.4.3 default标签
 
每个switch语句中，都需要显式声明default标签。即使没有任何代码也需要显示声明。

switch enum 时不需要添加default语句，因为IDE和其他的静态分析工具会检查出这种情况，而提醒你。 如果已经写了default语句，之后enum又发生了增补反而检查不出来

#### 4.8.5 注解

注解紧跟在文档块后面，应用于类、方法和构造函数，一个注解独占一行。这些换行不属于自动换行(第4.5节，自动换行)，因此缩进级别不变。例如：

    @Override
    @Nullable
    public String getNameIfPresent() { ... }

例外：单个的注解可以和签名的第一行出现在同一行。例如：

    @Override public int hashCode() { ... }

应用于字段的注解紧随文档块出现，应用于字段的多个注解允许与字段出现在同一行。例如：

    @Partial @Mock DataLoader loader;

参数和局部变量注解没有特定规则。

#### 4.8.6 注释

这里说的仅指注释，Javadoc放在第7章。

Any line break may be preceded by arbitrary whitespace followed by an implementation comment. Such a comment renders the line non-blank.

##### 4.8.6.1 块注释风格

块注释与其周围的代码在同一缩进级别。它们可以是/* ... */风格，也可以是// ...风格。对于多行的/* ... */注释，后续行必须从*开始， 并且与前一行的*对齐。以下示例注释都是OK的。

    /*
     * This is          // And so           /* Or you can
     * okay.            // is this.          * even do this. */
     */

注释里，不要用多余的装饰符号

    /*==============
     *= i'm a box  =
     *==============
     */

提示：在写多行的注释时，如果你不介意换行的位置，使用`/**/`风格，如果你介意换行的位置使用`//`风格，这个是因为有IDE会自动格式化`/**/`中的内容，或者设置你的IDE不要格式化/**/中的内容

#### 4.8.7 Modifiers

按照规范建议的顺序来

    public protected private abstract default static final transient volatile synchronized native strictfp

#### 4.8.8 数字字面量

一定要使用大写的L来表示长整型，不能使用小写的l来表示长整型，小心眼花看成数字1，例如:

    long a = 1L;//没问题
    long a = 1l;//不行，看着像11


## 5 命名

### 5.1 通用规则

Java规范对标识符的限制是相当宽泛的。常规来说，先考虑英文字母能否命名，其次在考虑使用unicode字符，不要用拼音的声母拼接变量名。

不用添加特别的前缀和后缀： name_, mName, s_name and kName.

我们建议建立一个公共的词汇表，优先使用共同知晓的词汇起名

- 开放词汇表
- 公司词汇表
- 业务词汇表
- 团队词汇表


### 5.2 分项细则

#### 5.2.1 Package names

包名全小写，只用英文字母，不用下划线分隔单词。

例如： com.helloworld

#### 5.2.2 Class names

并遵照JAVA规范的格式：首字符大写（UpperCamelCase），不能出现首字母小写的情况（兼容性异常）。

##### 生产代码

生产代码的类名严格限制只使用英文字母，并且必须是首字母大写的UpperCamelCase风格。

类名通常是名词或名词短语，接口名称有时可能是形容词或形容词短语。现在还没有特定的规则或行之有效的约定来命名注解类型。

提示： 尽管Java支持 unicode 做类名，但有些下游环境支持的不够好而导致错误。

##### 测试类

测试类允许使用汉字做类名

自动化单元测试类： 以Test结尾，例如 ConverterTest

自动化集成测试： 以IT结尾，例如 HostDaoIT

非单元测试，学习，演示用途: 以Demo结尾，例如 SpringDemo


#### 5.2.3 方法名（Method names）

常规的方法名使用java规范的基础风格，Method names are written in lowerCamelCase.

Method names are typically verbs or verb phrases. For example, sendMessage or stop.

没有合适英文名字的时候可以选择汉语

`src/test`中可以使用任何帮助理解的风格，下划线、汉语


#### 5.2.4 Constant names

常量名命名模式为CONSTANT_CASE，全部字母大写，用下划线分隔单词。那，到底什么算是一个常量？

每个常量都是一个static final field，但不是所有static final field(包括字符串，数字，集合，对象等等)都是常量。在选择使用常量命名规则给变量命名时，你需要明确这个变量是否是常量。例如，如果这个变量的状态可以发生改变，那么这个变量几乎可以肯定不是常量。只是计划不会发生改变的变量不足以成为一个常量。下面是常量和非常量的例子：

    // Constants
    static final int NUMBER = 5;
    static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
    static final ImmutableMap<String, Integer> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
    static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
    static final SomeMutableType[] EMPTY_ARRAY = {};
    enum SomeEnum { ENUM_CONSTANT }

    // Not constants
    static String nonFinal = "non-final";
    final String nonStatic = "non-static";
    static final Set<String> mutableCollection = new HashSet<String>();
    static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
    static final ImmutableMap<String, SomeMutableType> mutableValues =
        ImmutableMap.of("Ed", mutableInstance, "Ann", mutableInstance2);
    static final Logger logger = Logger.getLogger(MyClass.getName());
    static final String[] nonEmptyArray = {"these", "can", "change"};
    These names are typically nouns or noun phrases.

#### 5.2.5 非常量的field名



非常量的成员变量命名（包括静态变量和非静态变量），采用lowerCamelCase命名。

一般使用名词或名词短语，例子：computedValues or index.

#### 5.2.6 Parameter names

参数命名采用lowerCamelCase命名。
应该避免使用一个字符作为参数的命名方式。

#### 5.2.7 Local variable names

局部变量采用lowerCamelCase命名。它相对于其他类型的命名，可以采用更简短宽松的方式。
但即使如此，也应该尽量避免采用单个字母进行命名的情况，除了在循环体内使用的临时变量。
 
即使局部变量是final、不可改变的，它也不能被认为是常量，也不应该采用常量的命名方式去命名。

#### 5.2.8 Type variable names

类型名有两种命名方式：
 
1. 单独一个大写字母，有时后面再跟一个数字。（例如，E、T、X、T2）。
2. 像一般的class命名一样（见5.2.2节），再在最后接一个大写字母。（例如，RequestT、FooBarT）。

### 5.3 Camel case规范

有时一些短语被写成Camel case的时候可以有多种写法。例如一些缩写词汇，或者一些组合词：IPv6 或者 iOS 等。
为了统一写法，Google style给出了一种几乎可以确定为一种的写法。
 
1. 将字符全部转换为ASCII字符，并且去掉 ' 等符号。例如，"Müller's algorithm" 被转换为 "Muellers algorithm" 。
2. 将上一步转换的结果拆分成一个一个的词语。从空格处和从其他剩下的标点符号处划分。
    注意：一些已经是Camel case的词语，也应该在这个时候被拆分。（例如 AdWords 被拆分为 ad words）。但是例如iOS之类的词语，它其实不是一个Camel case的词语，而是人们惯例使用的一个词语，因此不用做拆分。
3. 经过上面两部后，先将所有的字母转换为小写，再把每个词语的第一个字母转换为大写。
4. 最后，将所有词语连在一起，形成一个标示符

注意：词语原来的大小写规则，应该被完全忽略。以下是一些例子：

| Prose form |  Correct | Incorrect |
|------------|----------|----------|
|  "XML HTTP request" | XmlHttpRequest | XMLHTTPRequest |
|  "new customer ID" | newCustomerId | newCustomerID
|  "inner stopwatch" | innerStopwatch |  innerStopWatch |
|  "supports IPv6 on iOS?" | supportsIpv6OnIos | supportsIPv6OnIOS |
|  "YouTube importer" | YouTubeImporter
|    |YoutubeImporter*  

*Acceptable, but not recommended.

注意，有些词语在英文中，可以用 - 连接使用，也可以不使用 - 直接使用。例如 “nonempty”和 “non-empty”都是可以的。因此，方法名字为checkNonempty 或者checkNonEmpty 都是可以的。

## 6 编程实践

### 6.1 @override 都应该使用

覆盖父类的方法要写 @Override，预防拼写错误

Exception: @Override may be omitted when the parent method is @Deprecated.

### 6.2 Caught exceptions: not ignored

空的catch块要给出说明，（常规的流程是log，throw）。

下面这个例子，catch 是流程上的一部分，并且给出了说明。

    try {
      int i = Integer.parseInt(response);
      return handleNumericResponse(i);
    } catch (NumberFormatException ok) {
      // it's not numeric; that's fine, just continue
    }
    ...
    return handleTextResponse(response);

Exception: 测试用途类可以不写

    try {
      emptyStack.pop();
      fail();
    } catch (NoSuchElementException expected) {
    }
    
### 6.3 静态成员的访问：应该通过类，而不是对象

当一个静态成员被访问时，应该通过class名去访问，而不应该使用这个class的具体实例对象。例如：

    Foo aFoo = ...;
    Foo.aStaticMethod(); // good
    aFoo.aStaticMethod(); // bad
    somethingThatYieldsAFoo().aStaticMethod(); // very bad

### 6.4 不使用Finalizers 方法

别用 Object.finalize，也没有使用的必要。

注意：不应该使用这以方法。如果你认为你必须使用，请先仔细阅读并理解 Effective Java 第七条 “Avoid Finalizers”。然后不要使用它。


## 7 Javadoc
### 7.1 格式
#### 7.1.1 常规格式

最基本的Javadoc风格模板：

    /**
     * 摘要
     * <p>
     * 详情
     * 
     */
    public int method(String p1) { ... }
    
或者单行：

    /** An especially short bit of Javadoc. */
    
不能使用`//`来当做文档，开发工具不会解析这种注释
    
    public static final String MAGIC_NUMBER; //XX功能开关

#### 7.1.2 段落

空白行：是指javadoc中，上下两个段落之间只有上下对齐的 * 字符的行。每个段落的第一行在第一个字符之前，有一个<p>标签，并且之后不要有任何空格。

摘要和详情段落之间要空开一行（这个和git提交备注风格一致）

    /**
     * 第一行摘要
     * 
     * <p>段落
     * ……
     */

#### 7.1.3 Javadoc标记

标准的Javadoc标记按以下顺序出现：@param, @return, @throws, @deprecated, 前面这4种标记如果出现，描述都不能为空。 当描述无法在一行中容纳，连续行需要至少再缩进4个空格。

### 7.2 摘要段

每个类或成员的Javadoc以一个简短的摘要片段开始。这个片段是非常重要的，在某些情况下，它是唯一出现的文本，比如在类和方法索引中。

这只是一个小片段，可以是一个名词短语或动词短语，但不是一个完整的句子。它不会以A {@code Foo} is a...或This method returns...开头, 它也不会是一个完整的祈使句，如Save the record...。然而，由于开头大写及被加了标点，它看起来就像是个完整的句子。

### 7.3 应用场景

任何有必要说明意图的地方，必须使用Javadoc格式来记录。

举例：

JPA实体字段，注释在 field上，必须使用，例子：

    @Entity
    @Table
    class Account{
       /** 主键*/  
        @Id
        String id;
        /** 密码 */
        @Column
        String woshimima
    }
 
算法模块的类名，用于阐述设计

    /**
     * Interface to be implemented by beans that need to react once all their
     * properties have been set by a BeanFactory: for example, to perform custom
     * initialization, or merely to check that all mandatory properties have been set.
     *
     * <p>An alternative to implementing InitializingBean is specifying a custom
     * init-method, for example in an XML bean definition.
     * For a list of all bean lifecycle methods, see the BeanFactory javadocs.
     */
    public interface InitializingBean {
    
      /**
       * Invoked by a BeanFactory after it has set all bean properties supplied
       * (and satisfied BeanFactoryAware and ApplicationContextAware).
       * <p>This method allows the bean instance to perform initialization only
       * possible when all bean properties have been set and to throw an
       * exception in the event of misconfiguration.
       * @throws Exception in the event of misconfiguration (such
       * as failure to set an essential property) or if initialization fails.
       */
      void afterPropertiesSet() throws Exception;
    
    }

有特别调用顺序的方法之间

    /**
     * 连接总线的工具
     * 
     * <p>
     * 设置完成后需要调用 #init() 方法
     * example:
     * <pre>
     * Worker conn = new Worker(new File("config.properties"));
     * conn.init();
     * </pre>
     *
     *
     */
    class Wokrer{
    
        public Worker(File config);
    
        public void init()
    }


简言之，任何需要`公开`使用的class、method、field 都要有文档支持。


#### 7.3.1 例外：自描述的methods

方法在起名的时候就足够说明意图，并且入参如何传也能十分确定，这类方法地方再去写Javadoc就多余了

单纯的getter setter可以考虑把javadoc写在在field上面，多数开发工具可以支持。

Tip：如果有一些相关信息是需要读者了解的，那么以上的例外不应作为忽视这些信息的理由。例如，对于方法名getCanonicalName， 就不应该忽视文档说明，因为读者很可能不知道词语canonical name指的是什么。

#### 7.3.2 例外: overrides

如果一个方法重写了父类中的方法，那么Javadoc就要看情况来决定是否提供。

- interface 和 class 一对一实现，如果interface上面已经写了Javadoc，详细说明实现意图，子类不用写
- callback，handler 等等回调形式的子类，需要用Javadoc写明设计意图，例如（Runnable的子类，要么在class上写Javadoc，要么在run()上写明Javadoc）
- 对父类的Decorator,Proxy 子类 override 父类方法时要写明Javadoc 

#### 7.3.4 其他（Non-required Javadoc）

Other classes and members have Javadoc as needed or desired.

Whenever an implementation comment would be used to define the overall purpose or behavior of a class or member, that comment is written as Javadoc instead (using /**).

Non-required Javadoc is not strictly required to follow the formatting rules of Sections 7.1.2, 7.1.3, and 7.2, though it is of course recommended.
