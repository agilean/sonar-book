# JAVA 
<!-- toc -->

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

成员的顺序没有严格的限制，我们也不认可按照 fields,method 这种简单的方法排序是好的。

The order you choose for the members and initializers of your class can have a great effect on learnability. However, there's no single correct recipe for how to do it; different classes may order their contents in different ways.

What is important is that each class uses some logical order, which its maintainer could explain if asked. For example, new methods are not just habitually added to the end of the class, as that would yield "chronological by date added" ordering, which is not a logical ordering.

##### 3.4.2.1 Overloads: never split

When a class has multiple constructors, or multiple methods with the same name, these appear sequentially, with no other code in between (not even private members).

## 4 格式化

Terminology Note: block-like construct refers to the body of a class, method or constructor. Note that, by Section 4.8.3.1 on array initializers, any array initializer may optionally be treated as if it were a block-like construct.

宗旨：自然美，阅读方便，兼容性好

### 4.1 大括号

#### 4.1.1 Braces are used where optional

Braces are used with if, else, for, do and while statements, even when the body is empty or contains only a single statement.

#### 4.1.2 Nonempty blocks: K & R style
Braces follow the Kernighan and Ritchie style ("Egyptian brackets") for nonempty blocks and block-like constructs:

No line break before the opening brace.
Line break after the opening brace.
Line break before the closing brace.
Line break after the closing brace, only if that brace terminates a statement or terminates the body of a method, constructor, or named class. For example, there is no line break after the brace if it is followed by else or a comma.
Examples:

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

#### 4.1.3 Empty blocks: may be concise

An empty block or block-like construct may be in K & R style (as described in Section 4.1.2). Alternatively, it may be closed immediately after it is opened, with no characters or line break in between ({}), unless it is part of a multi-block statement (one that directly contains multiple blocks: if/else or try/catch/finally).

Examples:

      // This is acceptable
      void doNothing() {}
    
      // This is equally acceptable
      void doNothingElse() {
      }
      // This is not acceptable: No concise empty blocks in a multi-block statement
      try {
        doSomething();
      } catch (Exception e) {}
      
### 4.2 Block indentation: +2 spaces

Each time a new block or block-like construct is opened, the indent increases by two spaces. When the block ends, the indent returns to the previous indent level. The indent level applies to both code and comments throughout the block. (See the example in Section 4.1.2, Nonempty blocks: K & R Style.)

### 4.3 每行一个表达式

Each statement is followed by a line break.

限制每行的语句数量，可以帮助准确定位异常。

### 4.4 每行字符数限制：100

常规语句不要超过这个限制，可以提前换行。超长的语句会影响阅读，还的移动滚动条。

提示：当分屏对照、diff模式时，就能体现出优势来了。

例外：

* Lines where obeying the column limit is not possible (for example, a long URL in Javadoc, or a long JSNI method reference).
* package and import statements (see Sections 3.2 Package statement and 3.3 Import statements).
* Command lines in a comment that may be cut-and-pasted into a shell.

### 4.5 Line-wrapping

Terminology Note: When code that might otherwise legally occupy a single line is divided into multiple lines, this activity is called line-wrapping.

There is no comprehensive, deterministic formula showing exactly how to line-wrap in every situation. Very often there are several valid ways to line-wrap the same piece of code.

Note: While the typical reason for line-wrapping is to avoid overflowing the column limit, even code that would in fact fit within the column limit may be line-wrapped at the author's discretion.

Tip: Extracting a method or local variable may solve the problem without the need to line-wrap.

#### 4.5.1 Where to break

The prime directive of line-wrapping is: prefer to break at a higher syntactic level. Also:

When a line is broken at a non-assignment operator the break comes before the symbol. (Note that this is not the same practice used in Google style for other languages, such as C++ and JavaScript.)
This also applies to the following "operator-like" symbols:
the dot separator (.)
the two colons of a method reference (::)
an ampersand in a type bound (<T extends Foo & Bar>)
a pipe in a catch block (catch (FooException | BarException e)).
When a line is broken at an assignment operator the break typically comes after the symbol, but either way is acceptable.
This also applies to the "assignment-operator-like" colon in an enhanced for ("foreach") statement.
A method or constructor name stays attached to the open parenthesis (() that follows it.
A comma (,) stays attached to the token that precedes it.
A line is never broken adjacent to the arrow in a lambda, except that a break may come immediately after the arrow if the body of the lambda consists of a single unbraced expression. Examples:

    MyLambda<String, Long, Object> lambda =
        (String label, Long value, Object obj) -> {
            ...
        };

    Predicate<String> predicate = str ->
        longExpressionInvolving(str);
Note: The primary goal for line wrapping is to have clear code, not necessarily code that fits in the smallest number of lines.

#### 4.5.2 Indent continuation lines at least +4 spaces
When line-wrapping, each line after the first (each continuation line) is indented at least +4 from the original line.

When there are multiple continuation lines, indentation may be varied beyond +4 as desired. In general, two continuation lines use the same indentation level if and only if they begin with syntactically parallel elements.

Section 4.6.3 on Horizontal alignment addresses the discouraged practice of using a variable number of spaces to align certain tokens with previous lines.

### 4.6 Whitespace

#### 4.6.1 Vertical Whitespace
A single blank line always appears:

Between consecutive members or initializers of a class: fields, constructors, methods, nested classes, static initializers, and instance initializers.
Exception: A blank line between two consecutive fields (having no other code between them) is optional. Such blank lines are used as needed to create logical groupings of fields.
Exception: Blank lines between enum constants are covered in Section 4.8.1.
As required by other sections of this document (such as Section 3, Source file structure, and Section 3.3, Import statements).
A single blank line may also appear anywhere it improves readability, for example between statements to organize the code into logical subsections. A blank line before the first member or initializer, or after the last member or initializer of the class, is neither encouraged nor discouraged.

Multiple consecutive blank lines are permitted, but never required (or encouraged).

#### 4.6.2 Horizontal whitespace

Beyond where required by the language or other style rules, and apart from literals, comments and Javadoc, a single ASCII space also appears in the following places only.

1. Separating any reserved word, such as if, for or catch, from an open parenthesis (() that follows it on that line
2. Separating any reserved word, such as else or catch, from a closing curly brace (}) that precedes it on that line
3. Before any open curly brace ({), with two exceptions:
    * `@SomeAnnotation({a, b}) (no space is used)`
    * `String[][] x = {{"foo"}}; `
4. On both sides of any binary or ternary operator. This also applies to the following "operator-like" symbols:
    * the ampersand in a conjunctive type bound: <T extends Foo & Bar>
    * the pipe for a catch block that handles multiple exceptions: catch (FooException | BarException e)  
    * the colon (:) in an enhanced for ("foreach") statement
    * the arrow in a lambda expression: (String str) -> str.length()
    * but not
    * the two colons (::) of a method reference, which is written like Object::toString
    * the dot separator (.), which is written like object.toString()
5. After ,:; or the closing parenthesis ()) of a cast
6. On both sides of the double slash (//) that begins an end-of-line comment. Here, multiple spaces are allowed, but not required.
7. Between the type and variable of a declaration: List<String> list
8. Optional just inside both braces of an array initializer
    * new int[] {5, 6} and new int[] { 5, 6 } are both valid
9. Between a type annotation and [] or ....

This rule is never interpreted as requiring or forbidding additional space at the start or end of a line; it addresses only interior space.

4.6.3 垂直方向对齐：不要求

不需要做到垂直对齐，垂直对齐在个别场合会起到帮助阅读的作用，为了垂直对齐需要付出额外的人工成本。

Terminology Note: Horizontal alignment is the practice of adding a variable number of additional spaces in your code with the goal of making certain tokens appear directly below certain other tokens on previous lines.

This practice is permitted, but is never required by Google Style. It is not even required to maintain horizontal alignment in places where it was already used.

Here is an example without alignment, then using alignment:

    private int x; // this is fine
    private Color color; // this too
    
    private int   x;      // permitted, but future edits
    private Color color;  // may leave it unaligned

Tip: Alignment can aid readability, but it creates problems for future maintenance. Consider a future change that needs to touch just one line. This change may leave the formerly-pleasing formatting mangled, and that is allowed. More often it prompts the coder (perhaps you) to adjust whitespace on nearby lines as well, possibly triggering a cascading series of reformattings. That one-line change now has a "blast radius." This can at worst result in pointless busywork, but at best it still corrupts version history information, slows down reviewers and exacerbates merge conflicts.

### 4.7 小括号分组：推荐

推荐，运算符的优先级只有编译器自己门清，推荐增加额外的小括号来减少阅读难度。

特别是逻辑运算符的左边和右边

Optional grouping parentheses are omitted only when author and reviewer agree that there is no reasonable chance the code will be misinterpreted without them, nor would they have made the code easier to read. It is not reasonable to assume that every reader has the entire Java operator precedence table memorized.

### 4.8 Specific constructs

#### 4.8.1 Enum classes

After each comma that follows an enum constant, a line break is optional. Additional blank lines (usually just one) are also allowed. This is one possibility:

    private enum Answer {
      YES {
        @Override public String toString() {
          return "yes";
        }
      },
    
      NO,
      MAYBE
    }

An enum class with no methods and no documentation on its constants may optionally be formatted as if it were an array initializer (see Section 4.8.3.1 on array initializers).

private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
Since enum classes are classes, all other rules for formatting classes apply.

#### 4.8.2 Variable declarations

##### 4.8.2.1 One variable per declaration
Every variable declaration (field or local) declares only one variable: declarations such as int a, b; are not used.

Exception: Multiple variable declarations are acceptable in the header of a for loop.

##### 4.8.2.2 Declared when needed

需要的时候声明，就近声明，就近使用。

Local variables are not habitually declared at the start of their containing block or block-like construct. Instead, local variables are declared close to the point they are first used (within reason), to minimize their scope. Local variable declarations typically have initializers, or are initialized immediately after declaration.

#### 4.8.3 Arrays

##### 4.8.3.1 Array initializers: can be "block-like"

Any array initializer may optionally be formatted as if it were a "block-like construct." For example, the following are all valid (not an exhaustive list):

    new int[] {           new int[] {
      0, 1, 2, 3            0,
    }                       1,
                            2,
    new int[] {             3,
      0, 1,               }
      2, 3
    }                     new int[]
                              {0, 1, 2, 3}
##### 4.8.3.2 No C-style array declarations

使用：`String[] args`, 不用:`String args[]`


#### 4.8.4 Switch statements

Terminology Note: Inside the braces of a switch block are one or more statement groups. Each statement group consists of one or more switch labels (either case FOO: or default:), followed by one or more statements (or, for the last statement group, zero or more statements).

##### 4.8.4.1 Indentation
As with any other block, the contents of a switch block are indented +2.

After a switch label, there is a line break, and the indentation level is increased +2, exactly as if a block were being opened. The following switch label returns to the previous indentation level, as if a block had been closed.

##### 4.8.4.2 Fall-through: commented
Within a switch block, each statement group either terminates abruptly (with a break, continue, return or thrown exception), or is marked with a comment to indicate that execution will or might continue into the next statement group. Any comment that communicates the idea of fall-through is sufficient (typically // fall through). This special comment is not required in the last statement group of the switch block. Example:

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

##### 4.8.4.3 The default case is present
Each switch statement includes a default statement group, even if it contains no code.

Exception: A switch statement for an enum type may omit the default statement group, if it includes explicit cases covering all possible values of that type. This enables IDEs or other static analysis tools to issue a warning if any cases were missed.

#### 4.8.5 Annotations
Annotations applying to a class, method or constructor appear immediately after the documentation block, and each annotation is listed on a line of its own (that is, one annotation per line). These line breaks do not constitute line-wrapping (Section 4.5, Line-wrapping), so the indentation level is not increased. Example:

    @Override
    @Nullable
    public String getNameIfPresent() { ... }
Exception: A single parameterless annotation may instead appear together with the first line of the signature, for example:

    @Override public int hashCode() { ... }
Annotations applying to a field also appear immediately after the documentation block, but in this case, multiple annotations (possibly parameterized) may be listed on the same line; for example:

    @Partial @Mock DataLoader loader;
There are no specific rules for formatting annotations on parameters, local variables, or types.

#### 4.8.6 Comments
This section addresses implementation comments. Javadoc is addressed separately in Section 7, Javadoc.

Any line break may be preceded by arbitrary whitespace followed by an implementation comment. Such a comment renders the line non-blank.

##### 4.8.6.1 Block comment style
Block comments are indented at the same level as the surrounding code. They may be in /* ... */ style or // ... style. For multi-line /* ... */ comments, subsequent lines must start with * aligned with the * on the previous line.

    /*
     * This is          // And so           /* Or you can
     * okay.            // is this.          * even do this. */
     */
Comments are not enclosed in boxes drawn with asterisks or other characters.

Tip: When writing multi-line comments, use the `/* ... */` style if you want automatic code formatters to re-wrap the lines when necessary (paragraph-style). Most formatters don't re-wrap lines in // ... style comment blocks.

#### 4.8.7 Modifiers
Class and member modifiers, when present, appear in the order recommended by the Java Language Specification:

public protected private abstract default static final transient volatile synchronized native strictfp
#### 4.8.8 Numeric Literals
long-valued integer literals use an uppercase L suffix, never lowercase (to avoid confusion with the digit 1). For example, 3000000000L rather than 3000000000l.


## 5 命名

### 5.1 Rules common to all identifiers

Identifiers use only ASCII letters and digits, and, in a small number of cases noted below, underscores. Thus each valid identifier name is matched by the regular expression \w+ .

In Google Style, special prefixes or suffixes are not used. For example, these names are not Google Style: name_, mName, s_name and kName.

### 5.2 Rules by identifier type

#### Package names

包名全小写，只用英文字母。

例如： com.helloworld

#### Class names

并遵照JAVA规范的格式：首字符大写（UpperCamelCase），不能出现首字母小写的情况（兼容性异常）。


##### 生产代码

生产代码的类名首选使用英文字母，

提示： 尽管Java支持 unicode 做类名，但有些下游环境支持的不够好而导致错误。

Class names are typically nouns or noun phrases. For example, Character or ImmutableList. Interface names may also be nouns or noun phrases (for example, List), but may sometimes be adjectives or adjective phrases instead (for example, Readable).

There are no specific rules or even well-established conventions for naming annotation types.

##### 测试类

测试类允许使用汉字做类名

自动化单元测试类： 以Test结尾，例如 ConverterTest

自动化集成测试： 以IT结尾，例如 HostDaoIT

非单元测试，学习，演示用途: 以Demo结尾，例如 SpringDemo


### 方法名（Method names）

常规的方法名使用java规范的基础风格，Method names are written in lowerCamelCase.

Method names are typically verbs or verb phrases. For example, sendMessage or stop.

没有合适英文名字的时候可以选择汉语

src/test中可以使用任何帮助理解的风格，下划线、汉语


#### Constant names

Constant names use CONSTANT_CASE: all uppercase letters, with each word separated from the next by a single underscore. But what is a constant, exactly?

Constants are static final fields whose contents are deeply immutable and whose methods have no detectable side effects. This includes primitives, Strings, immutable types, and immutable collections of immutable types. If any of the instance's observable state can change, it is not a constant. Merely intending to never mutate the object is not enough. Examples:

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

#### Non-constant field names

Non-constant field names (static or otherwise) are written in lowerCamelCase.

These names are typically nouns or noun phrases. For example, computedValues or index.

#### Parameter names

Parameter names are written in lowerCamelCase.

One-character parameter names in public methods should be avoided.

#### Local variable names

Local variable names are written in lowerCamelCase.

Even when final and immutable, local variables are not considered to be constants, and should not be styled as constants.

#### Type variable names

Each type variable is named in one of two styles:

A single capital letter, optionally followed by a single numeral (such as E, T, X, T2)
A name in the form used for classes (see Section 5.2.2, Class names), followed by the capital letter T (examples: RequestT, FooBarT).

### 5.3 Camel case: defined

Sometimes there is more than one reasonable way to convert an English phrase into camel case, such as when acronyms or unusual constructs like "IPv6" or "iOS" are present. To improve predictability, Google Style specifies the following (nearly) deterministic scheme.

Beginning with the prose form of the name:

Convert the phrase to plain ASCII and remove any apostrophes. For example, "Müller's algorithm" might become "Muellers algorithm".
Divide this result into words, splitting on spaces and any remaining punctuation (typically hyphens).
Recommended: if any word already has a conventional camel-case appearance in common usage, split this into its constituent parts (e.g., "AdWords" becomes "ad words"). Note that a word such as "iOS" is not really in camel case per se; it defies any convention, so this recommendation does not apply.
Now lowercase everything (including acronyms), then uppercase only the first character of:
... each word, to yield upper camel case, or
... each word except the first, to yield lower camel case
Finally, join all the words into a single identifier.
Note that the casing of the original words is almost entirely disregarded. Examples:

Prose form  Correct Incorrect

    "XML HTTP request"  XmlHttpRequest  XMLHTTPRequest
    "new customer ID" newCustomerId newCustomerID
    "inner stopwatch" innerStopwatch  innerStopWatch
    "supports IPv6 on iOS?" supportsIpv6OnIos supportsIPv6OnIOS
    "YouTube importer"  YouTubeImporter
    YoutubeImporter*  
    *Acceptable, but not recommended.

Note: Some words are ambiguously hyphenated in the English language: for example "nonempty" and "non-empty" are both correct, so the method names checkNonempty and checkNonEmpty are likewise both correct.




## 6 编程实践

### 6.1 @Override: always used

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
    
### 6.3 Static members: qualified using class
When a reference to a static class member must be qualified, it is qualified with that class's name, not with a reference or expression of that class's type.

    Foo aFoo = ...;
    Foo.aStaticMethod(); // good
    aFoo.aStaticMethod(); // bad
    somethingThatYieldsAFoo().aStaticMethod(); // very bad

### 6.4 Finalizers: not used

别用 Object.finalize，也没有使用的必要。

Tip: Don't do it. If you absolutely must, first read and understand Effective Java Item 7, "Avoid Finalizers," very carefully, and then don't do it.

替代方案： 使用Spring 生命周期回调 

    @javax.annotation.PreDestroy
    interface org.springframework.beans.factory.DisposableBean

这两种方式在Spring容器管理的bean中都能达到程序关闭前的回调


## 7 Javadoc
### 7.1 格式
#### 7.1.1 General form

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

#### 7.1.2 Paragraphs

摘要和详情段落之间要空开一行（这个和git提交备注风格一致）

    /**
     * 第一行摘要
     * 
     * <p>段落
     * ……
     */

#### 7.1.3 Block tags

Any of the standard "block tags" that are used appear in the order @param, @return, @throws, @deprecated, and these four types never appear with an empty description. When a block tag doesn't fit on a single line, continuation lines are indented four (or more) spaces from the position of the @.

### 7.2 摘要段

Each Javadoc block begins with a brief summary fragment. This fragment is very important: it is the only part of the text that appears in certain contexts such as class and method indexes.

This is a fragment—a noun phrase or verb phrase, not a complete sentence. It does not begin with A {@code Foo} is a..., or This method returns..., nor does it form a complete imperative sentence like Save the record.. However, the fragment is capitalized and punctuated as if it were a complete sentence.

Tip: A common mistake is to write simple Javadoc in the form /** @return the customer ID */. This is incorrect, and should be changed to /** Returns the customer ID. */.

### 7.3 应用场景

任何有必要说明意图的地方，必须使用Javadoc格式来记录。

举例：

- JPA实体字段，注释在 field上，必须使用 

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
 
- 算法模块的类名

- 有特别调用顺序的方法之间

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


简言之，任何需要公开使用的class、method、field 都要有文档支持。

At the minimum, Javadoc is present for every public class, and every public or protected member of such a class, with a few exceptions noted below.

Additional Javadoc content may also be present, as explained in Section 7.3.4, Non-required Javadoc.

#### 7.3.1 例外：自描述的methods

方法在起名的时候就足够说明意图，这类方法可以不用写多余的注释

单纯的getter setter可以考虑把javadoc写在在field上面，多数开发工具可以支持。

Javadoc is optional for "simple, obvious" methods like getFoo, in cases where there really and truly is nothing else worthwhile to say but "Returns the foo".

Important: it is not appropriate to cite this exception to justify omitting relevant information that a typical reader might need to know. For example, for a method named getCanonicalName, don't omit its documentation (with the rationale that it would say only /** Returns the canonical name. */) if a typical reader may have no idea what the term "canonical name" means!

#### 7.3.2 Exception: overrides

Javadoc is not always present on a method that overrides a supertype method.

#### 7.3.4 Non-required Javadoc

Other classes and members have Javadoc as needed or desired.

Whenever an implementation comment would be used to define the overall purpose or behavior of a class or member, that comment is written as Javadoc instead (using /**).

Non-required Javadoc is not strictly required to follow the formatting rules of Sections 7.1.2, 7.1.3, and 7.2, though it is of course recommended.
