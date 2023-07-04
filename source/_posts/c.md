---
title: c#
date: 2023-05-29 15:39:15
tags: 工程
typora-root-url: c
description: "记录c#的基础知识"
---

## C#基础

### 基础

简单介绍一下c#“简单操作”，基本上学过其他语言的都是通用的，简单看看就行。如果在序号之前看到了*，说明这个知识点比较抽象（或者没啥用），建议多查👋

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace _001_第一个程序
{
    internal class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

这段代码是一个简单的C#程序框架示例，它包含了一些常见的命名空间和一个包含`Main`方法的类。

1. `using`语句：在代码的开头，使用了一系列的`using`语句来引入不同的命名空间。这些命名空间包括`System`、`System.Collections.Generic`、`System.Linq`、`System.Text`和`System.Threading.Tasks`等。通过引入这些命名空间，我们可以在代码中直接使用这些命名空间中定义的类型和成员，而无需使用完全限定名。

   > 命名空间
   >
   > ```c#
   > using System;
   > using System.Collections.Generic;
   > using System.Linq;
   > using System.Text;
   > using System.Threading.Tasks;
   > ```
   >
   > C#中的命名空间（Namespace）用于组织和管理代码，它具有以下作用：
   >
   > 1. 避免命名冲突：命名空间可以防止不同代码元素（类、结构、接口、枚举等）之间的命名冲突。通过将相关的代码元素放置在同一个命名空间中，可以确保它们的名称在该命名空间内是唯一的。
   > 2. 提供代码组织结构：命名空间提供了一种逻辑上组织和划分代码的方式。你可以根据功能、模块或其他逻辑关系将相关的类型放置在同一个命名空间中，以便更好地组织和管理代码。
   > 3. 支持代码重用和模块化：通过使用命名空间，可以将代码分割成多个逻辑模块，使得这些模块可以在不同的项目或文件中进行重用。通过引用适当的命名空间，可以在代码中访问和使用其他命名空间中定义的类型和成员。
   > 4. 提供代码可见性控制：命名空间也可以用于控制类型和成员的可见性。在C#中，命名空间可以被限定为特定的访问修饰符（例如`public`、`internal`等），从而决定其内部的类型和成员是否可以被其他代码访问。
   > 5. 与类库和命名空间的组织结构对应：C#标准类库和第三方类库通常使用命名空间来组织和命名其提供的类型和功能。通过使用相应的命名空间，你可以引用并使用这些类库中的类型和成员。
   >
   > 总之，C#中的命名空间提供了一种组织、管理和访问代码的机制，能够避免命名冲突，提供代码重用和模块化，控制代码可见性，并与类库和模块的组织结构对应。它是代码组织和协作的重要工具。

2. 命名空间：代码中定义了一个名为`_001_第一个程序`的命名空间。命名空间用于组织和管理代码，它可以包含类、结构、接口、枚举等代码元素。在这个示例中，命名空间内部只包含一个类`Program`。

   > ```c#
   > namespace _001_第一个程序
   > {
   >     internal class Program
   >     {
   >         static void Main(string[] args)
   >         {
   >         }
   >     }
   > }
   > ```
   >
   > 在C#中，自定义命名空间时需要遵循以下规则：
   >
   > 1. 命名空间名称必须是有效的标识符：命名空间的名称必须符合C#中有效标识符的规则。它可以包含字母、数字和下划线，并且必须以字母或下划线开头。
   > 2. 命名空间可以使用层次结构：命名空间可以包含多个层次，使用`.`进行分隔。例如，命名空间可以是`MyNamespace`、`MyNamespace.SubNamespace`、`MyNamespace.SubNamespace.SubSubNamespace`等。
   > 3. 命名空间应具有描述性：命名空间的名称应该具有描述性，能够清楚地表达其所包含代码的目的或功能。例如，如果你正在创建一个与日志记录相关的代码，可以选择一个描述性的命名空间名称，如`MyCompany.Logging`。
   > 4. 避免与现有命名空间冲突：在自定义命名空间时，应避免与已有的系统命名空间或其他第三方库的命名空间发生冲突。可以通过选择唯一的命名空间名称、使用公司名或项目名作为前缀等方式来避免冲突。
   > 5. 命名空间与目录结构可以对应：在组织项目文件时，可以将命名空间与文件的物理目录结构相对应。这有助于在代码文件的组织和查找方面更加直观和一致。
   > 6. 命名空间的规范化：根据一般的命名约定，命名空间的首字母应大写，采用帕斯卡命名法（PascalCase）。例如，`MyNamespace`、`MyNamespace.SubNamespace`。
   >
   > 总之，自定义命名空间时需要确保名称符合C#中有效标识符的规则，具有描述性，避免与现有命名空间冲突，可以与文件的目录结构对应，符合命名约定。这些规则有助于编写清晰、可维护的代码，并提供一致性和可读性。

3. 类：在命名空间中定义了一个名为`Program`的类。这是程序的入口点，其中包含一个静态的`Main`方法。`Main`方法是C#程序的起始执行点，它是程序开始运行的地方。在这个示例中，`Main`方法没有任何具体的代码

#### 变量标识符

变量标识符是用来命名变量的名称，它具有以下特点：

1. 标识符必须是有效的标识符：变量标识符必须符合C#中有效标识符的规则。它可以包含字母、数字和下划线，并且必须以字母或下划线开头。标识符区分大小写，因此大小写敏感。

2. 标识符应具有描述性：变量标识符应该具有描述性，能够清晰地表达所代表的变量的含义和用途。良好的标识符命名有助于提高代码的可读性和可维护性。

3. 遵循命名约定：在C#中，通常采用驼峰命名法（camelCase）来命名变量。这意味着标识符的第一个单词以小写字母开头，后续的每个单词的首字母大写。例如，`myVariable`、`firstName`。

   > 驼峰命名法（CamelCase）是一种命名约定，用于命名标识符（如变量、函数、属性、类等）。它的命名规则如下：
   >
   > 1. 第一个单词以小写字母开头：首字母小写，后续单词的首字母大写。
   > 2. 单词之间没有空格或其他分隔符：所有单词连续写在一起，没有空格或其他分隔符。
   >
   > 驼峰命名法分为两种形式：
   >
   > 1. 小驼峰命名法（lowerCamelCase）：第一个单词的首字母小写，后续单词的首字母大写。例如：`myVariable`、`firstName`。
   > 2. 大驼峰命名法（PascalCase）：每个单词的首字母都大写。例如：`MyVariable`、`FirstName`。
   >
   > 驼峰命名法的目的是使标识符具有更好的可读性和一致性，并提供一种清晰的命名约定。它在C#编程中被广泛使用，并且也是许多其他编程语言中常见的命名约定之一。
   >
   > 以下是一些使用驼峰命名法的示例：
   >
   > ```c#
   > csharpCopy codeint myVariable = 10;
   > string firstName = "John";
   > 
   > class MyClass
   > {
   >     int myProperty { get; set; }
   >     void myMethod()
   >     {
   >         // 方法体
   >     }
   > }
   > ```
   >
   > 总之，驼峰命名法是一种命名约定，根据首字母的大小写来区分单词，并且没有空格或其他分隔符。它有助于提高代码的可读性和一致性。

4. 不使用保留字：标识符不能是C#的保留字（关键字），因为保留字具有特殊的意义和用途，用于编程语言本身的语法和功能。

5. 尽量简洁明确：标识符应该尽量简洁明确，避免过长或过于复杂的命名。一个好的标识符能够准确地表达变量的用途，同时也便于代码的编写和阅读。

6. 语义正确：标识符应该与变量的含义和类型相符。它们应该能够清晰地表达变量所代表的数据或对象。

7. 遵循命名约定和项目规范：在特定的项目或团队中，可能会有特定的命名约定或规范。遵循这些规范可以保持代码风格的一致性，并使不同开发者之间更容易理解和协作。

总的来说，变量标识符是用于命名变量的名称，应符合C#中有效标识符的规则，具有描述性，遵循命名约定，不使用保留字，简洁明确，并与变量的含义和类型相符。良好的标识符命名有助于编写易读、易维护的代码。

#### 变量

在 C# 中，变量具有以下规则：

1. 命名规则：

   - 变量名称必须以字母或下划线 `_` 开头。
   - 变量名称可以包含字母、数字和下划线。
   - 变量名称区分大小写。

2. 关键字保留：

   - 不可以使用 C# 中的关键字作为变量名称。例如，`int`、`string`、`if` 等都是关键字，不能用作变量名。

3. 类型声明：

   - 变量必须先声明后使用。在声明变量时，需要指定变量的类型。

4. 类型安全：

   - C# 是一种静态类型语言，变量在声明时需要指定其类型，且变量的类型在编译时就确定了。类型安全意味着变量在使用过程中必须与其声明的类型匹配，不允许类型之间的隐式转换（除非使用显式类型转换）。

     > 当说到C#是一种静态类型语言时，意味着在编译时，每个变量都必须被显式地声明为某种类型。这意味着在声明变量时，必须指定变量的数据类型，例如整数 (`int`)、字符串 (`string`)、布尔值 (`bool`) 等。
     >
     > 一旦变量被声明为某种类型，它的类型就在编译时确定，无法在运行时更改。这为编译器提供了类型检查的机会，以确保变量在使用过程中与其声明的类型匹配。
     >
     > 类型安全的概念是指在使用变量时，变量的类型必须与其声明的类型相匹配，否则将产生编译时错误。这意味着在对变量进行操作、赋值或传递给函数时，需要保证类型的一致性。
     >
     > 举个例子，假设有以下代码片段：
     >
     > ```c#
     > csharpCopy codeint num = 10;
     > string text = "Hello";
     > 
     > int result = num + text; // 编译时错误：不能将 int 类型和 string 类型相加
     > ```
     >
     > 在上面的代码中，`num` 声明为整数类型 (`int`)，`text` 声明为字符串类型 (`string`)。由于整数和字符串是不兼容的类型，试图将它们相加会导致编译时错误。这是因为 C# 是类型安全的，不允许将不同类型的值混合使用。
     >
     > 然而，有时我们确实需要将一个类型的值转换为另一个类型。在这种情况下，可以使用显式类型转换来实现，例如使用 `(type)` 运算符。但是需要注意，显式类型转换可能会导致数据丢失或精度损失，所以需要谨慎使用。
     >
     > 总结起来，类型安全是指在 C# 中，变量在声明时需要指定其类型，并且在使用过程中必须与其声明的类型相匹配。这有助于在编译时捕捉类型错误，并提高代码的可靠性和可维护性。

5. 作用域：

   - 变量的作用域指的是其可见性和可访问性的范围。
   - 变量可以在代码块内部声明，包括方法、循环、条件语句等。
   - 变量的作用域通常是在声明它的代码块中，可以通过大括号 `{}` 来限定作用域。
   - 局部变量的作用域在声明的代码块内有效，一旦超出作用域，变量就无法访问。

6. 生命周期：

   - 变量的生命周期是指变量存在的时间段。
   - 局部变量的生命周期与其作用域一致，当超出作用域时，变量将被销毁。
   - 类成员变量的生命周期与其所属的对象或类的生命周期相关。

7. 值赋值和修改：

   - 变量可以通过赋值运算符 `=` 来进行值的赋值。
   - 变量可以在其生命周期内被修改，即可以重新赋予不同的值。

> 变量的简单类型
>
> 在C#中，有许多简单的数据类型可以用来声明变量。以下是一些常见的简单类型：
>
> 1. 整数类型：
>    - `int`: 表示整数，范围为约 -2,147,483,648 到 2,147,483,647。
>    - `long`: 表示长整数，范围更大，约 -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807。
>    - `short`: 表示短整数，范围约为 -32,768 到 32,767。
>    - `byte`: 表示无符号字节，范围为 0 到 255。
> 2. 浮点类型：
>    - `float`: 表示单精度浮点数，范围为约 ±1.5 x 10^(-45) 到 ±3.4 x 10^(38)，精度约为 7 位小数。
>    - `double`: 表示双精度浮点数，范围更大，约 ±5.0 x 10^(-324) 到 ±1.7 x 10^(308)，精度约为 15 位小数。
> 3. 字符类型：
>    - `char`: 表示单个字符，用单引号括起来，例如 'A'、'b'。
> 4. 布尔类型：
>    - `bool`: 表示布尔值，只有两个可能的值：`true` 或 `false`。
> 5. 字符串类型：
>    - `string`: 表示文本字符串，用双引号括起来，例如 "Hello"、"World"。
> 6. 其他简单类型：
>    - `decimal`: 表示高精度小数，适用于金融和货币计算，精度约为 28-29 位小数。
>    - `DateTime`: 表示日期和时间值。
>    - `enum`: 表示枚举类型，用于定义一组具名的常量值。
>
> 这些是C#中一些常见的简单数据类型，你可以根据需要选择适当的类型来声明变量。每种类型都有其特定的用途和取值范围。

#### 块

在C#中，块（block）是由一组语句（statements）组成的代码段。块可以用来将多个语句组合在一起，以便按照一定的逻辑顺序执行。常见的使用块的情况是在条件语句（if、else if、else）和循环语句（for、while、do while）中。

一个块由一对花括号（{}）括起来，其中包含了一系列的语句。例如：

```c#
csharpCopy code{
    // 这是一个块
    Console.WriteLine("语句1");
    Console.WriteLine("语句2");
    // ...
    Console.WriteLine("语句n");
}
```

在上面的示例中，花括号内的部分就是一个块，它包含了多个语句。当程序执行到这个块时，其中的语句将按照编写的顺序依次执行。

块的作用不仅仅是为了组织代码，还可以用来限制变量的作用域。在一个块内部声明的变量只在该块内有效，在块外部是不可见的。这种方式可以提供更好的代码封装和变量管理。例如：

```c#
csharpCopy code{
    int x = 10; // 在块内部声明变量x
    Console.WriteLine(x); // 输出：10
}

Console.WriteLine(x); // 编译错误，x 在此处不可见
```

在上面的示例中，变量 `x` 在块内部声明，并在该块内部可见。在块外部的代码中，变量 `x` 是不可见的，因此会导致编译错误。

总之，块在C#中是一种用于组织语句和限制变量作用域的重要结构。

#### *字面值

在C#中，字面值（Literal）是指直接表示特定值的固定文本或符号。字面值可以用于声明和初始化变量，或者直接用于表达式中。

以下是C#中常见的字面值类型：

1. 整数字面值：
   - 十进制整数：例如 `123`。
   - 二进制整数：以 `0b` 或 `0B` 开头，后跟一串二进制数字。例如 `0b1010` 表示十进制的 10。
   - 八进制整数：以 `0` 开头，后跟一串八进制数字。例如 `0123` 表示十进制的 83。
   - 十六进制整数：以 `0x` 或 `0X` 开头，后跟一串十六进制数字。例如 `0xFF` 表示十进制的 255。
2. 浮点数字面值：
   - 十进制浮点数：例如 `3.14`。
   - 指数表示法：使用 `E` 或 `e` 表示指数部分。例如 `1.23e-4` 表示十进制的 0.000123。
3. 字符字面值：
   - 字符字面值用单引号括起来，表示单个字符。例如 `'A'`、`'7'`、`'!'`。
4. 字符串字面值：
   - 字符串字面值用双引号括起来，表示文本字符串。例如 `"Hello"`、`"World"`。
5. 布尔字面值：
   - 布尔字面值表示逻辑值，只有两个可能的值：`true` 和 `false`。

除了上述基本类型的字面值，C#还支持其他类型的字面值，如日期时间字面值、空值字面值等。

以下是一些示例：

```c#
csharpCopy codeint num = 42;           // 整数字面值
double pi = 3.14159;    // 浮点数字面值
char ch = 'A';          // 字符字面值
string message = "Hello, World!";   // 字符串字面值
bool isTrue = true;     // 布尔字面值
DateTime now = DateTime.Now;   // 日期时间字面值
```

这些字面值在代码中直接表示相应的值，无需其他转换或操作。

#### 运算符

C#中有多种运算符，用于执行各种算术、逻辑和位运算。以下是C#中常用的运算符的分类和示例：

1. 算术运算符：

   - `+`：加法
   - `-`：减法
   - `*`：乘法
   - `/`：除法
   - `%`：取模（取余数）

2. 关系运算符：

   - `==`：等于
   - `!=`：不等于
   - `>`：大于
   - `<`：小于
   - `>=`：大于等于
   - `<=`：小于等于

3. 逻辑运算符：

   - `&&`：逻辑与（and）
   - `||`：逻辑或（or）
   - `!`：逻辑非（not）

4. 赋值运算符：

   - `=`：简单赋值
   - `+=`：加法赋值
   - `-=`：减法赋值
   - `*=`：乘法赋值
   - `/=`：除法赋值
   - `%=`：取模赋值

5. 位运算符：

   - `&`：按位与
   - `|`：按位或
   - `^`：按位异或
   - `<<`：左移
   - `>>`：右移
   - `~`：按位取反

6. 条件运算符：

   - `? :`：条件运算符（三元运算符），用于简化条件语句

     > 在C#中，三元运算符（Conditional Operator）是一种简洁的条件表达式，它允许根据条件的真假选择不同的值。三元运算符的语法形式为 `condition ? trueValue : falseValue`，其中 `condition` 是一个条件表达式，`trueValue` 是在条件为真时返回的值，`falseValue` 是在条件为假时返回的值。
     >
     > 以下是一个使用三元运算符的示例：
     >
     > ```c#
     > csharpCopy code
     > int num = (x > y) ? x : y;
     > ```
     >
     > 在上面的示例中，根据条件 `(x > y)` 的真假，如果条件为真，则将 `x` 的值赋给 `num`；如果条件为假，则将 `y` 的值赋给 `num`。这样就可以根据条件动态地选择不同的值进行赋值。
     >
     > 三元运算符的优点是简洁、易读，并且可以在单行中完成条件判断和赋值操作。但是，需要注意避免过度使用，以免降低代码的可读性和可维护性。在某些情况下，使用 `if-else` 语句可能更易于理解和维护，特别是当需要执行更复杂的逻辑时。因此，在选择使用三元运算符还是 `if-else` 语句时，可以根据具体情况和代码的可读性来进行判断。

7. 其他运算符：

   - `()`：括号，用于分组和优先级控制
   - `[]`：方括号，用于访问数组元素和索引器
   - `.`：点运算符，用于访问成员和方法

> 布尔运算符:
>
> C# 中的布尔运算符用于执行逻辑运算，操作布尔类型的值（true 或 false）。以下是常用的布尔运算符：
>
> 逻辑与运算符（&&）：当且仅当两个操作数都为 true 时，结果才为 true。否则，结果为 false。
>
> ```c#
> csharpCopy code
> bool result = condition1 && condition2;
> ```
>
> 逻辑或运算符（||）：当至少一个操作数为 true 时，结果为 true。只有两个操作数都为 false 时，结果才为 false。
>
> ```c#
> csharpCopy code
> bool result = condition1 || condition2;
> ```
>
> 逻辑非运算符（!）：对单个操作数进行取反操作，如果操作数为 true，则结果为 false；如果操作数为 false，则结果为 true。
>
> ```c#
> csharpCopy code
> bool result = !condition;
> ```
>
> 除了上述三个基本的布尔运算符，C# 还提供了其他一些用于布尔运算的运算符，如：
>
> 相等运算符（==）：用于比较两个操作数是否相等，如果相等则结果为 true，否则为 false。
>
> ```c#
> csharpCopy code
> bool result = value1 == value2;
> ```
>
> 不等运算符（!=）：用于比较两个操作数是否不相等，如果不相等则结果为 true，否则为 false。
>
> ```c#
> csharpCopy code
> bool result = value1 != value2;
> ```
>
> 大于运算符（>）、小于运算符（<）、大于等于运算符（>=）、小于等于运算符（<=）：用于比较两个操作数的大小关系，返回结果为 true 或 false。
>
> ```c#
> csharpCopy codebool result = value1 > value2;
> bool result = value1 < value2;
> bool result = value1 >= value2;
> bool result = value1 <= value2;
> ```
>
> 这些布尔运算符可用于控制流程、条件判断、循环控制等情况下，以便根据条件的真假进行相应的操作。

#### 转义字符

在C#中，转义字符（Escape Character）是用于表示一些特殊字符的特殊序列。转义字符以反斜杠 `\` 开头，后面跟着一个或多个字符，用于表示特定的含义。

以下是C#中常见的转义字符及其含义：

- `\'`：表示单引号字符。
- `\"`：表示双引号字符。
- `\\`：表示反斜杠字符。
- `\n`：表示换行符（ASCII 值为 10）。
- `\r`：表示回车符（ASCII 值为 13）。
- `\t`：表示制表符。
- `\b`：表示退格符。
- `\f`：表示换页符。
- `\uXXXX`：表示一个 Unicode 字符，其中 `XXXX` 是四个十六进制数字。

以下是一些示例：

```c#
csharpCopy codestring singleQuote = '\'';     // 单引号字符
string doubleQuote = '\"';     // 双引号字符
string backslash = '\\';       // 反斜杠字符
string newLine = "\n";         // 换行符
string tab = "\t";             // 制表符
string unicodeChar = "\u03A9"; // 表示希腊字母大写Omega（Ω）
```

在字符串中使用转义字符时，反斜杠后的字符将被解释为其特殊含义，而不是字面字符。

需要注意的是，如果你希望在字符串中显示实际的反斜杠字符而非转义字符，你可以使用两个连续的反斜杠 `\\` 来表示。

> 在C#中，转义字符是一种特殊字符序列，用于表示一些特殊的字符或控制字符。如果你想禁用转义字符，你可以使用字符串前缀 `@` 来创建一个原始字符串（raw string），这样其中的转义字符就会被当作普通字符对待。
>
> 下面是一个示例，展示如何在C#中禁用转义字符：
>
> ```c#
> csharpCopy codestring normalString = "Hello\tWorld"; // 使用转义字符的普通字符串
> string rawString = @"Hello\tWorld"; // 使用原始字符串禁用转义字符
> 
> Console.WriteLine(normalString); // 输出：Hello   World
> Console.WriteLine(rawString); // 输出：Hello\tWorld
> ```
>
> 在 `rawString` 中，`\t` 不再表示制表符，而是被当作普通的两个字符：反斜杠和字母 't'。

#### 条件语句

在C#中，条件语句用于根据条件的真假来执行不同的代码块。C#提供了几种条件语句的形式，包括以下常见的：

1. `if` 语句：

   ```c#
   csharpCopy codeif (condition)
   {
       // 当条件为真时执行的代码
   }
   else
   {
       // 当条件为假时执行的代码
   }
   ```

2. `else if` 语句：

   ```c#
   csharpCopy codeif (condition1)
   {
       // 当条件1为真时执行的代码
   }
   else if (condition2)
   {
       // 当条件1为假而条件2为真时执行的代码
   }
   else
   {
       // 当前面条件都为假时执行的代码
   }
   ```

3. `switch` 语句：

   ```c#
   csharpCopy codeswitch (variable)
   {
       case value1:
           // 当变量等于value1时执行的代码
           break;
       case value2:
           // 当变量等于value2时执行的代码
           break;
       default:
           // 当变量不匹配任何case时执行的代码
           break;
   }
   ```

这些条件语句根据条件的真假来选择执行不同的代码块。`if` 语句根据单个条件的真假执行不同的代码块，而 `else if` 语句可以用于检查多个条件并选择执行对应的代码块。`switch` 语句根据变量的值进行匹配，并执行与值匹配的代码块。

这些条件语句使程序能够根据不同的情况采取不同的行动，从而实现灵活的控制流程和逻辑。您可以根据具体的需求选择适当的条件语句来控制程序的行为。

#### 循环语句

在C#中，循环语句用于重复执行一段代码块，直到满足指定的条件。C#提供了几种常用的循环语句，包括以下：

1. `for` 循环：

   ```c#
   csharpCopy codefor (initialization; condition; iteration)
   {
       // 循环执行的代码
   }
   ```

2. `while` 循环：

   ```c#
   csharpCopy codewhile (condition)
   {
       // 循环执行的代码
   }
   ```

3. `do-while` 循环：

   ```c#
   csharpCopy codedo
   {
       // 循环执行的代码
   } while (condition);
   ```

4. `foreach` 循环（用于遍历集合）：

   ```c#
   csharpCopy codeforeach (var item in collection)
   {
       // 循环执行的代码，item 代表集合中的每个元素
   }
   ```

这些循环语句允许您根据需要重复执行一段代码块，直到满足指定的条件。`for` 循环在执行循环之前进行初始化，并在每次迭代后进行迭代操作。`while` 循环在每次循环开始之前检查条件。`do-while` 循环首先执行一次循环体，然后在每次循环结束后检查条件。

`foreach` 循环用于遍历集合中的元素，无需手动追踪索引或迭代器。它适用于数组、列表、字典等集合类型。

#### 枚举

在C#中，枚举（Enum）是一种特殊的数据类型，用于定义一组具名的常量值。枚举类型允许我们为特定概念创建有意义的命名常量，使代码更加可读和可维护。

以下是一个枚举的示例：

```c#
csharpCopy codeenum DayOfWeek
{
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
}
```

在上面的示例中，我们定义了一个名为 `DayOfWeek` 的枚举类型，它包含了一周中的每一天。枚举中的每个值都是一个常量，并用逗号分隔。默认情况下，第一个枚举成员的值为0，然后依次递增。

我们可以使用枚举类型来声明变量，并将其限制为枚举中的某个特定值，例如：

```c#
csharpCopy code
DayOfWeek today = DayOfWeek.Monday;
```

在上面的示例中，我们将 `today` 声明为 `DayOfWeek` 枚举类型的变量，并将其赋值为 `DayOfWeek.Monday`。这样，我们可以使用 `today` 变量来表示今天是星期几。

枚举还可以使用 `switch` 语句进行处理，方便地处理不同枚举值的情况：

```c#
csharpCopy codeswitch (today)
{
    case DayOfWeek.Monday:
        Console.WriteLine("今天是星期一");
        break;
    case DayOfWeek.Tuesday:
        Console.WriteLine("今天是星期二");
        break;
    // ...
    default:
        Console.WriteLine("未知的星期");
        break;
}
```

在上面的示例中，我们根据 `today` 变量的值，通过 `switch` 语句判断今天是星期几，并输出相应的信息。

枚举提供了一种方便的方式来定义和使用一组相关的常量值，以增加代码的可读性和可维护性。它在许多情况下都是非常有用的，例如表示日期、状态、选项等。

#### 结构体

在C#中，结构体（Struct）是一种用户定义的值类型，用于封装一组相关的数据字段。结构体是值类型，意味着它们在赋值或传递给方法时会被复制，而不是引用。

以下是一个结构体的示例：

```c#
csharpCopy codestruct Point
{
    public int X;
    public int Y;
}
```

在上面的示例中，我们定义了一个名为 `Point` 的结构体，它包含两个整型字段 `X` 和 `Y`。这个结构体表示二维平面上的一个点。

我们可以创建结构体的实例，并访问其字段，例如：

```c#
csharpCopy codePoint p;
p.X = 10;
p.Y = 5;
Console.WriteLine($"X: {p.X}, Y: {p.Y}");
```

在上面的示例中，我们创建了一个名为 `p` 的 `Point` 结构体的实例，并设置了其 `X` 和 `Y` 字段的值。然后，我们使用插值字符串将字段的值输出到控制台。

结构体可以具有构造函数、方法和属性，以提供更丰富的功能。此外，结构体还可以实现接口，从而使其具备更多的行为和特性。

与类相比，结构体通常更适合用于简单的数据封装，特别是对于小型、轻量级的对象。由于结构体是值类型，它们在内存上的存储方式和复制行为也与类不同，这些特性使得结构体在某些场景下具有性能优势。

需要注意的是，结构体是值类型，因此在将结构体实例传递给方法时，传递的是结构体的副本而不是引用。这可能会导致性能开销，尤其在结构体较大或复杂时。在这种情况下，可以考虑使用类来代替结构体。

#### 数组

在C#中，数组（Array）是一种用于存储相同类型的元素的数据结构。数组提供了一个连续的内存块来存储多个元素，并使用索引来访问和操作这些元素。

以下是一个数组的示例：

```c#
csharpCopy code
int[] numbers = new int[5];
```

在上面的示例中，我们声明了一个名为 `numbers` 的整型数组，它可以存储5个整数。使用 `new int[5]` 创建了一个长度为5的整型数组，并将其赋值给 `numbers` 变量。

我们可以使用索引来访问数组中的元素。数组的索引从0开始，因此可以使用 `[index]` 访问特定位置的元素，例如：

```c#
csharpCopy codenumbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;

Console.WriteLine(numbers[1]);  // 输出: 20
```

在上面的示例中，我们使用索引操作符 `[index]` 将值分配给数组的特定位置，并使用索引访问特定位置的元素。

除了使用固定大小的数组，C#还提供了动态数组，称为 `List<T>`，它具有更灵活的大小调整和功能。使用 `List<T>` 类型可以动态添加、删除和修改元素。

以下是一个使用 `List<int>` 的示例：

```c#
csharpCopy codeList<int> numbers = new List<int>();
numbers.Add(10);
numbers.Add(20);
numbers.Add(30);

Console.WriteLine(numbers[1]);  // 输出: 20
```

在上面的示例中，我们创建了一个 `List<int>` 类型的对象，并使用 `Add` 方法向列表中添加整数。然后，使用索引访问特定位置的元素。

数组在许多编程场景中都是非常有用的，可以用于存储和处理大量数据，以及实现诸如集合、矩阵、图像等数据结构和算法。

> 在C#中，`new` 操作符用于创建一个对象的实例或初始化一个数组。它在内存中分配了足够的空间来存储对象或数组，并调用适当的构造函数来初始化对象的状态。
>
> `new` 操作符的用法有以下几个常见的情况：
>
> 1. 创建对象实例：
>
>    ```c#
>    csharpCopy code
>    ClassName obj = new ClassName();
>    ```
>
>    这种用法创建了一个类的实例，并调用了该类的默认构造函数来初始化对象的初始状态。可以通过 `obj` 变量来访问和操作该对象的成员。
>
> 2. 创建对象实例并传递参数：
>
>    ```c#
>    csharpCopy code
>    ClassName obj = new ClassName(arg1, arg2, ...);
>    ```
>
>    这种用法创建了一个类的实例，并调用了带有指定参数的构造函数来初始化对象的状态。构造函数根据参数的类型和顺序来确定调用哪个构造函数重载。
>
> 3. 初始化数组：
>
>    ```c#
>    csharpCopy code
>    int[] numbers = new int[5];
>    ```
>
>    这种用法创建了一个指定长度的数组，并使用默认值对数组的元素进行初始化。在上面的示例中，数组的长度为5，所有元素都被初始化为 `int` 类型的默认值 0。
>
> 在C#中，`new` 操作符是用于动态创建对象和数组的关键操作符之一。它使我们能够在运行时创建和初始化新的实例，以满足程序的需求。同时，`new` 操作符也触发了相应类型的构造函数，允许我们在对象创建的过程中执行必要的初始化操作。

#### 函数

在C#中，函数具有以下特点：

1. 封装性（Encapsulation）：函数允许将相关代码块封装成一个独立的单元，使代码更加模块化和可复用。函数可以隐藏内部实现细节，并提供一个公共接口供其他部分使用。
2. 参数传递（Parameter Passing）：函数可以接受零个或多个参数，这些参数用于向函数传递数据。在C#中，参数可以是值类型、引用类型或输出参数，允许函数对传入的参数进行读取和修改。
3. 返回值（Return Value）：函数可以返回一个值，用于向调用方提供计算结果或其他需要的信息。返回值可以是任何有效的数据类型，包括基本类型、自定义类型和引用类型。
4. 可访问性（Accessibility）：函数可以具有不同的访问修饰符，如`public`、`private`、`protected`等，用于控制函数的可见性和访问权限。这决定了哪些部分可以调用函数和访问函数内部。
5. 方法重载（Method Overloading）：在C#中，可以定义具有相同名称但参数列表不同的多个函数，称为方法重载。通过方法重载，可以根据不同的参数类型和数量来选择调用合适的函数。
6. 递归（Recursion）：函数可以调用自身，这称为递归。递归在解决某些问题时非常有用，可以简化代码逻辑，但需要注意递归终止条件，以避免无限递归。
7. 异常处理（Exception Handling）：函数可以抛出异常（`throw`语句），并在调用方处于适当的位置进行异常处理（`try-catch`语句）。异常处理可以捕获和处理在函数执行过程中可能发生的错误或异常情况。
8. 匿名函数和Lambda表达式（Anonymous Functions and Lambda Expressions）：C#支持定义匿名函数和使用Lambda表达式，这使得在需要时可以更方便地编写简单的函数代码块。

### 基本操作

#### 输入与输出

在C#中，可以使用标准输入和输出流进行输入和输出操作。下面是一些常用的输入和输出方法：

Console.ReadLine()：从标准输入流读取一行用户输入，并以字符串的形式返回。

```c#
csharpCopy code
string input = Console.ReadLine();
```

![image-20230529202723287](image-20230529202723287.png)

Console.Read()：从标准输入流读取下一个字符的 ASCII 值，并以整数形式返回。

```c#
csharpCopy code
    //只能读一个是吗？
int input = Console.Read();
```

![image-20230529203000925](image-20230529203000925.png)

*Console.ReadKey()：从标准输入流读取下一个键入的字符，并返回一个表示该键入的 ConsoleKeyInfo 对象。

```c#
csharpCopy codeConsoleKeyInfo keyInfo = Console.ReadKey();
char input = keyInfo.KeyChar;
```

> ```c#
> using System;
> using System.Collections.Generic;
> using System.Linq;
> using System.Text;
> using System.Threading.Tasks;
> 
> namespace _001_第一个程序
> {
>     internal class Program
>     {
>         static void Main(string[] args)
>         {
>             //结构体：接受输入的值
>             ConsoleKeyInfo keyInfo = Console.ReadKey();
> 
>             char inputChar = keyInfo.KeyChar;         // 获取按下的字符值
>             ConsoleKey key = keyInfo.Key;             // 获取按下的键的枚举值
>             ConsoleModifiers modifiers = keyInfo.Modifiers;  // 获取修饰键的状态
> 
>             Console.WriteLine("输入的字符值: " + inputChar);
>             Console.WriteLine("输入的键: " + key);
>             Console.WriteLine("修饰键的状态: " + modifiers);
> 
>         }
>     }
> }
> ```
>
> `ConsoleKeyInfo` 是在 `System` 命名空间下定义的结构，用于表示从标准输入流读取的键入信息。该结构包含了按下的键、键的字符值和修饰键的状态。
>
> 以下是一些 `ConsoleKeyInfo` 结构的属性：
>
> - `KeyChar`：按下的键对应的字符值。如果按下的是特殊键或无法映射到字符的键，则该属性的值为 `\0`。
> - `Key`：按下的键的枚举值，类型为 `ConsoleKey`。可以使用 `Key` 属性来判断按下的是哪个特殊键，如方向键、功能键等。
> - `Modifiers`：按下键时同时按下的修饰键（如 Shift、Ctrl 等）。该属性的类型为 `ConsoleModifiers` 枚举，可以使用按位运算符来判断是否同时按下了某个修饰键。
>
> 以下是一个示例，演示如何使用 `ConsoleKeyInfo` 结构：
>
> ```c#
> csharpCopy codeConsoleKeyInfo keyInfo = Console.ReadKey();
> 
> char inputChar = keyInfo.KeyChar;         // 获取按下的字符值
> ConsoleKey key = keyInfo.Key;             // 获取按下的键的枚举值
> ConsoleModifiers modifiers = keyInfo.Modifiers;  // 获取修饰键的状态
> 
> Console.WriteLine("输入的字符值: " + inputChar);
> Console.WriteLine("输入的键: " + key);
> Console.WriteLine("修饰键的状态: " + modifiers);
> ```
>
> 通过上述代码，你可以获取按下的字符值、键和修饰键的状态，并根据需要进行进一步的处理和判断。

Console.Write()：将指定的数据写入标准输出流，不附加换行符。

```c#
Console.Write("The number is: ");
Console.Write(number);
```

![image-20230529210259411](image-20230529210259411.png)

Console.WriteLine()：将指定的数据写入标准输出流，并附加换行符。

```c#
string name = "John";
Console.WriteLine("Hello, " + name + "!");
```

![image-20230529210347145](image-20230529210347145.png)

Console.WriteFormat()：将格式化的字符串写入标准输出流。

```c#
string name = "Alice";
int age = 25;
Console.WriteFormat("My name is {0} and I am {1} years old.", name, age);
```

![image-20230529210603766](image-20230529210603766.png)

> 这里用到了占位符，就简单的讲一下：
>
> 占位符的使用特点如下：
>
> 1. 顺序替换：占位符按照在格式化字符串中的顺序进行替换。第一个占位符 `{0}` 对应第一个参数，第二个占位符 `{1}` 对应第二个参数，依此类推。确保提供的参数数量与占位符的数量匹配，否则会引发异常。
> 2. 参数类型灵活：占位符可以接受任意类型的参数。在替换过程中，会自动将参数转换为字符串形式。这允许你在格式化字符串中插入各种类型的数据，包括字符串、数字、日期等。
> 3. 重复使用：占位符可以在格式化字符串中多次重复使用。这意味着你可以多次插入同一个参数值，或者在不同位置插入相同或不同的参数值。
> 4. 格式化选项：占位符还可以使用格式化选项来指定参数的显示格式。格式化选项可以用于数字、日期、时间等类型的参数，以控制其显示方式。例如：`{0:C}` 表示将第一个参数作为货币值进行格式化显示。
>
> 下面是一个示例，展示了占位符的使用特点：
>
> ```c#
> csharpCopy codestring name = "Alice";
> int age = 25;
> double salary = 2500.50;
> 
> string result = String.Format("My name is {0}, I am {1} years old, and my salary is {2:C}.", name, age, salary);
> Console.WriteLine(result);
> ```
>
> 输出结果：
>
> ```c#
> swiftCopy code
> My name is Alice, I am 25 years old, and my salary is $2,500.50.
> ```
>
> 在上述示例中，`{0}` 对应变量 `name`，`{1}` 对应变量 `age`，`{2:C}` 对应变量 `salary`。参数按照顺序替换占位符，并使用相应的格式化选项。
>
> 通过占位符的使用特点，你可以方便地构建格式化的字符串，插入适当的参数，并根据需要进行格式化控制。

这些方法可以用于控制台应用程序中的输入和输出操作。你可以根据需要使用适当的方法来读取用户输入或向控制台输出数据。

#### 字符串拼接

在 C# 中，有多种方法可以进行字符串拼接。下面是一些常见的字符串拼接规则和方法：

使用 `+` 运算符：可以使用 `+` 运算符将多个字符串连接在一起。

```c#
string = "John";
string lastName = "Doe";
string fullName = firstName + " " + lastName;
Consolo.Write(fullName);
```

![image-20230529212308991](image-20230529212308991.png)

*使用字符串插值（String Interpolation）：可以使用 `$` 符号和花括号 `{}` 来将变量插入到字符串中。

```c#
string firstName = "John";
string lastName = "Doe";
string fullName = $"{firstName} {lastName}";
```

![image-20230529212411765](image-20230529212411765.png)

使用 `String.Format` 方法：可以使用 `String.Format` 方法来格式化字符串，并将参数插入到占位符中。

```c#
string firstName = "John";
string lastName = "Doe";
string fullName = String.Format("{0} {1}", firstName, lastName);
```

![image-20230529212513492](image-20230529212513492.png)

**使用 `StringBuilder` 类：如果需要高效地拼接大量字符串或在循环中进行拼接操作，可以使用 `StringBuilder` 类。**

```c#
StringBuilder sb = new StringBuilder();
sb.Append("Hello");
sb.Append(" ");
sb.Append("World");
string result = sb.ToString();
```

![image-20230529212821511](image-20230529212821511.png)

需要注意的是，字符串是不可变的（immutable），这意味着每次对字符串进行修改时，实际上会创建一个新的字符串对象。因此，在需要进行大量字符串拼接的情况下，使用 `StringBuilder` 类可以避免不必要的内存分配和性能损失。

> 在C#中，有许多常用的字符串操作方法可用于处理和操作字符串。以下是一些常见的字符串操作示例：
>
> 1. 字符串连接：
>
>    ```c#
>    cCopy codestring str1 = "Hello";
>    string str2 = "World";
>    string result = str1 + " " + str2;  // 结果为 "Hello World"
>    ```
>
> 2. 字符串长度：
>
>    ```c#
>    cCopy codestring str = "Hello World";
>    int length = str.Length;  // 结果为 11
>    ```
>
> 3. 字符串截取：
>
>    ```c#
>    cCopy codestring str = "Hello World";
>    string substring = str.Substring(6);  // 结果为 "World"
>    ```
>
> 4. 字符串分割：
>
>    ```c#
>    luaCopy codestring str = "apple,banana,orange";
>    string[] fruits = str.Split(',');  // 结果为 ["apple", "banana", "orange"]
>    ```
>
> 5. 字符串替换：
>
>    ```c#
>    cCopy codestring str = "Hello World";
>    string replaced = str.Replace("World", "Universe");  // 结果为 "Hello Universe"
>    ```
>
> 6. 字符串大小写转换：
>
>    ```c#
>    cCopy codestring str = "Hello World";
>    string uppercase = str.ToUpper();  // 结果为 "HELLO WORLD"
>    string lowercase = str.ToLower();  // 结果为 "hello world"
>    ```
>
> 7. 字符串去除空格：
>
>    ```c#
>    cCopy codestring str = "  Hello World  ";
>    string trimmed = str.Trim();  // 结果为 "Hello World"
>    ```
>
> 8. 字符串查找：
>
>    ```c#
>    rustCopy codestring str = "Hello World";
>    bool containsHello = str.Contains("Hello");  // 结果为 true
>    int index = str.IndexOf("World");  // 结果为 6
>    ```

#### 变量的类型转换

在C#中，可以使用类型转换操作符或转换方法来进行变量类型转换。以下是几种常见的类型转换方法：

1. 显式类型转换（Explicit Casting）：

   ```c#
   csharpCopy code// 使用类型转换操作符
   int intValue = (int)doubleValue;
   
   // 使用Convert类的转换方法
   int intValue = Convert.ToInt32(doubleValue);
   ```

2. 隐式类型转换（Implicit Casting）：

   ```c#
   csharpCopy code// 隐式类型转换只适用于可安全地转换的情况
   int intValue = 10;
   double doubleValue = intValue;
   ```

3. Parse方法和TryParse方法：

   ```c#
   csharpCopy code// Parse方法将字符串转换为特定类型的值
   int intValue = int.Parse("10");
   
   // TryParse方法尝试将字符串转换为特定类型的值，并返回转换是否成功的布尔值
   bool success = int.TryParse("10", out int intValue);
   ```

4. Convert类的转换方法：

   ```c#
   csharpCopy code// 使用Convert类的转换方法进行类型转换
   int intValue = Convert.ToInt32(doubleValue);
   ```

## 练习1

用几个简单的实例，来让我们熟系c#代码的功能吧。

### 交换两个数据的值

```c#
namespace _002_练习
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //声明两个变量交换他们的值
            int a = 100;
            int b = 10;
            int temp = 0;
            Console.WriteLine("打印交换之前的数据");
            Console.WriteLine(a);
            Console.WriteLine(b);

            // 等待用户按下一个键
            Console.ReadKey(); 

            temp = a;
            a = b;
            b = temp;
            //打印
            Console.WriteLine(a);
            Console.WriteLine(b);
        }
    }
}
```

### 计算梯形和圆形的值

```c++
namespace _002_练习
{
    internal class Program
    {
        static double m(double up, double down, double h)
        {
            return (up + down) * h / 2;
        }

        static void Main(string[] args)
        {
            Console.WriteLine("计算梯形的面积");
            Console.WriteLine("请输入上底：");
            double up = Convert.ToDouble(Console.ReadLine());

            Console.WriteLine("请输入下底：");
            double down = Convert.ToDouble(Console.ReadLine());

            Console.WriteLine("请输入高度：");
            double h = Convert.ToDouble(Console.ReadLine());

            double result = m(up, down, h);

            Console.WriteLine("计算结果为：" + result);
            Console.ReadKey();
        }
    }
}
```

> `ToDouble` 是一个用于将其他数据类型转换为 `double` 类型的方法。它是.NET Framework 中的一个内置方法，可用于在C#中进行数据类型转换。
>
> 在C#中，数据类型转换是将一个数据类型的值转换为另一个数据类型的过程。当您需要在不同的数据类型之间进行转换时，可以使用适当的类型转换方法。
>
> `ToDouble` 方法用于将其他数据类型的值转换为 `double` 类型的值。它是静态方法，可以通过调用相应数据类型的实例来使用。
>
> 例如，如果要将一个字符串转换为 `double` 类型，可以使用以下代码：
>
> ```c#
> csharpCopy codestring numberString = "3.14";
> double number = Convert.ToDouble(numberString);
> ```
>
> 在上述示例中，我们将字符串变量 `numberString` 的值转换为 `double` 类型，并将结果存储在 `number` 变量中。`Convert.ToDouble()` 方法根据字符串的内容将其转换为相应的 `double` 类型值。
>
> 请注意，如果无法成功进行转换（例如，字符串不是有效的数字表示），则会抛出 `FormatException` 异常。因此，在进行数据类型转换时，需要确保输入的数据与目标数据类型兼容，否则可能会导致运行时错误。
>
> > `Convert` 是一个在 .NET Framework 中提供的类，用于执行数据类型之间的转换操作。它包含了各种静态方法，用于在不同的数据类型之间进行转换。
> >
> > `Convert` 类提供了一系列的静态方法，用于将一个数据类型的值转换为另一个数据类型。它支持各种常见的数据类型，例如整数、浮点数、字符串、日期等。通过调用相应的 `Convert` 方法，您可以将一个数据类型的值转换为另一个数据类型，前提是转换是合法和有效的。
> >
> > 以下是一些常用的 `Convert` 方法的示例：
> >
> > 1. `Convert.ToInt32()`：将其他数据类型的值转换为 32 位整数类型（`int`）。
> > 2. `Convert.ToDouble()`：将其他数据类型的值转换为双精度浮点数类型（`double`）。
> > 3. `Convert.ToString()`：将其他数据类型的值转换为字符串类型（`string`）。
> > 4. `Convert.ToDateTime()`：将其他数据类型的值转换为日期和时间类型（`DateTime`）。
> >
> > 这些方法使用方法类似于 `Convert.ToDouble(value)`，其中 `value` 是要转换的值。方法将返回转换后的值，并根据需要进行合理的舍入、截断或格式化操作。
> >
> > 需要注意的是，在进行转换之前，需要确保源数据类型可以被转换为目标数据类型，否则可能会引发异常。例如，如果尝试将一个非数字字符串转换为整数类型，则会引发 `FormatException` 异常。
> >
> > `Convert` 类提供了一种方便且安全的方法来执行数据类型转换，使开发人员能够轻松地在不同的数据类型之间进行转换操作。

为什么要在函数体前加static？

> 在C#中，`static` 关键字用于声明静态成员（方法、字段、属性等）。当在函数体前加上 `static` 关键字时，表示该方法是一个静态方法。
>
> 静态方法与实例方法有一些不同之处：
>
> 1. 静态方法属于类本身，而不是类的实例。可以通过类名直接调用静态方法，而无需创建类的实例。
> 2. 静态方法可以在不创建类的实例的情况下访问和操作类的静态成员（静态字段、静态属性等）。
> 3. 静态方法不能直接访问实例成员（实例字段、实例属性等），因为它们没有与特定实例关联。
>
> 在将方法声明为静态时，有几个常见的原因和用途：
>
> 1. 共享性：静态方法是类的共享成员，所有类的实例都可以共享相同的静态方法。这使得静态方法可以用于实现与类相关的功能，而无需每次创建实例。
> 2. 实用性：某些方法不需要访问实例状态或实例成员，只需要执行一些独立于实例的操作。将这些方法声明为静态方法可以更清晰地表达其意图和用途。
> 3. 工具类方法：静态方法经常用于实现工具类或辅助类中的通用功能。这些方法不需要访问实例状态，可以通过类名直接调用。
>
> 需要注意的是，静态方法只能访问静态成员，无法直接访问实例成员。如果在静态方法中需要访问实例成员，可以通过传递实例参数或创建实例对象来间接访问。
>
> 总而言之，将函数体前加上 `static` 关键字表示该方法是一个静态方法，具有与实例方法不同的特性和用途。

```c#
namespace _002_练习
{
    internal class Program
    {
        static double o(double r)
        {
            return (3.14 * r * r);  
        }

        static void Main(string[] args)
        {
            Console.WriteLine("请输入圆的半径");
            double r = Convert.ToDouble(Console.ReadLine());
           Console.WriteLine(o(r));
            Console.ReadKey();
        }
    }
}
```

### 条件语句练习

```c#
using System;

namespace ConditionalStatementExercise
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("请输入一个整数：");
            int number = Convert.ToInt32(Console.ReadLine());

            // 判断输入的数值是否为正数、负数或零，并输出相应的消息
            if (number > 0)
            {
                Console.WriteLine("输入的数值是正数");
            }
            else if (number < 0)
            {
                Console.WriteLine("输入的数值是负数");
            }
            else
            {
                Console.WriteLine("输入的数值是零");
            }
  //暂停
            Console.ReadKey();
        }
    }
}
```

### 兔子增速问题

兔子繁育问题。设有一对新生的兔子，从第三个月开始他们每个月都生一对兔子，新生的兔子从第三个月开始又每个月生一对兔子。技此规律，并假定象子没有死亡，20个月后共有多少个兔子? 要求编编写为控制台程序。

```c#
using System;

namespace RabbitReproduction
{
    class Program
    {
        static void Main(string[] args)
        {
            int totalMonths = 20; // 总月数
            int adultPairs = 1; // 成年兔对数（初始为1对）
            int childPairs = 0; // 幼兔对数（初始为0对）

            for (int i = 1; i <= totalMonths; i++)
            {
                int newPairs = adultPairs; // 新生的兔对数等于成年兔对数
                adultPairs += childPairs; // 成年兔对数增加幼兔对数
                childPairs = newPairs; // 幼兔对数更新为新生的兔对数
                
   //$符号是C#语言中的字符串插值符号。它允许你在字符串中嵌入表达式
                Console.WriteLine($"第{i}个月：成年兔对数：{adultPairs}，幼兔对数：{childPairs}");
            }

            int totalPairs = adultPairs + childPairs; // 总兔对数
            Console.WriteLine($"总共有{totalMonths}个月后，共有{totalPairs}对兔子。");

            Console.ReadKey();
        }
    }
}
```

```c#
//随机数版本
namespace _02_兔子增速
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //开始兔子数量给他一个随机数
            int starting_number = new Random().Next(1,10);

            //一共生二十个月
            int totalMouths = 20;
            int adultPairs = starting_number;
            //生下来幼小的兔子开始为0对
            int childPairs = 0;

            for(int i = 0; i < totalMouths; i++)
            {
                int newPair = adultPairs; //新生的兔子对数等于成年兔对数
                adultPairs += childPairs; //成年兔对数增加幼兔对数
                childPairs = newPair; //幼兔出现

                Console.WriteLine($"第{i}个月：成年兔对数：{adultPairs}，幼兔对数：{childPairs}");
            }
            //总数 ： 幼+成
            int totalPairs = adultPairs+childPairs;
            Console.WriteLine($"总共有{totalMouths}个月后，共有{totalPairs}对兔子。");

            Console.ReadKey();



        }
    }
}
```

### 控制输出

```c#
namespace NumberList
{
    class Program
    {
        static void Main(string[] args)
        {
            while (true)
            {
                Console.WriteLine("请输入一个整数：");
                int n = Convert.ToInt32(Console.ReadLine());

                if (n > 0)
                {
                    for (int i = 1; i <= n; i++)
                    {
                        Console.Write(i);
                    }
                    //打印个空格 换行
                    Console.WriteLine();
                }
                else if (n < 0)
                {
                    break; // 退出程序
                }
                else
                {
                    continue; // 转到下一次循环，接收下一个整数
                }
            }
        }
    }
}
```

### 求解1000以内的所有完数

完数（Perfect Number）是指一个正整数，它恰好等于它的所有因子（除了自身）之和。换句话说，一个完数的所有真因子之和等于它本身。

```c#
using System;

namespace PerfectNumbers
{
    class Program
    {
        static void Main(string[] args)
        {
            for (int number = 1; number <= 1000; number++)
            {
                int sum = 0;

                // 寻找因子并求和
                for (int i = 1; i < number; i++)
                {
                    if (number % i == 0)
                    {
                        sum += i;
                    }
                }

                // 判断是否为完数
                if (sum == number)
                {
                    Console.WriteLine(number);
                }
            }

            Console.ReadKey();
        }
    }
}
```

### 投掷骰子

编写一个掷筛子100次的程序，并打印出各种点数的出现次数。

```c#
using System;

namespace DiceRoll
{
    class Program
    {
        static void Main(string[] args)
        {
            //随机数
            Random random = new Random();
            int[] count = new int[6]; // 用于记录每个点数的出现次数

            for (int i = 0; i < 100; i++)
            {
                int diceValue = random.Next(1, 7); // 随机生成1到6的整数，模拟筛子的点数
                count[diceValue - 1]++; // 对应点数的出现次数加1
            }

            // 打印各种点数的出现次数
            for (int i = 0; i < 6; i++)
            {
                Console.WriteLine($"点数 {i + 1} 出现的次数：{count[i]}");
            }

            Console.ReadKey();
        }
    }
}

```

> 生成随机数：
>
> 在 C# 中，可以使用 `Random` 类来生成随机数。`Random` 类提供了一些方法来生成不同范围和类型的随机数。
>
> 下面是一些常见的随机数生成示例：
>
> 生成一个介于0和最大整数之间的随机整数：
>
> ```c#
> csharpCopy codeRandom random = new Random();
> int randomNumber = random.Next();
> ```
>
> 生成一个指定范围内的随机整数（包括最小值但不包括最大值）：
>
> ```c#
> csharpCopy codeRandom random = new Random();
> int randomNumber = random.Next(minValue, maxValue);
> ```
>
> 生成一个随机双精度浮点数（介于0.0和1.0之间）：
>
> ```c#
> csharpCopy codeRandom random = new Random();
> double randomDouble = random.NextDouble();
> ```
>
> 生成一个指定范围内的随机双精度浮点数：
>
> ```c#
> csharpCopy codeRandom random = new Random();
> double randomDouble = minValue + (random.NextDouble() * (maxValue - minValue));
> ```
>
> 在上述示例中，`Random` 类的实例被创建，并使用 `Next()` 或 `NextDouble()` 方法来生成随机数。对于整数，`Next()` 方法生成一个非负整数；对于浮点数，`NextDouble()` 方法生成一个介于0.0和1.0之间的浮点数。
>
> 需要注意的是，在某些情况下，如果在循环中连续创建 `Random` 对象并立即使用 `Next()` 方法，可能会得到相同的随机数序列。为了避免这种情况，建议将 `Random` 对象的创建移到循环外部，以便重复使用同一个对象。

### 99乘法表

```c#
using System;

namespace MultiplicationTable
{
    class Program
    {
        static void Main(string[] args)
        {
            for (int i = 1; i <= 9; i++)
            {
                //j<=i是个细节
                for (int j = 1; j <= i; j++)
                {
                    Console.Write($"{j} × {i} = {i * j}\t");
                }
                Console.WriteLine();
            }

            Console.ReadKey();
        }
    }
}

```

> `Console.Write($"{j} × {i} = {i * j}\t");`
>
> 字符串插值（String interpolation）的语法。`${j} × {i} = {i * j}\t` 是一个包含格式占位符的字符串，其中 `{}` 用于包含表达式，而 `$` 前缀表示字符串是一个插值字符串。在这个字符串中，`{j}` 表示将变量 `j` 的值插入到字符串中，`{i}` 表示将变量 `i` 的值插入到字符串中，`{i * j}` 表示将变量 `i * j` 的结果插入到字符串中。`\t` 是一个制表符，用于在输出中创建水平制表。
>
> 综上所述，`Console.Write($"{j} × {i} = {i * j}\t")` 的作用是将一条形如 "j × i = (i * j)" 的带有制表符的字符串输出到控制台。

### 计算素数个数

```c#
using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("Prime numbers up to 1000:");

        for (int i = 2; i <= 1000; i++)
        {
            bool isPrime = true;

            for (int j = 2; j <= Math.Sqrt(i); j++)
            {
                if (i % j == 0)
                {
                    isPrime = false;
                    break;
                }
            }

            if (isPrime)
            {
                Console.WriteLine(i);
            }
        }
    }
}
```

### 要求用户输入5个大写字母

```c#
using System;

class Program
{
    static void Main()
    {
        string input;
        bool isValid = false;

        while (!isValid)
        {
            Console.WriteLine("请输入5个大写字母：");
            input = Console.ReadLine();

            if (input.Length == 5 && IsAllUpperCaseLetters(input))
            {
                isValid = true;
                Console.WriteLine("输入有效！");

                // 在这里可以继续处理输入的逻辑
            }
            else
            {
                Console.WriteLine("输入无效，请确保输入5个大写字母。");
            }
        }
    }

    static bool IsAllUpperCaseLetters(string input)
    {
        foreach (char c in input)
        {
            //检查c是否为大写字母
            if (!char.IsUpper(c))
            {
                return false;
            }
        }
        return true;
    }
}

```

> `char.IsUpper(c)` 是一个方法调用，用于判断给定的字符是否为大写字母。`char.IsUpper` 方法返回一个布尔值，如果字符是大写字母，则返回 `true`，否则返回 `false`。

## 练习2

### 猜数字

猜数字游戏，我有一个数请您猜猜是多少?请忽输入一个0-50之问的数:20( 用户输入数字)您猜小了，这个数字比20大:30.您猜大了，这个数字比30小:25.恭喜您猜对了，这个数字为:25

```c#
namespace MultiplicationTable
{
    class Program
    {
        static void Main()
        {
            
            bool is_read = false;
            int counter = 0;
            while (!is_read)
            {
                //让这个语句只执行一次
                if(counter == 0)
                {
                    Console.WriteLine("请你猜猜我这个数据是多少涅？");
                    counter++;
                }
                else
                {
                    Console.WriteLine("不对捏，再猜");
                }
               
                //读入一个int类型数据
                int readS = Convert.ToInt32(Console.ReadLine());
                //猜的数据
                int guess = 61;

                if (readS == guess)
                {
                    Console.WriteLine("你猜对了");
                    is_read = true;
                }else if(readS > guess) {
                    Console.WriteLine("输入的数据比目标数据大{0}", Math.Abs(guess - readS));
                }
                else
                {
                    Console.WriteLine("输入的数据比目标数据小{0}", Math.Abs(guess - readS));
                }

            }
            Console.ReadKey();
           
        }
    }
}
```

```c#
//随机数版本
namespace MultiplicationTable
{
    class Program
    {
        static void Main()
        {
            bool isGuessed = false;
            int guess = new Random().Next(1, 101); // 生成1到100之间的随机数作为目标数据

            while (!isGuessed)
            {
                int readS;
                Console.WriteLine("请你猜猜我这个数据是多少涅？");
                if (int.TryParse(Console.ReadLine(), out readS)) // 输入验证
                {
                    if (readS == guess)
                    {
                        Console.WriteLine("你猜对了");
                        isGuessed = true;
                    }
                    else if (readS > guess)
                    {
                        Console.WriteLine("输入的数据比目标数据大{0}", readS - guess);
                    }
                    else
                    {
                        Console.WriteLine("输入的数据比目标数据小{0}", guess - readS);
                    }
                }
                else
                {
                    Console.WriteLine("输入的不是有效的整数，请重新输入");
                }
            }

            Console.ReadKey();
        }
    }
}

```

### 排序

编写一个控制台程序，要求用户输入一组数字，对用户输入的数字从小到大输出。

```c#
using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("请输入一组数字（以逗号分隔）：");
        string input = Console.ReadLine();

        string[] numbers = input.Split(',');
        int[] arr = new int[numbers.Length];

        for (int i = 0; i < numbers.Length; i++)
        {
            int.TryParse(numbers[i], out arr[i]);
        }

        Array.Sort(arr);

        Console.WriteLine("从小到大排序后的数字为：");
        foreach (int num in arr)
        {
            Console.Write(num + " ");
        }

        Console.ReadKey();
    }
}
```

### 猴子吃桃

每天的桃子数量是前一天桃子数量加1后乘以2的结果。

```c#
using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("请输入天数 n 的值：");
        int n = Convert.ToInt32(Console.ReadLine());

        int peachCount = CalculatePeaches(n);

        Console.WriteLine("悟空第一天吃桃子的数量为：" + peachCount);
        Console.ReadKey();
    }

    static int CalculatePeaches(int n)
    {
        int peachCount = 1;

        for (int i = n - 1; i >= 1; i--)
        {
            peachCount = (peachCount + 1) * 2;
        }

        return peachCount;
    }
}

```

### 找最小

输入n(n<100)个数，找出其中最小的数，将它与最前面的数交换后输出这些数

```c#
using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("请输入要输入的数字个数 n（n < 100）：");
        int n = Convert.ToInt32(Console.ReadLine());

        int[] numbers = new int[n];

        Console.WriteLine("请输入这 " + n + " 个数字：");
        for (int i = 0; i < n; i++)
        {
            numbers[i] = Convert.ToInt32(Console.ReadLine());
        }

        int minIndex = FindMinimumIndex(numbers);

        if (minIndex != 0)
        {
            Swap(numbers, 0, minIndex);
        }

        Console.WriteLine("交换后的数字为：");
        foreach (int num in numbers)
        {
            Console.Write(num + " ");
        }

        Console.ReadKey();
    }

    static int FindMinimumIndex(int[] arr)
    {
        int minIndex = 0;
        int minValue = arr[0];

        for (int i = 1; i < arr.Length; i++)
        {
    if (arr[i] < minValue)
            {
                minIndex = i;
                minValue = arr[i];
            }
        }

        return minIndex;
    }

    static void Swap(int[] arr, int index1, int index2)
    {
        int temp = arr[index1];
        arr[index1] = arr[index2];
        arr[index2] = temp;
    }
}
```

### 发工资

作为泰课的老师，最盼望的日子就是每月的8号了，因为这一天是发工资的日子，养家糊口就它了，呵呵但是对子素则务处的工作人员来说，这一天是很忙品的天,对务处的小云最近就在考海一个同题:如果每个老所的工资额都和道，最少需要备多少张人民币，才能在治年位老师发工路的时候都不用老师战零呢这里假设老师的工资都是正整数，单位元，人民币一共有100元、50元、10元，5元、2元和1元六种

```c#
using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("请输入老师的人数：");
        int teacherCount = Convert.ToInt32(Console.ReadLine());

        Console.WriteLine("请输入每位老师的工资额（以空格分隔）：");
        string input = Console.ReadLine();
        string[] salaryStrArray = input.Split(' ');
        int[] salaries = new int[teacherCount];

        for (int i = 0; i < teacherCount; i++)
        {
            salaries[i] = Convert.ToInt32(salaryStrArray[i]);
        }

        int minimumBills = CalculateMinimumBills(salaries);

        Console.WriteLine("最少需要备 " + minimumBills + " 张人民币。");
        Console.ReadKey();
    }

    static int CalculateMinimumBills(int[] salaries)
    {
        int[] denominations = { 100, 50, 10, 5, 2, 1 };
        int minimumBills = 0;

        foreach (int salary in salaries)
        {
            int remainingSalary = salary;
            for (int i = 0; i < denominations.Length; i++)
            {
                minimumBills += remainingSalary / denominations[i];
                remainingSalary %= denominations[i];
            }
        }

        return minimumBills;
    }
}

```

### 判断是否合法

```c#
using System;
using System.Text.RegularExpressions;

public class Program
{
    public static void Main()
    {
        // 从用户输入中获取标识符
        Console.WriteLine("请输入一个标识符:");
        string input = Console.ReadLine();

        // 使用正则表达式检查标识符的合法性
        bool isValidIdentifier = Regex.IsMatch(input, @"^[a-zA-Z_][a-zA-Z0-9_]*$");

        // 输出结果
        if (isValidIdentifier)
        {
            Console.WriteLine("输入的字符串是一个合法的C#标识符");
        }
        else
        {
            Console.WriteLine("输入的字符串不是一个合法的C#标识符");
        }
    }
}

```

> 正则表达式：
>
>

## c#面向对象

面向对象编程（Object-Oriented Programming，简称OOP）是一种编程范式，它将程序设计问题划分为对象的集合，这些对象通过相互之间的交互来解决问题。在面向对象编程中，程序被组织成一组对象，每个对象都是类的实例，类定义了对象的属性（数据）和行为（方法）。

面向对象编程的核心思想是将真实世界中的事物抽象成为程序中的对象，每个对象都具有自己的状态和行为。对象可以通过发送消息（调用方法）来与其他对象进行交互，从而实现数据的封装、继承和多态性等特性。

面向对象编程具有以下特点：

1. 封装（Encapsulation）：将数据和操作封装在对象中，对象对外部提供有限的接口来访问和修改其内部状态，隐藏了实现细节，增强了安全性和模块化。
2. 继承（Inheritance）：通过继承机制，一个类可以派生出子类，子类继承了父类的属性和方法，并可以在此基础上进行扩展或修改，实现代码的重用和扩展。
3. 多态（Polymorphism）：多态性允许对象根据上下文的不同呈现不同的行为。通过多态，可以通过父类类型引用来引用子类对象，并根据实际对象类型来调用相应的方法。

面向对象编程能够提供更加模块化、可扩展和可维护的代码，通过将问题分解成对象的方式来组织代码结构，使得代码更易于理解和重用。它被广泛应用于许多编程语言和软件开发领域。

### 命名空间

在C#中，命名空间（Namespace）用于组织和管理代码，提供了一种避免命名冲突和代码组织的机制。命名空间可以包含类、结构、枚举、接口和其他命名空间等元素。

以下是一些命名空间的作用：

1. **避免命名冲突**：命名空间提供了一种将相关的类、结构和其他元素组织在一起的方式。不同命名空间中的同名类或成员不会发生冲突，可以在代码中使用完全限定名（包含命名空间）来区分它们。
2. **代码组织和模块化**：命名空间可以按照逻辑关系将代码组织成模块，提高代码的可读性和可维护性。通过使用命名空间，可以将相关的类和功能放在一起，使代码更加结构化。
3. **访问控制和可见性**：命名空间可以定义公共（public）和非公共（internal、private）的元素，从而控制代码的可见性和访问级别。通过将类和成员放置在适当的命名空间中，并使用适当的访问修饰符，可以限制对代码的访问。
4. **代码重用和组件化**：命名空间可以用于组织和管理代码库中的各个组件。可以通过引用相应的命名空间来重用其他代码库中的类和功能，提高代码的重用性和可维护性。
5. **名称空间别名**：命名空间还可以使用别名来简化代码中的引用。通过使用`using`关键字并提供一个别名，可以在代码中使用简短的名称来引用长命名空间，减少代码的冗余性和书写工作。

例如，以下是一个使用命名空间的示例：

```c#
csharpCopy codeusing System;

namespace MyApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");
        }
    }
}
```

在上述示例中，`using System`语句引入了名为`System`的命名空间，其中包含了`Console`类。这样就可以直接使用`Console.WriteLine`方法而不需要使用完全限定名。

总而言之，命名空间在C#中提供了一种组织、管理和控制代码的机制，可以提高代码的可读性、可维护性和可重用性，并帮助避免命名冲突。

### 封装

封装（Encapsulation）是面向对象编程中的一个重要概念，它指的是将数据和操作封装在一个单元（如类）中，以实现信息隐藏和安全性。

封装通过将相关的数据和操作组合在一起形成一个单元，将其视为一个独立的实体，并对外部隐藏其内部实现细节。这意味着外部代码无法直接访问和修改封装单元的数据，而是通过定义的公共接口来与其进行交互。

类是实现封装的主要方式之一。在面向对象编程中，类是一个封装数据和方法的模板。它将数据（成员变量）和操作（成员方法）组合在一起，形成一个独立的实体。

通过使用访问修饰符（如`public`、`private`、`protected`等），类可以控制成员的可访问性。私有成员只能在类的内部访问，而公共成员可以被外部代码访问。这种访问控制机制实现了封装的一部分，通过隐藏内部实现细节，提高了代码的安全性和可维护性。

封装的优点包括：

1. 数据隐藏：封装可以隐藏类的内部数据，防止外部直接访问和修改，从而确保数据的一致性和完整性。
2. 安全性：通过限制对类的访问权限，可以控制外部代码对类的操作，提高安全性。
3. 简化接口：封装通过定义公共接口，隐藏内部实现细节，使外部代码只需要关注如何使用接口而不需要了解内部实现。
4. 代码重用：封装可以将相关的数据和方法组合在一起，形成可重用的类，提高代码的可维护性和可重用性。
5. 隔离变化：封装可以隔离类的内部实现细节，当需要修改内部实现时，只需修改类的内部，而不影响外部代码。

#### 类

在C#中，类（Class）是一种用户自定义的数据类型，它定义了对象的属性（数据成员）和行为（成员方法）。类是面向对象编程的核心概念之一，它允许开发人员将相关的数据和功能封装在一个单独的实体中。

类可以看作是对象的蓝图或模板，用于创建具体的对象实例。每个对象实例都基于类的定义，并拥有自己的一组属性和行为。

类的定义通常包含以下组成部分：

1. 类名：类的名称，用于标识和引用该类。
2. 数据成员：用于存储对象的状态或属性的变量。这些成员可以是各种数据类型，如整数、浮点数、字符串等。
3. 成员方法：用于定义对象的行为和功能的函数。成员方法可以访问和操作数据成员，并执行其他操作。

以下是一个简单的C#类的示例：

```c#
csharpCopy codepublic class Person
{
    // 数据成员
    public string Name;
    public int Age;

    // 成员方法
    public void SayHello()
    {
        Console.WriteLine("Hello, my name is " + Name + " and I am " + Age + " years old.");
    }
}
```

在上面的示例中，定义了一个名为"Person"的类，它具有两个数据成员：Name（姓名）和Age（年龄），以及一个成员方法SayHello（打招呼），用于输出一个简单的问候语。使用该类，可以创建多个Person对象的实例，并访问它们的属性和方法。

```c#
csharpCopy codePerson person1 = new Person();
person1.Name = "John";
person1.Age = 25;
person1.SayHello();

Person person2 = new Person();
person2.Name = "Alice";
person2.Age = 30;
person2.SayHello();
```

上述代码创建了两个Person对象的实例，分别设置它们的姓名和年龄，并调用它们的SayHello方法输出问候语。

> 类访问修饰符号：
>
> 在C#中，类的访问修饰符用于控制类的可见性和访问级别。C#提供了以下几种类的访问修饰符：
>
> 1. **public**：公共访问修饰符，表示该类对于所有代码都是可见的。可以从任何位置访问该类。
> 2. **private**：私有访问修饰符，表示该类仅对于当前类内部可见。其他类无法直接访问私有类。
> 3. **protected**：受保护访问修饰符，表示该类对于当前类及其派生类可见。其他类无法直接访问受保护类。
> 4. **internal**：内部访问修饰符，表示该类对于当前程序集内的其他类可见。跨程序集时无法访问内部类。
> 5. **protected internal**：受保护内部访问修饰符，表示该类对于当前程序集内的其他类及其派生类可见。跨程序集时无法访问受保护内部类。
>
> 这些访问修饰符可以应用于类的定义，以控制类的可见性和访问级别。默认情况下，如果没有显式指定访问修饰符，类的访问级别为`internal`（内部）。
>
> 以下是一些示例：
>
> ```c#
> csharpCopy codepublic class PublicClass
> {
>     // 可以从任何位置访问PublicClass
> }
> 
> private class PrivateClass
> {
>     // 仅对当前类内部可见
> }
> 
> protected class ProtectedClass
> {
>     // 仅对当前类及其派生类可见
> }
> 
> internal class InternalClass
> {
>     // 仅对当前程序集内的其他类可见
> }
> 
> protected internal class ProtectedInternalClass
> {
>     // 对当前程序集内的其他类及其派生类可见
> }
> ```
>
> 请注意，类的访问修饰符还可以与其他修饰符（如静态修饰符`static`、抽象修饰符`abstract`等）一起使用，以提供更多的控制和灵活性。
>
> **tip: 在类中如果声明的时候没有加访问修饰符，一般默认为private**

##### 类的构造函数

当提及C#语言时，类的构造函数是通过使用特定的构造函数语法来定义的。在C#中，构造函数与类的名称相同，并且可以包含参数。以下是一个简单的C#类和构造函数的示例：

```c#
csharpCopy codepublic class MyClass
{
    // 无参数构造函数
    public MyClass()
    {
        // 构造函数的代码逻辑
    }

    // 带参数的构造函数
    public MyClass(string name, int age)
    {
        // 构造函数的代码逻辑
    }
}
```

在上述示例中，`MyClass`类具有两个构造函数。第一个构造函数是无参数的构造函数，它不接受任何参数。第二个构造函数是带有两个参数（`name`和`age`）的构造函数。

你可以根据实际需要在构造函数中执行各种初始化任务，例如设置对象的初始状态、分配内存、初始化成员变量等。

在使用C#时，你可以根据需要选择使用适当的构造函数，并在创建类的实例时自动调用相应的构造函数。

> **在C#中，有参构造函数不会直接代替无参构造函数。当一个类定义了有参构造函数时，如果没有显式定义无参构造函数，编译器将不会为类自动生成默认的无参构造函数。**
>
> 无参构造函数（也称为默认构造函数）是一个没有参数的构造函数，它在创建类的新实例时使用。如果没有显式定义无参构造函数，编译器会自动为类生成一个默认的无参构造函数。无参构造函数通常用于执行类的基本初始化操作，例如将成员变量设置为默认值。
>
> 有参构造函数是一个带有一个或多个参数的构造函数，它允许在创建对象时提供初始化所需的值。有参构造函数可以接受参数，并用这些参数对类的成员进行初始化。
>
> 如果一个类定义了有参构造函数，而没有显式定义无参构造函数，那么在创建类的实例时，必须使用有参构造函数并提供必要的参数。如果尝试使用无参构造函数创建对象，编译器将报错。
>
> 然而，如果需要在类中同时支持有参构造函数和无参构造函数，可以通过**重载构造函数的方式实现。重载构造函数允许在一个类中定义多个构造函数，每个构造函数有不同的参数列表。在重载构造函数中，可以在参数列表不同的构造函数中实现不同的初始化逻辑。**
>
> 以下是一个示例，演示了有参构造函数和无参构造函数的使用：
>
> ```c#
> csharpCopy codeclass MyClass
> {
>     private int myVariable;
> 
>     // 有参构造函数
>     public MyClass(int value)
>     {
>         myVariable = value;
>     }
> 
>     // 无参构造函数
>     public MyClass()
>     {
>         myVariable = 0;
>     }
> 
>     public void DisplayVariable()
>     {
>         Console.WriteLine("变量的值为：" + myVariable);
>     }
> }
> 
> class Program
> {
>     static void Main()
>     {
>         MyClass myObject1 = new MyClass(10);
>         myObject1.DisplayVariable(); // 输出：变量的值为：10
> 
>         MyClass myObject2 = new MyClass();
>         myObject2.DisplayVariable(); // 输出：变量的值为：0
>     }
> }
> ```
>
> 在上述示例中，**`MyClass` 类定义了一个有参构造函数和一个无参构造函数。有参构造函数接受一个整数参数，并将其用于初始化私有变量 `myVariable`。无参构造函数将 `myVariable` 设置为默认值 0。**
>
> 通过在创建对象时选择适当的构造函数，我们可以使用有参构造函数或无参构造函数来初始化类的实例，并执行相应的初始化操作。这样，无参构造函数和有参构造函数可以在不同的情况下使用。

##### 类的析构函数

在C#中，析构函数（Destructor）是一种特殊的方法，用于在对象被销毁之前执行清理操作。析构函数通常用于释放对象占用的资源，例如关闭文件、释放内存、关闭数据库连接等。

在C#中，析构函数使用特殊的语法进行定义，其名称与类的名称相同，但前面加上一个波浪线（~）。析构函数不能有参数，也不能被显式地调用，而是由垃圾回收器自动在对象被销毁时调用。

以下是一个简单的C#类和析构函数的示例：

```c#
public class MyClass
{
    // 构造函数
    public MyClass()
    {
        // 构造函数的代码逻辑
    }

    // 析构函数
    ~MyClass()
    {
        // 析构函数的代码逻辑，执行清理操作
    }
}
```

在上述示例中，`MyClass`类具有一个构造函数和一个析构函数。当对象被销毁时，垃圾回收器会自动调用析构函数，以执行清理操作。

需要注意的是，**C#的垃圾回收器会自动管理对象的内存，因此在大多数情况下，你不需要显式地定义和使用析构函数。析构函数通常用于释放非托管资源（如文件句柄或数据库连接），在使用这些资源时可能需要手动清理。**对于托管资源，垃圾回收器会自动进行垃圾回收和内存释放。

但是，如果确实有需要使用析构函数进行清理操作的情况，可以通过析构函数来实现。

#### 属性

在C#中，属性（Property）是一种特殊的成员，用于封装类的字段（Field）并提供对其读取和写入的访问方法。属性允许你定义类的外部代码如何访问和操作类的数据。

属性通过使用`get`和`set`访问器来定义读取和写入操作。`get`访问器用于获取属性的值，而`set`访问器用于设置属性的值。属性可以具有不同的访问修饰符，例如`public`、`private`、`protected`等，用于控制属性的可见性。

以下是一个简单的C#类和属性的示例：

```c#
public class Person
{
    private string name;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }
}
```

在上述示例中，`Person`类有一个名为`Name`的属性。属性的类型是`string`，并且使用了默认的访问修饰符`public`。属性对应的字段是`name`，它被`private`修饰，只能在类的内部访问。

**通过定义属性，可以通过以下方式读取和写入属性的值：**

```c#
Person person = new Person();
person.Name = "John"; // 设置属性的值
string name = person.Name; // 获取属性的值
```

在上述示例中，我们使用属性来设置和获取`Person`对象的`Name`属性的值。

属性提供了一种更加直观和易于使用的方式来访问类的字段，并且可以对字段的读取和写入进行额外的逻辑处理，例如数据验证、计算等。属性使得代码更具可读性、可维护性和安全性。

> get访问器：
>
> 当定义属性时，`get` 访问器用于获取属性的值。它指定了在访问属性时应该执行的代码逻辑。`get` 访问器没有参数，并且返回与属性类型相匹配的值。
>
> 以下是 `get` 访问器的基本语法：
>
> ```c#
> public <type> PropertyName
> {
>     get
>     {
>         // 返回属性的值的代码逻辑
>     }
> }
> ```
>
> 在 `<type>` 中，你需要指定属性的类型。代码逻辑部分位于 `get` 访问器的花括号中，用于返回属性的值。
>
> 让我们看一个示例，假设我们有一个 `Person` 类，其中包含 `name` 字段和 `Name` 属性，我们使用 `get` 访问器获取 `Name` 属性的值：
>
> ```c#
> public class Person
> {
>     private string name;
> 
>     public string Name
>     {
>         get
>         {
>             return name;
>         }
>     }
> }
> ```
>
> 在上述示例中，`Name` 属性的 `get` 访问器简单地返回 `name` 字段的值。
>
> 当我们创建 `Person` 对象时，我们可以通过调用 `Name` 属性来获取 `name` 字段的值：
>
> ```c#
> Person person = new Person();
> person.Name = "John"; // 这里是错误的，因为 Name 属性只定义了 get 访问器
> string name = person.Name; // 获取属性的值
> Console.WriteLine(name); // 输出 "John"
> ```
>
> 请注意，由于属性只定义了 `get` 访问器，我们不能使用 `person.Name = "John"` 这样的语法为属性赋值，这将导致编译错误。只有当属性同时定义了 `get` 和 `set` 访问器时，我们才可以读取和写入属性的值。
>
> 通过使用 `get` 访问器，我们可以实现对属性的封装，并在需要时提供定制的获取逻辑，例如对属性进行计算、验证或其他处理。
> `set` 访问器:
>
> 当定义属性时，`set` 访问器用于设置属性的值。它指定了在给属性赋值时应该执行的代码逻辑。`set` 访问器接受一个参数，用于传递要设置的值。
>
> 以下是 `set` 访问器的基本语法：
>
> ```c#
> public <type> PropertyName
> {
>     get
>     {
>         // 返回属性的值的代码逻辑
>     }
>     set
>     {
>         // 设置属性的值的代码逻辑
>     }
> }
> ```
>
> 在 `<type>` 中，你需要指定属性的类型。`set` 访问器包含一个名为 `value` 的特殊参数，用于传递要设置的值。你可以在 `set` 访问器的代码逻辑中使用 `value` 参数来设置属性的值。
>
> 让我们看一个示例，假设我们有一个 `Person` 类，其中包含 `name` 字段和 `Name` 属性，我们使用 `get` 和 `set` 访问器来获取和设置 `Name` 属性的值：
>
> ```c#
> public class Person
> {
>     private string name;
> 
>     public string Name
>     {
>         get
>         {
>             return name;
>         }
>         set
>         {
>             name = value;
>         }
>     }
> }
> ```
>
> 在上述示例中，`Name` 属性定义了 `get` 和 `set` 访问器。`get` 访问器返回 `name` 字段的值，而 `set` 访问器将传递的值赋给 `name` 字段。
>
> 当我们创建 `Person` 对象时，我们可以通过调用 `Name` 属性来获取和设置 `name` 字段的值：
>
> ```c#
> Person person = new Person();
> person.Name = "John"; // 设置属性的值
> string name = person.Name; // 获取属性的值
> Console.WriteLine(name); // 输出 "John"
> ```
>
> 通过使用 `set` 访问器，我们可以在为属性赋值时执行验证、计算或其他逻辑。我们可以根据需要自定义 `set` 访问器的行为，例如检查传递的值是否满足某些条件，然后再决定是否设置属性的值。

> 属性（Property）和变量（Variable）在 C# 中有几个重要的区别：
>
> 1. 访问方式：属性提供了对类的字段的封装访问方式，通过使用 get 和 set 访问器来读取和修改属性的值。而变量可以直接读取和修改其存储的值。
> 2. 封装性和控制：属性允许你在访问器中添加额外的逻辑，以对数据进行验证、处理或实施封装策略。这使得属性更加灵活和可控。而变量没有这种封装和控制的额外层面。
> 3. 语法：属性使用特定的语法来定义，包含 get 和 set 访问器。变量则是通过简单的声明来定义。
> 4. 外部可见性：属性可以具有不同的可见性修饰符，可以控制属性的访问级别。变量的可见性取决于其所在的作用域。
> 5. 命名约定：属性通常使用 PascalCase 命名约定，以强调它们的行为和封装性。变量通常使用 camelCase 命名约定。
>
> 总的来说，属性提供了更好的封装性、控制和灵活性，使得对类中的字段的访问和操作更加可控和一致。变量则是简单的数据存储容器，可以直接访问和修改其值。你可以根据具体的需求和设计目标选择使用属性或变量。

#### 匿名类型

匿名类型是使用 `var` 关键字声明的一种特殊类型，它在编译时期由编译器自动推断出来。匿名类型是具有只读属性的类，它的属性名称和类型是根据初始化时提供的属性和值自动生成的。

匿名类型的语法如下所示：

```c#
csharpCopy codevar anonymousObject = new
{
    Property1 = value1,
    Property2 = value2,
    // ...
};
```

在上述示例中，`anonymousObject` 是使用 `var` 声明的匿名类型变量。通过使用 `new` 关键字和对象初始化器的方式，我们可以创建一个匿名类型对象，并指定一组属性和对应的值。

匿名类型的属性是只读的，即不能在运行时修改其值。编译器为匿名类型生成一个类，并自动为每个属性创建一个只读的公共属性。匿名类型的属性名称和类型在编译时期确定，而不是在运行时。

匿名类型主要用于临时存储和传递一组相关的数据，通常与LINQ查询一起使用，以方便地处理查询结果。由于匿名类型没有显式的类定义，它们在编译时期是未知的，因此不能将匿名类型直接转换为其他类型。如果需要将匿名类型的数据转换为其他类型，可以使用显式类型转换或将其属性值逐个提取到新的对象中。

请注意，匿名类型的作用域通常被限制在定义它们的方法或代码块内部。超出作用域后，匿名类型将不再可用。

> 匿名类型在C#中是通过 `new` 关键字创建的，它可以是结构体（struct）或类（class）的实例。实际上，编译器会根据匿名类型的属性数量和类型来决定生成结构体还是类。
>
> 当匿名类型的属性都是值类型（如整数、字符串等）时，编译器会生成一个结构体作为匿名类型的实例。例如：
>
> ```c#
> csharpCopy codevar anonymousObject = new
> {
>     Property1 = 42,
>     Property2 = "Hello"
> };
> ```
>
> 在上述示例中，由于 `Property1` 是整数类型，`Property2` 是字符串类型，编译器会生成一个结构体作为匿名类型的实例。
>
> 当匿名类型的属性包含引用类型（如自定义类、数组等）时，编译器会生成一个类作为匿名类型的实例。例如：
>
> ```c#
> csharpCopy codevar anonymousObject = new
> {
>     Property1 = new MyClass(),
>     Property2 = new int[] { 1, 2, 3 }
> };
> ```
>
> 在上述示例中，`Property1` 是一个自定义类的实例，`Property2` 是一个整数数组，因此编译器会生成一个类作为匿名类型的实例。
>
> 无论是结构体还是类，匿名类型都是只读的，不能在运行时修改其属性的值。匿名类型的属性名称和类型是在编译时期确定的。
>
> 需要注意的是，由于匿名类型是在编译时期生成的，所以它们的具体类型名称是不可知的，并且不能将匿名类型直接转换为其他类型。如果需要将匿名类型的数据转换为其他类型，可以使用显式类型转换或将其属性值逐个提取到新的对象中。

#### 堆和栈

在C#中，堆（Heap）和栈（Stack）是两种用于存储和管理内存的重要概念。

栈（Stack）：

- 栈是一种用于存储局部变量和方法调用的内存区域。
- 栈中的数据以“后进先出”（LIFO）的方式进行操作。
- 栈的操作非常高效，因为它使用简单的指针操作来管理内存。
- 当你调用一个方法时，该方法的局部变量和参数将被分配到栈上。当方法执行完毕，栈上的数据将被释放。

堆（Heap）：

- 堆是一种用于存储动态分配的对象的内存区域。
- 堆中的数据可以以任意顺序进行操作，没有特定的顺序要求。
- 堆的操作相对较慢，因为它需要进行内存分配和垃圾回收。
- 当你使用 `new` 关键字创建一个对象时，该对象将被分配到堆上。堆上的对象可以长时间存在，直到垃圾回收器将其标记为不再使用。

在C#中，基本数据类型（如整数、浮点数、布尔值等）和引用类型（如类、接口、数组等）的存储位置有所不同：

- 基本数据类型通常存储在栈上。当你声明一个基本数据类型的变量并为其赋值时，数据将直接存储在栈上。这些变量的生命周期在其作用域结束时就会结束。
- 引用类型的变量本身存储在栈上，但实际的对象存储在堆上。当你声明一个引用类型的变量并为其赋值时，变量本身存储在栈上，而对象存储在堆上。对象的生命周期由垃圾回收器来管理，垃圾回收器会自动检测不再使用的对象并进行回收。

需要注意的是，通过传递引用类型变量作为参数或在方法中创建对象时，引用类型的对象可能存储在堆上或栈上。这取决于具体的情况和编译器的优化策略。

总结起来，栈用于存储局部变量和方法调用，它的操作效率高；堆用于存储动态分配的对象，它的操作相对较慢。在C#中，栈和堆都是用于管理内存的重要部分，理解它们的运行逻辑有助于编写高效和可靠的代码。

#### 值类型和引用类型

在C#中，变量可以是值类型（Value Types）或引用类型（Reference Types）。这两种类型在内存中的存储方式和行为有所不同。

值类型（Value Types）：

- 值类型的变量直接包含其数据的实际值。
- 值类型的变量在栈上分配内存。
- 值类型的赋值是通过将一个变量的值复制到另一个变量来完成的。
- 值类型的变量通常具有固定的大小，由编译时确定。
- 值类型包括整数类型（如int、float、double等）、布尔类型（bool）、字符类型（char）、结构体（struct）等。

下面是一个值类型的示例：

```c#
csharpCopy codeint x = 10; // 值类型变量 x，存储在栈上
int y = x; // 将 x 的值复制给 y
y = 20; // 修改 y 不会影响 x 的值
Console.WriteLine(x); // 输出 10
Console.WriteLine(y); // 输出 20
```

引用类型（Reference Types）：

- 引用类型的变量存储的是对象的引用，而不是对象的实际数据。
- 引用类型的变量在栈上分配内存，但对象本身存储在堆上。
- 引用类型的赋值是通过将一个变量的引用复制到另一个变量来完成的，这样两个变量将引用同一个对象。
- 引用类型的大小是固定的，不受对象数据大小的影响，因为变量本身只是引用。
- 引用类型包括类（class）、接口（interface）、委托（delegate）、字符串（string）等。

下面是一个引用类型的示例：

```c#
int[] array1 = new int[] { 1, 2, 3 }; // 引用类型变量 array1，存储在栈上，对象存储在堆上
int[] array2 = array1; // 将 array1 的引用复制给 array2
array2[0] = 10; // 修改 array2 也会影响 array1 中的值
Console.WriteLine(array1[0]); // 输出 10
Console.WriteLine(array2[0]); // 输出 10
```

需要注意的是，值类型的赋值是将值复制给另一个变量，而引用类型的赋值是复制引用。这意味着修改引用类型变量的值会影响到其他引用同一对象的变量，因为它们引用的是同一个对象。

在实际编程中，理解值类型和引用类型的区别很重要，因为它们的存储和传递方式不同，可能会对程序的行为产生影响。

> **值类型和引用类型在内存中的存储原理有所不同。**
>
> 值类型是指基本数据类型（如整数、浮点数、布尔值等）以及结构体（struct）等，它们的值直接存储在栈（stack）内存中。栈是一种后进先出（LIFO）的数据结构，用于存储局部变量和方法调用的上下文。当一个值类型的变量被声明时，系统会为其分配一块栈内存，并将值直接存储在这块内存中。当变量超出其作用域时，栈内存会被自动释放。
>
> 引用类型是指类（class）、接口（interface）、数组（array）以及字符串（string）等，它们的值存储在堆（heap）内存中。堆是一种动态分配的内存区域，用于存储对象和数据结构。当一个引用类型的变量被声明时，系统会为其分配一块堆内存，并将变量的引用（即指向该对象或数据结构的内存地址）存储在栈内存中。引用类型的变量实际上存储的是对象或数据结构在堆内存中的地址，通过这个地址可以找到实际存储的值。当变量超出其作用域时，只有栈内存中的引用被释放，而堆内存中的实际对象或数据结构需要等待垃圾回收器（garbage collector）进行回收。
>
> 这种区别在于内存的管理方式。值类型的存储是直接的，而引用类型的存储则是通过引用来访问实际的数据。这也意味着对于值类型的操作是在栈上进行的，而对于引用类型的操作需要通过引用来查找并操作堆内存中的数据。
>
> **在C#中，值类型和引用类型在内存中的存储原理与前面描述的概念基本相同。**
>
> 对于值类型，在C#中包括基本数据类型（如整数、浮点数、布尔值等）以及结构体（struct）。这些值类型的实例直接存储在栈内存中，而不是堆内存。当值类型的变量被声明时，内存会分配在栈上，并将值直接存储在该栈内存中。当变量超出其作用域时，栈内存会自动释放。
>
> 引用类型在C#中包括类（class）、接口（interface）、数组（array）以及字符串（string）。这些引用类型的实例存储在堆内存中。当引用类型的变量被声明时，内存会分配在栈上，并将引用（即指向堆内存中对象的地址）存储在栈内存中。实际的对象或数据结构则存储在堆内存中。当变量超出其作用域时，栈内存中的引用会被释放，但堆内存中的对象或数据结构会等待垃圾回收器进行回收。
>
> 需要注意的是，**C#中的引用类型还涉及到垃圾回收的机制**。垃圾回收器会定期检查堆内存中的对象，释放不再被引用的对象，并回收其所占用的内存。这可以减轻开发者的内存管理负担，但也意味着无法精确控制对象的生命周期和内存释放的时机，因此在某些情况下可能会出现性能问题。

### 继承

在C#中，类可以通过继承来扩展和重用现有的类。继承是面向对象编程中的一个重要概念，它允许你创建一个新类，从一个或多个现有的类中派生，继承其属性和方法。

**要在C#中实现类的继承，你可以使用冒号（:）符号来指定一个类派生自另一个类。派生类（子类）继承基类（父类）的成员，并可以添加自己的成员或覆盖基类的成员。**

下面是一个简单的示例，演示了如何在C#中实现类的继承：

```c#
// 基类
public class Animal
{
    public void Eat()
    {
        Console.WriteLine("Animal is eating.");
    }
}

// 派生类
public class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("Dog is barking.");
    }
}

// 使用继承的示例
class Program
{
    static void Main(string[] args)
    {
        Dog dog = new Dog();
        dog.Eat();  // 继承自基类 Animal
        dog.Bark(); // 派生类 Dog 自己的方法

        Console.ReadLine();
    }
}
```

在上面的示例中，`Animal`类是基类，`Dog`类是派生类。`Dog`类继承了`Animal`类的`Eat()`方法，并添加了自己的`Bark()`方法。在`Main`方法中，我们创建了`Dog`的实例，并可以调用继承的方法和自己的方法。

继承的优点是可以实现代码的重用和层次化设计。派生类可以访问基类的非私有成员（公有、受保护或受内部可见性约束的成员），并且可以通过方法覆盖来修改或扩展基类的行为。

注意：在C#中，一个类只能直接继承自一个基类，即C#不支持多重继承。但是，你可以通过接口来实现类似多重继承的效果。

> 在使用继承时，有一些需要注意的事项：
>
> 1. 类之间的逻辑关系：继承应该基于"是一个"的关系，即派生类是基类的一种特化或扩展。确保在建立继承关系时，派生类能够满足基类的行为和属性，遵循类之间的逻辑关系。
> 2. 单一继承原则：C#中只支持单一继承，一个类只能直接继承自一个基类。这意味着你需要谨慎选择基类，以确保派生类能够获取所需的功能。
> 3. 虚方法和方法重写：当使用继承时，如果你希望在派生类中重写基类的方法，可以将基类中的方法声明为`virtual`，然后在派生类中使用`override`来进行重写。这样可以实现多态性，确保在运行时根据对象的实际类型调用正确的方法。
> 4. 访问修饰符的考虑：继承关系中，派生类对于基类的成员的访问权限取决于成员的访问修饰符。如果基类的成员是私有的（private），则派生类无法直接访问。如果基类的成员是受保护的（protected），则派生类可以直接访问。如果基类的成员是公共的（public），则派生类可以直接访问。
> 5. 构造函数的继承：派生类默认会调用基类的无参构造函数，如果基类没有无参构造函数，则需要在派生类中显式调用基类的有参构造函数。
> 6. 基类和派生类的生命周期管理：在继承关系中，需要注意基类和派生类对象的创建、销毁和生命周期管理。确保在适当的时候释放资源，避免内存泄漏等问题。
> 7. 避免过度继承：过度的继承可能导致类之间的耦合性增加，难以维护和扩展。在设计类继承关系时，尽量遵循单一责任原则，保持继承关系的简洁和清晰。

#### 虚方法

虚方法是C#中用于实现多态性的一种特殊类型的方法。通过将方法声明为虚方法，可以在派生类中重写该方法，实现基于对象的实际类型来调用相应的方法。

在C#中，使用`virtual`关键字来声明一个方法为虚方法。以下是一个示例：

```c#
public class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("The animal makes a sound.");
    }
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("The dog barks.");
    }
}

public class Cat : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("The cat meows.");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Animal animal = new Animal();
        animal.MakeSound();  // 输出："The animal makes a sound."

        Dog dog = new Dog();
        dog.MakeSound();     // 输出："The dog barks."

        Cat cat = new Cat();
        cat.MakeSound();     // 输出："The cat meows."

        Animal polymorphicAnimal = new Dog();
        polymorphicAnimal.MakeSound();  // 输出："The dog barks."

        Console.ReadLine();
    }
}
```

在上面的示例中，`Animal`类中的`MakeSound()`方法被声明为虚方法，允许派生类进行方法的重写。`Dog`和`Cat`类分别重写了基类中的`MakeSound()`方法，提供了它们自己的实现。

在`Main`方法中，我们创建了`Animal`、`Dog`和`Cat`的实例，并分别调用它们的`MakeSound()`方法。此外，我们还创建了一个`Animal`类型的变量，但将其实际引用为`Dog`类的实例。这体现了多态性，当我们调用`MakeSound()`方法时，实际执行的是`Dog`类中重写的方法。

通过使用虚方法和方法重写，我们可以根据具体对象的类型来决定调用哪个方法，实现了运行时多态性的特性。这样的设计模式能够提高代码的灵活性和可扩展性，使得程序能够适应不同类型的对象，并根据对象的实际行为进行相应的处理。

#### 隐藏方法

隐藏方法是继承中的一个相关概念，但它与方法重写（覆盖）有所不同。

在C#中，通过在派生类中使用`new`关键字，可以隐藏基类中的同名方法。这被称为方法隐藏（method hiding）。

方法隐藏是在派生类中定义一个与基类中同名的方法，而不是重写基类方法。当使用派生类的实例调用这个方法时，会调用派生类中定义的方法，而不是基类中的方法。

以下是一个示例，演示了方法隐藏的使用：

```c#
public class Animal
{
    public void MakeSound()
    {
        Console.WriteLine("Animal makes a sound.");
    }
}

public class Dog : Animal
{
    public new void MakeSound()
    {
        Console.WriteLine("Dog barks.");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Animal animal = new Animal();
        animal.MakeSound();  // 输出："Animal makes a sound."

        Dog dog = new Dog();
        dog.MakeSound();     // 输出："Dog barks."

        Animal dogAnimal = new Dog();
        dogAnimal.MakeSound();   // 输出："Animal makes a sound."

        Console.ReadLine();
    }
}
```

在上面的示例中，`Animal`类中有一个`MakeSound()`方法，而`Dog`类中也定义了一个同名的方法。`Dog`类中的`MakeSound()`方法使用`new`关键字隐藏了基类中的方法。

当我们使用`Animal`类的实例调用`MakeSound()`方法时，执行的是基类中的方法。当我们使用`Dog`类的实例调用`MakeSound()`方法时，执行的是派生类中隐藏的方法。

注意，当将派生类的实例赋值给基类的变量时（如`Animal dogAnimal = new Dog()`），如果使用该变量调用`MakeSound()`方法，仍会执行基类中的方法。这是因为隐藏方法是通过编译时的静态类型来确定调用的方法，而不是基于运行时的实际对象类型。

#### get和set关键字

`get` 和 `set` 是访问器标识符，用于定义属性的读取器和写入器。它们具有以下特点：

`get` 标识符：用于定义属性的读取器。它指定了在访问属性时返回属性值的逻辑。`get` 方法必须返回与属性类型兼容的值。例如，对于一个名为 `MyProperty` 的属性，可以使用以下方式定义 `get` 访问器：

```c#
public int MyProperty
{
    get
    {
        // 返回属性值的逻辑
        return someValue;
    }
}
```

`set` 标识符：用于定义属性的写入器。它指定了在给属性赋值时设置属性值的逻辑。`set` 方法使用一个隐含的参数 `value`，表示将要分配给属性的值。例如，对于一个名为 `MyProperty` 的属性，可以使用以下方式定义 `set` 访问器：

```c#
public int MyProperty
{
    set
    {
        // 设置属性值的逻辑
        someValue = value;
    }
}
```

`get` 和 `set` 访问器可以一起使用，用于定义可读写的属性。例如：

```c#
private int myField;

public int MyProperty
{
    get
    {
        // 返回属性值的逻辑
        return myField;
    }
    set
    {
        // 设置属性值的逻辑
        myField = value;
    }
}
```

`get` 和 `set` 访问器可以具有不同的访问修饰符，例如 `public`、`private` 等，用于控制属性的访问级别。

在 C# 6.0 之后，可以使用自动属性来简化属性的定义。自动属性使用简化的语法，编译器会自动生成私有字段和访问器的实现。例如：

```c#
public int MyProperty { get; set; }
```

通过使用 `get` 和 `set` 标识符，可以实现属性的读取和写入逻辑，并提供对属性的访问和修改。这种封装属性的方式使得代码更具可读性和可维护性，并提供了对属性访问的控制和验证。

#### this和base关键字

在C#中，`this`和`base`是两个关键字，用于引用当前对象和基类对象。它们在以下情况下使用：

`this`关键字：

- `this`关键字用于引用当前对象的成员（字段、属性、方法等）。它可以解决成员与局部变量之间的命名冲突。例如，如果一个方法的参数名与成员名相同，可以使用`this`关键字来指明要访问的是成员而不是参数。

- `this`关键字还可以在构造函数中调用其他构造函数，以实现构造函数的重载。通过使用`this`关键字，可以在一个构造函数中调用同一类的其他构造函数。

- 例如：

  ```c#
  csharpCopy codepublic class MyClass
  {
      private int value;
  
      public MyClass(int value)
      {
          this.value = value;
      }
  
      public MyClass() : this(0) // 调用另一个构造函数
      {
      }
  }
  ```

`base`关键字：

- `base`关键字用于引用基类对象的成员（字段、属性、方法等）。当派生类中定义了与基类同名的成员时，可以使用`base`关键字来访问基类的成员。

- `base`关键字还可以在派生类的构造函数中调用基类的构造函数。通过使用`base`关键字，可以在派生类的构造函数中调用基类的特定构造函数。

- 例如：

  ```c#
  csharpCopy codepublic class MyBaseClass
  {
      protected int value;
  
      public MyBaseClass(int value)
      {
          this.value = value;
      }
  }
  
  public class MyDerivedClass : MyBaseClass
  {
      public MyDerivedClass(int value) : base(value) // 调用基类的构造函数
      {
      }
  }
  ```

总而言之，`this`关键字用于引用当前对象的成员，而`base`关键字用于引用基类对象的成员。它们在构造函数重载和解决成员命名冲突时非常有用。

#### 基类

基类（Base Class）是面向对象编程中的一个概念，它是其他类的父类或超类。基类定义了一组通用的属性、方法和行为，可以被其他类继承和重用。

在继承关系中，子类可以从基类中继承（或派生）属性和方法。子类继承基类的成员后，可以直接使用这些成员，无需重新编写相同的代码。基类通过提供通用的结构和行为，可以为子类提供共享的功能和特征。

在C#中，使用关键字`class`定义一个类时，可以指定该类的基类。例如，如果要创建一个名为`Car`的子类，并使其继承一个名为`Vehicle`的基类，可以使用冒号（:）指定继承关系：

```c#
class Vehicle // 基类
{
    // 基类的成员
}

class Car : Vehicle // 子类，继承自基类
{
    // 子类的成员
}
```

在上述代码中，`Vehicle`是`Car`的基类。`Car`类继承了`Vehicle`类的成员，并可以使用基类中定义的属性和方法。

基类可以包含构造函数、字段、属性、方法和事件等成员。子类可以直接访问基类的公共和受保护成员（根据访问修饰符的限制），并可以通过关键字`base`引用基类的成员。

基类的主要目的是促进代码的重用性和扩展性。通过继承基类，可以避免在每个子类中重复编写相同的代码，而是共享和继承基类的通用功能。基类也可以作为抽象基类或接口来定义规范和约束，以确保子类满足特定的行为要求。

##### 抽象类

在C#中，抽象类（Abstract Class）是一种特殊的类，用于提供其他类的基类（父类）。抽象类本身不能被实例化，**它主要用作其他类的蓝图或模板。**

抽象类通过在类的定义中使用 `abstract` 关键字来声明。它可以包含抽象方法、虚方法、实例字段和属性等成员。抽象方法是没有实现的方法，而是在派生类中被重写和实现。

下面是一个抽象类的示例：

```c#
abstract class Shape
{
    public abstract double CalculateArea(); // 抽象方法

    public void Print()
    {
        Console.WriteLine("This is a shape.");
    }
}
```

在上面的示例中，`Shape` 类是一个抽象类，其中包含一个抽象方法 `CalculateArea()` 和一个非抽象方法 `Print()`。派生类必须实现抽象方法，但可以选择性地重写非抽象方法。

当你想要定义一个基类，提供一些通用行为，但又不希望直接实例化该基类时，抽象类非常有用。它允许你定义一些通用的方法和属性，并要求派生类提供实现细节。

需要注意的是，抽象类不能直接实例化，但可以被用作引用类型。你可以创建派生类的实例，然后将其赋值给抽象类类型的变量。

```c#
Shape shape = new Circle();
```

总而言之，抽象类是一种用于定义其他类的基类，并且它可以包含抽象方法和非抽象方法。派生类必须实现抽象方法，而非抽象方法可以选择性地被重写。

> 抽象基类（Abstract Base Class）是面向对象编程中的一个概念，它是一种特殊的类，不能被直接实例化，而是被用作其他类的基类。
>
> 抽象基类本身是抽象的，它定义了一组抽象方法、虚方法或具体方法，这些方法可以在子类中进行重写或实现。抽象基类通常用于定义一组共享的特征、行为或接口，而不关心具体实现细节。
>
> 在C#中，可以使用`abstract`关键字将一个类声明为抽象基类。抽象基类可以包含以下类型的成员：
>
> 1. 抽象方法（Abstract Method）：这是一个没有实现体的方法，只有方法的声明，没有方法体。抽象方法必须在子类中进行实现（重写），用以提供具体的实现逻辑。
> 2. 虚方法（Virtual Method）：这是一个具有默认实现的方法，但可以在子类中进行重写。子类可以选择重写虚方法以提供自定义的实现，也可以使用默认实现。
> 3. 具体方法（Concrete Method）：这是一个具有完整实现的方法，没有关键字修饰。具体方法在抽象基类中提供默认实现，子类可以选择性地进行重写。
>
> 抽象基类的主要目的是作为其他类的模板，通过继承它来共享通用行为和结构。它可以提供一种约束，要求子类必须实现某些方法或遵循特定的接口。抽象基类在设计和组织大型项目时非常有用，可以提高代码的可重用性、可扩展性和可维护性。

#### 密封类

在C#中，密封类（Sealed Class）是一种特殊类型的类，它阻止其他类继承或派生自该密封类。使用 `sealed` 关键字可以将类声明为密封类。

当你声明一个类为密封类时，它将被标记为最终类，不允许其他类继承它或派生自它。换句话说，密封类不能作为基类用于其他类的继承。

下面是一个密封类的示例：

```c#
sealed class MySealedClass
{
    // 类的定义
}
```

密封类的主要目的是限制类的继承，以确保其行为和状态不会被修改或扩展。密封类常用于具有固定功能的类，它们通常被设计为不可修改和不可继承的。

需要注意的是，密封类本身可以继承自其他类，但不能被其他类继承。例如，一个密封类可以继承自非密封的基类。

总结一下，密封类是通过使用 `sealed` 关键字声明的一种特殊类。它阻止其他类继承或派生自该类，主要用于限制类的继承，确保类的行为和状态不被修改或扩展。

> 密封类不能被用作基类。在 C# 中，使用 `sealed` 关键字可以将一个类声明为密封类。密封类是为了防止其他类继承它。
>
> 当你将一个类声明为密封类时，它将不能被其他类继承。这意味着密封类不能作为其他类的基类，因为它不允许其他类派生自它。
>
> 以下是一个示例，展示了如何声明和使用密封类：
>
> ```c#
> public sealed class SealedClass
> {
>     // Class implementation
> }
> 
> public class DerivedClass : SealedClass  // 错误！密封类不能作为基类
> {
>     // Class implementation
> }
> ```
>
> 在上述示例中，`SealedClass` 被声明为密封类，使用 `sealed` 关键字进行标识。然后，我们尝试从 `SealedClass` 派生一个名为 `DerivedClass` 的类，但这是错误的，因为密封类不能作为基类。
>
> 密封类的主要目的是为了限制继承，以确保某个类的特定实现不会被子类修改或扩展。密封类通常用于不希望被继承或修改的情况，例如某些框架类或工具类。
>
> 需要注意的是，密封类本身可以继承自其他类。只是它不能作为基类供其他类继承。

#### 派生类

派生类（Derived Class）是指在面向对象编程中，通过继承从一个基类（父类）派生出新的类。派生类可以继承基类的成员（字段、属性、方法等），并且可以添加新的成员或修改继承的成员的行为。

在C#中，可以使用关键字 `class` 和冒号 `:` 来定义派生类，并指定基类。派生类在定义时可以扩展基类的功能，重写基类的虚方法，并可以添加自己的成员。

下面是一个派生类的示例：

```c#
class Vehicle // 基类
{
    public string Brand { get; set; }
    public int Year { get; set; }

    public virtual void Start()
    {
        Console.WriteLine("The vehicle starts.");
    }
}

class Car : Vehicle // 派生类
{
    public void Accelerate()
    {
        Console.WriteLine("The car accelerates.");
    }

    public override void Start()
    {
        Console.WriteLine("The car starts.");
    }
}
```

在上面的示例中，`Vehicle` 是一个基类，`Car` 是派生类。`Car` 类继承了基类 `Vehicle` 的属性和方法，并添加了自己的方法 `Accelerate()`。此外，派生类还可以重写基类中的虚方法，例如在 `Car` 类中重写了 `Start()` 方法。

使用派生类的好处是可以通过继承和扩展现有的类来创建新的类，从而实现代码的重用和组织。派生类可以使用基类的成员，并且可以根据需要修改或添加功能。

需要注意的是，C#不支持多重继承，即一个类不能直接继承自多个基类。但是，C#支持通过接口实现多重继承的特性。

总结一下，派生类是通过继承基类创建的新类。它可以继承基类的成员，可以添加新的成员或重写继承的成员的行为。通过派生类，可以实现代码的重用、组织和扩展。

#### 访问修饰符

在C#中，有多种访问修饰符（Access Modifiers）用于控制类、成员（字段、属性、方法等）以及其他元素的访问级别。以下是C#中常用的访问修饰符：

1. **public**：公共访问修饰符，表示成员可以从任何地方都可以访问，没有访问限制。
2. **private**：私有访问修饰符，表示成员只能在定义它的类内部被访问。私有成员对于类的外部是不可见的。
3. **protected**：受保护访问修饰符，表示成员可以在定义它的类及其派生类中被访问，但对于类的外部是不可见的。
4. **internal**：内部访问修饰符，表示成员可以在当前程序集内的任何类中访问，但对于程序集之外的类是不可见的。如果没有显式指定访问修饰符，默认情况下成员的访问级别就是 internal。
5. **protected internal**：受保护内部访问修饰符，表示成员可以在当前程序集内的任何类及其派生类中访问，以及在其他程序集中通过派生类访问。

这些访问修饰符可以用于类的定义、类的成员（字段、属性、方法等）的定义，以及命名空间的定义。它们用于控制对不同成员和元素的访问权限，帮助实现封装性、继承性和抽象性等面向对象编程的概念。

需要注意的是，访问修饰符的使用要根据具体的需求和设计决策。选择适当的访问修饰符可以确保代码的安全性和可维护性。

### 多态

在C#中，多态（Polymorphism）是面向对象编程中的一个重要概念。它允许使用相同的接口来处理不同类型的对象，从而实现代码的灵活性和可重用性。

多态性有两种形式：静态多态性（静态绑定）和动态多态性（动态绑定）。

静态多态性（静态绑定）： 在编译时，通过方法的重载和运算符重载，可以根据参数的类型来决定要调用的具体方法或操作符。这称为静态多态性，因为它在编译时期已经确定了具体的方法或操作符。

例如，假设有一个基类`Shape`和两个派生类`Circle`和`Rectangle`。如果在基类和派生类中定义了名为`Draw`的方法，那么可以根据调用对象的类型来决定具体调用哪个类的`Draw`方法。这就是静态多态性的体现。

动态多态性（动态绑定）： 动态多态性是通过继承和方法重写（override）实现的。它在运行时根据对象的实际类型来确定要调用的方法。当基类引用指向派生类的对象时，可以通过基类引用调用派生类中重写的方法。

例如，假设有一个基类`Animal`和两个派生类`Dog`和`Cat`。如果在基类中定义了一个名为`MakeSound`的虚方法，并在派生类中重写该方法，那么可以通过基类引用指向派生类的对象，并调用`MakeSound`方法时，实际执行的是派生类中重写的方法。这就是动态多态性的体现。

多态性的好处在于可以编写通用的代码，能够处理不同类型的对象，而无需为每种对象编写独立的代码。它提高了代码的可维护性和可扩展性，并促进了面向对象编程的核心概念之一：封装、继承和多态。

> 多态性在面向对象编程中具有多种常用功能。以下是一些常见的多态性功能：
>
> 1. 方法重写（Method Overriding）：多态性通过方法重写实现。子类可以重写基类的方法，并在运行时根据实际对象类型调用相应的方法。这允许不同的对象以不同的方式响应相同的方法调用。
> 2. 动态绑定（Dynamic Binding）：多态性通过动态绑定实现。在运行时，系统根据对象的实际类型决定要调用的方法。这使得代码能够在运行时适应不同的对象类型，实现灵活性和可扩展性。
> 3. 抽象类和接口（Abstract Class and Interface）：抽象类和接口是多态性的重要概念。它们定义了一组共享的行为或功能，并允许不同的类实现这些行为或功能。通过基于抽象类或接口编程，可以编写通用的代码，处理多个具体的类对象。
> 4. 泛型编程（Generic Programming）：多态性在泛型编程中起着关键作用。通过泛型类型参数，可以编写与具体类型无关的通用代码。这样的代码可以在不同类型的对象上进行操作，提高了代码的可重用性和类型安全性。
> 5. 集合和容器类的多态性：在面向对象的集合和容器类中，多态性允许将不同类型的对象存储在同一个集合或容器中，并可以通过统一的接口来访问和处理这些对象。这提供了更灵活的数据结构和算法设计。
> 6. 虚方法和抽象方法的多态性：通过在基类中定义虚方法（Virtual Method）或抽象方法（Abstract Method），子类可以重写这些方法，并根据自身的需求提供不同的实现。这种多态性允许通过基类的引用调用子类的方法。
>
> 这些功能使得多态性成为面向对象编程中的重要概念之一。它提供了灵活性、可扩展性和可维护性，使得代码能够更好地适应变化和复杂性。

#### 接口

在C#中，接口（Interface）是一种定义了一组方法、属性和事件的合同（Contract），用于描述类或结构体应具有的行为。接口定义了一组公共的成员，但没有提供具体的实现。类或结构体可以实现一个或多个接口，并通过实现接口中的成员来达到接口所描述的行为。

接口在C#中使用 `interface` 关键字进行声明。下面是一个简单的接口示例：

```c#
interface IShape
{
    double CalculateArea();
    void Draw();
}
```

在上面的示例中，`IShape` 接口定义了两个方法：`CalculateArea()` 和 `Draw()`。任何类实现了这个接口都必须提供这两个方法的具体实现。

类可以通过使用 `:` 符号来实现一个或多个接口。例如：

```c#
class Circle : IShape
{
    public double Radius { get; set; }

    public double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }

    public void Draw()
    {
        Console.WriteLine("Drawing a circle.");
    }
}
```

在上面的示例中，`Circle` 类实现了 `IShape` 接口，并提供了 `CalculateArea()` 和 `Draw()` 方法的具体实现。

通过接口，可以实现多态性，即一个对象可以被视为属于多个类型。类可以实现多个接口，从而具备不同接口所定义的行为，这提供了更大的灵活性和可重用性。

接口还可以用于定义事件和属性的规范，类在实现接口时必须提供这些事件和属性的实现。

总结一下，接口是一种定义行为规范的合同，它描述了类或结构体应该具有的一组方法、属性和事件。接口本身没有提供具体的实现，而是由类或结构体实现接口并提供具体的实现。通过实现接口，类可以达到多态性，并具备不同接口所定义的行为。

> **接口和类的区别：**
>
> 相似之处：
>
> 1. 都可以定义抽象成员（未实现的方法、属性、事件等）。
> 2. 都不能被直接实例化，需要其他类来实现或继承。
>
> 区别之处：
>
> 1. 接口可以多重继承，而类只能单继承。一个类可以实现多个接口，但只能继承一个抽象类。
> 2. 接口只能定义抽象成员，不包含实现，而抽象类可以包含具体实现的成员。
> 3. 接口中的成员默认是公共的（public），而抽象类中的成员可以有不同的访问修饰符。
> 4. 接口不能包含字段（字段是类的状态），而抽象类可以包含字段。
> 5. 接口不能提供代码的实现，而抽象类可以提供部分实现。
>
> 接口通常用于描述对象应该具备的行为，定义了一套合约，供实现接口的类来遵循。通过实现接口，类可以在不同的层次结构中共享相同的行为规范，实现了一种多态性。
>
> 抽象类则更多地用于定义一种通用的基类，可以包含具体的实现代码，以及一些子类通用的属性和方法。抽象类可以作为其他类的基类被继承，子类可以继承抽象类的实现代码，并覆盖其中的抽象成员。
>
> 总之，接口主要关注行为的规范和多态性，而抽象类则更关注代码的复用和继承。选择使用接口还是抽象类，取决于设计的目标和需求。

#### 方法重写

方法重写（Method overriding）是面向对象编程中的一个概念，它允许子类重新定义并实现从父类继承的方法。

当一个类继承自另一个类时，子类可以继承父类的方法。但有时子类可能需要修改继承的方法的实现，以满足自己的特定需求。方法重写提供了这样的机制，允许子类提供一个新的实现来替代父类的方法。

要实现方法重写，需要满足以下条件：

1. **子类必须继承自父类。**
2. **父类中的方法必须被标记为`virtual`，表示它可以被子类重写。**
3. **子类中需要使用`override`关键字来重写父类中的方法，并提供新的实现。**

子类中重写的方法必须具有与父类中被重写的方法相同的名称、参数列表和返回类型。通过重写方法，子类可以改变或扩展父类方法的行为，以适应子类的需求。

当使用父类的引用指向子类的对象时，如果调用被重写的方法，实际上会执行子类中重写的方法，而不是父类中的原始方法。这种行为称为动态绑定，因为在运行时决定要调用的具体方法。

方法重写是实现多态性的一种方式，它允许以统一的方式处理不同类型的对象，并根据对象的实际类型来调用相应的方法。这提高了代码的灵活性、可维护性和可扩展性。

#### 动态绑定

动态绑定（Dynamic Binding）是多态性的一个重要概念，它是指在运行时根据对象的实际类型来决定要调用的方法或执行的操作。通过动态绑定，程序能够根据对象的实际类型选择正确的方法实现，实现灵活性和可扩展性。

在编译时，编译器通常根据变量的声明类型来确定要调用的方法。但是，当使用多态性的特性时，对象的实际类型可能与声明类型不同。在这种情况下，动态绑定允许在运行时根据对象的实际类型来解析方法调用。

具体而言，当通过基类的引用指向子类的对象时，编译器会根据基类的声明类型确定要调用的方法。但在运行时，实际执行的是子类中重写的方法，而不是基类中的原始方法。这是因为在运行时，系统会根据对象的实际类型动态选择正确的方法实现。

例如，考虑以下代码片段：

```c#
class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Animal makes a sound");
    }
}

class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Dog barks");
    }
}

Animal animal = new Dog();
animal.MakeSound(); // 动态绑定，实际调用的是Dog类的MakeSound方法
```

在上述代码中，`animal`是一个基类`Animal`的引用，但它实际上指向一个子类`Dog`的对象。当调用`MakeSound`方法时，通过动态绑定，实际执行的是`Dog`类中重写的`MakeSound`方法，输出"Dog barks"。

通过动态绑定，程序能够根据对象的实际类型来决定要调用的方法，实现了多态性的灵活性和可扩展性。这对于处理不同类型的对象和实现运行时多态行为非常重要。

> 

#### 泛型

泛型（Generics）是一种在编程语言中使用类型参数来创建可重用代码的机制。它允许在编写类或方法时延迟指定具体的数据类型，使代码能够适用于多种不同类型的数据。

泛型的主要目的是提供类型安全和代码重用。通过使用泛型，可以编写与数据类型无关的通用代码，避免重复编写相似的代码，提高代码的可维护性和可扩展性。

在C#中，泛型可以应用于类、结构体、接口和方法。它使用尖括号（<>）来指定类型参数，并在需要使用具体类型时进行替换。

以下是一个使用泛型的示例：

```c#
csharpCopy codeclass MyGenericClass<T>
{
    private T genericField;

    public MyGenericClass(T value)
    {
        genericField = value;
    }

    public T GetGenericField()
    {
        return genericField;
    }
}

// 使用泛型类
MyGenericClass<int> myIntObject = new MyGenericClass<int>(42);
int value = myIntObject.GetGenericField(); // 返回42

MyGenericClass<string> myStringObject = new MyGenericClass<string>("Hello");
string text = myStringObject.GetGenericField(); // 返回"Hello"
```

在上述示例中，`MyGenericClass<T>`是一个泛型类，其中的`T`是一个类型参数。通过在创建对象时指定具体类型，可以创建`MyGenericClass`的实例。这样，`genericField`字段和`GetGenericField`方法的类型将根据实际指定的类型进行实例化。

泛型提供了许多优势，包括：

1. 类型安全：泛型在编译时提供类型检查，防止类型不匹配的错误。
2. 代码重用：可以编写通用的算法和数据结构，适用于多种类型的数据。
3. 性能提升：泛型可以避免装箱和拆箱操作，提高性能。
4. 可读性和维护性：泛型代码通常更具可读性和可维护性，因为它们是与类型无关的通用代码。

总而言之，泛型是一种强大的编程机制，它允许在编写代码时推迟具体类型的指定，从而提供更灵活、类型安全和可重用的代码。

## 练习3

这个练习是为了熟系所有的面向对象的内容，我们将从头开始创建一个类，用于实现所有的面向对象特性。

### 练习封装

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Object_oriented_testing
{
    //创建一个类用来练习封装
    //tip - 在类中，对方法和变量如果不加访问修饰符一般都是私有的（private）
    public class IT
    {
        //1.生成一个类中最基础的东西
        // - 成员变量
        //公有变量可以在类外中改变  - 用.操作符即可
        //私有变量只能在类中调用 - 在类外就不能调用
        private int myVariable;
        public int myPublic;

        //2.构造函数
        // - 构造函数 - 用来初始化变量
        //在创建对象的时候自动运行
        public IT() {
            Console.WriteLine("我是构造函数");
            myVariable = 1;
            myPublic = 1;
        }
        //3.有参构造函数
        //用于用户自己指定所需要的私有变量的值
        //私有变量只能在类中改变值 - 但是我们可以通过构造函数来初始化我们想要的值
        public IT(int variable)
        {
            myVariable = variable;
            myPublic = variable;
        }
        //4.构造我们的类方法
        //类方法完成某种方式 - 这里我们就简单一点
        //用方法打印一些字段
        public void test()
        {
            Console.WriteLine("你好我是test这个方法");
            Console.WriteLine("{0}我打印变量{1}和{2}", myVariable, myPublic);
        }
        //5.析构函数
        //在程序结束的时候自动运行
        ~IT() {
            myPublic = 1;
            myVariable = 1;
            Console.WriteLine("我是析构函数");
        }

    }
}
```

```c#
using Object_oriented_testing;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Object_oriented_testing
{

    internal class Program
    {
        static void Main(string[] args)
        {
            IT a = new IT();
            //1.对公共变量赋值
            a.myPublic = 3;
            //2.调用对象方法
            a.test();
            Console.ReadKey();
        }
    }
}
```

![image-20230604202621214](image-20230604202621214.png)

### 练习继承

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace 练习继承
{
   
    public abstract class Car
    {
        //创建Car作为基类 - 一般基类被创建为抽象类
        //抽象类不能实例化——只是我们子类的蓝图
        //创建两个变量 - 来传值给我们的属性
        private string id = "并没有设定默认id";
        private string name = "并没有表明该车辆的名字";
        //创建Car类来模拟继承

        //1.创建两个属性
        //属性的概念其实无处不在，我们的名字和性别就是一种属性
        //我们可以赋予属性功能来实现我们的目的
        public string Id
        {
            get
            {
                return id;
            }
            set
            {
                //赋值的时候由value传递值给name
                id = value;
            }

        }
        public string Name
        {
            get
            {
                return name;
            }
            set
            {
                name = value;
            }
        }

        //2.创建Car的虚方法 - 用于子类重写
        public virtual void MyCars()
        {
            Console.WriteLine("这辆车是什么");
        }
        //这里可以用属性代替
        public virtual void Price()
        {
            Console.WriteLine("这辆车的价钱是多少");
        }
        public virtual void Balance()
        {
            Console.WriteLine("测试一下未重写是怎么样的");
        }

    }

    public class Wuling_Hongguang : Car
    {
        //3.重写父类的虚方法
        public override void MyCars()
        { 
            Console.WriteLine("这辆车是五菱宏光");
        }
        public override void Price() {
            Console.WriteLine("这辆车很便宜");
        }
        
        public Wuling_Hongguang()
        {
            //在构造函数可用 - 用于调用父类的属性
            base.Id ="001";
            base.Name = "五菱宏光";
            //打印一下对应的属性
            Console.WriteLine(Id);
            Console.WriteLine(Name);
        }
        
    }


    internal class Program
    {
        static void Main(string[] args)
        {
            //创建一个五菱宏光的对象
            Wuling_Hongguang a = new Wuling_Hongguang();
            //更改一下五菱宏光的属性
            a.Id = "002";
            a.Name = "小绵羊";
            a.Price();
            a.MyCars();
            a.Balance();
            //打印自定义属性
            Console.WriteLine("{0},{1}",a.Id,a.Name);
            //打印对象就出现对象对应的命名空间
            Console.WriteLine(a);

        }
    }
}
```

![image-20230605130627976](image-20230605130627976.png)

> 当你打印一个对象时，它的默认打印行为是输出对象的完全限定类型名称。这是通过调用对象的 `ToString()` 方法实现的。
>
> 在 C# 中，所有类都继承自 `System.Object`，而 `System.Object` 类定义了默认的 `ToString()` 方法。默认情况下，`ToString()` 方法会返回对象的完全限定类型名称。
>
> 例如，假设你声明了一个名为 `myCar` 的对象，类型为 `Car`：
>
> ```c#
> csharpCopy codeCar myCar = new Car();
> Console.WriteLine(myCar);
> ```
>
> 输出将会是类似以下的结果：
>
> ```c#
> vbnetCopy code
> Namespace.Car
> ```
>
> `Namespace` 是 `Car` 类所在的命名空间，而 `Car` 是类的名称。
>
> 如果你想在输出中显示自定义的信息，你可以重写基类 `System.Object` 中的 `ToString()` 方法，以便返回你想要的字符串表示形式。例如，你可以在 `Car` 类中重写 `ToString()` 方法，以返回车辆的特定信息：
>
> ```c#
> csharpCopy codepublic override string ToString()
> {
>     return "Car: " + Name + ", ID: " + Id;
> }
> ```
>
> 这样，当你打印 `myCar` 对象时，将会输出你自定义的字符串表示形式，而不是默认的完全限定类型名称。

### 练习多态

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace _03_练习多态
{
    //1.定义了一个接口
    //接口可以实现多继承——来调用我们频繁使用的方法
    interface IMyInterface
    {
        void MyMethod();
        int MyProperty { get; set; }

    }
    public abstract class Name {

        private string a;
        //创建属性对a操作
        public string A
        {
            get { return a; }
            set { a = value; }
        }

        //抽象类无法声明主题，需要继承来重写
        public abstract double Call(double value);
      
        
    }



    //2.创建一个类用来调用我们的接口方法
    //顺便继承父类 - 记住继承抽象类一定要重写它的抽象方法
    class MyClass : Name, IMyInterface
    {
        public void MyMethod()
        {
            // 实现接口中定义的方法
        }
        public int MyProperty { get; set; }

        public override double Call(double value)
        {
            // 提供具体的实现
            return value * 2;
        }
    }

    //例子2
    class Animal
    {
        public virtual void MakeSound()
        {
            Console.WriteLine("The animal makes a sound.");
        }
    }

    class Dog : Animal
    {
        public override void MakeSound()
        {
            Console.WriteLine("The dog barks.");
        }
    }

    class Cat : Animal
    {
        public override void MakeSound()
        {
            Console.WriteLine("The cat meows.");
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            Animal animal1 = new Dog();
            Animal animal2 = new Cat();

            animal1.MakeSound(); // 输出：The dog barks.
            animal2.MakeSound(); // 输出：The cat meows.

        }
    }
}
```

![image-20230605194425539](image-20230605194425539.png)

## c#功能类*

### 泛型

泛型（Generics）是一种在编程语言中实现参数化类型的机制。它允许我们编写可重用的代码，可以在不同的数据类型上进行操作，而无需为每种类型编写重复的代码。

使用泛型，我们可以定义类、接口、方法等具有参数化类型的实体。参数化类型允许我们在使用时指定具体的类型，从而使代码更加灵活和可重用。

泛型的主要好处如下：

1. 类型安全：通过使用泛型，我们可以在编译时捕获类型错误。编译器会在编译时检查泛型代码的类型，并提供类型安全保证。
2. 代码重用：泛型使我们能够编写通用的代码，可以在不同类型上进行操作。这样可以避免重复编写类似的代码，提高代码的重用性和可维护性。
3. 性能优化：使用泛型可以提高代码的性能。由于泛型代码在编译时生成特定类型的代码，因此避免了装箱和拆箱操作，并提供了更好的性能。

以下是一个使用泛型的简单示例：

```c#
class Stack<T>
{
    private T[] items;
    private int top;

    public Stack(int capacity)
    {
        items = new T[capacity];
        top = -1;
    }

    public void Push(T item)
    {
        items[++top] = item;
    }

    public T Pop()
    {
        return items[top--];
    }
}

class Program
{
    static void Main()
    {
        Stack<int> intStack = new Stack<int>(5);
        intStack.Push(1);
        intStack.Push(2);
        intStack.Push(3);

        int value = intStack.Pop();
        Console.WriteLine(value); // 输出：3

        Stack<string> stringStack = new Stack<string>(5);
        stringStack.Push("Hello");
        stringStack.Push("World");

        string str = stringStack.Pop();
        Console.WriteLine(str); // 输出：World
    }
}
```

在上述示例中，我们定义了一个泛型类 `Stack<T>`，它表示一个栈数据结构。通过在类定义中使用泛型参数 `T`，我们可以在创建对象时指定栈中存储的元素类型。

在 `Main` 方法中，我们创建了一个 `Stack<int>` 对象 `intStack`，用于存储整数类型的元素，并进行了入栈和出栈操作。然后，我们创建了一个 `Stack<string>` 对象 `stringStack`，用于存储字符串类型的元素，并进行了相应的操作。

通过使用泛型，我们可以在不同类型上重复使用相同的栈类，并在编译时获得类型安全和性能优化的好处。

#### list

在 C# 中，`List<T>` 类是泛型集合类之一，用于表示动态大小的可变数组。`List<T>` 提供了一组强大的方法和属性，用于操作和管理元素列表。

`List<T>` 类位于 `System.Collections.Generic` 命名空间中，是 .NET Framework 中最常用的集合类型之一。它提供了类似数组的功能，但具有更大的灵活性和功能。

以下是 `List<T>` 类的一些主要特点和功能：

1. 动态大小：`List<T>` 允许根据需要自动调整容量，以容纳不同数量的元素。可以随时向列表中添加或删除元素。
2. 泛型：`List<T>` 是一个泛型类，可以在创建时指定元素的类型。泛型提供了类型安全和性能优化的好处。
3. 索引访问：可以通过索引访问列表中的元素。使用索引可以读取或修改列表中特定位置的元素。
4. 增删改查：`List<T>` 提供了一系列的方法用于添加、删除、修改和搜索列表中的元素。例如，`Add` 方法用于添加元素，`Remove` 方法用于删除元素，`Find` 方法用于搜索元素等等。
5. 排序和排序搜索：`List<T>` 提供了排序方法（`Sort`）和排序搜索方法（`BinarySearch`），方便对列表中的元素进行排序和搜索。
6. 遍历：可以使用 `foreach` 循环或迭代器来遍历 `List<T>` 中的元素。

下面是一个使用 `List<T>` 的简单示例：

```c#
csharpCopy codeusing System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        List<string> fruits = new List<string>();
        
        // 添加元素
        fruits.Add("Apple");
        fruits.Add("Banana");
        fruits.Add("Orange");
        
        // 访问元素
        Console.WriteLine(fruits[0]); // 输出：Apple
        
        // 遍历元素
        foreach (string fruit in fruits)
        {
            Console.WriteLine(fruit);
        }
        
        // 删除元素
        fruits.Remove("Banana");
        
        // 判断元素是否存在
        bool containsOrange = fruits.Contains("Orange");
        Console.WriteLine(containsOrange); // 输出：True
    }
}
```

在上述示例中，我们创建了一个 `List<string>` 对象 `fruits`，并使用 `Add` 方法向其中添加了几个水果。然后，我们可以使用索引访问元素，使用 `foreach` 循环遍历元素，使用 `Remove` 方法删除元素，并使用 `Contains` 方法判断元素是否存在。

总之，`List<T>` 类是 C# 中用于表示动态大小的可变数组的泛型集合类，提供了丰富的方法和属性，方便对元素列表进行操作和管理。

## c#高级

### 正则表达式

正则表达式（Regular Expression），简称为正则或正则表达式，是一种用于描述和匹配字符串模式的工具。它是由一系列字符和特殊字符组成的表达式，可以用来进行字符串的搜索、匹配、替换和提取等操作。

正则表达式由普通字符（例如字母、数字和标点符号）和特殊字符组成，特殊字符具有特定的含义和功能。通过组合和使用这些字符，可以构建出具有复杂匹配规则的模式。

正则表达式常用于以下场景：

1. 搜索和匹配：可以用来搜索和匹配符合特定模式的字符串。例如，可以使用正则表达式来检查一个字符串是否符合某种格式，如邮箱地址、电话号码等。
2. 替换和修改：可以用来替换字符串中符合某个模式的部分。例如，可以使用正则表达式将一个文本中的所有日期格式改为另一种格式。
3. 提取和分割：可以用来提取字符串中符合某个模式的部分。例如，可以使用正则表达式从一个网页的 HTML 代码中提取出所有链接。
4. 格式验证：可以用来验证用户输入的数据是否符合特定的格式要求。例如，可以使用正则表达式来验证用户输入的密码是否包含特定的字符和长度。

正则表达式是跨平台的，几乎所有现代编程语言都支持正则表达式的操作。在 C# 中，可以使用 `System.Text.RegularExpressions` 命名空间提供的正则表达式类来处理和操作正则表达式。

以下是一个简单的示例，演示了使用正则表达式来搜索和匹配字符串：

```c#
csharpCopy codeusing System;
using System.Text.RegularExpressions;

class Program
{
    static void Main()
    {
        string input = "Hello, 12345!";

        // 定义匹配模式
        string pattern = @"\d+";

        // 创建正则表达式对象
        Regex regex = new Regex(pattern);

        // 搜索匹配项
        Match match = regex.Match(input);

        // 判断是否找到匹配项
        if (match.Success)
        {
            Console.WriteLine("找到匹配项：{0}", match.Value);
        }
        else
        {
            Console.WriteLine("未找到匹配项");
        }
    }
}
```

在上述示例中，我们定义了一个匹配模式 `@"\d+"`，表示一个或多个连续的数字。然后，我们使用 `Regex` 类创建一个正则表达式对象，并使用 `Match` 方法在输入字符串中搜索匹配项。

如果找到了匹配项，我们可以通过 `Match` 对象的属性来获取匹配结果。在此示例中，我们输出找到的匹配项。

总结来说，正则表达式是一种用于描述和匹配字符串模式的工具。它具有广泛使用的语法和特殊字符，可以用来进行字符串的搜索、匹配、替换和提取等操作。在不同编程语言中，正则表达式的具体语法和使用方式可能有所不同，但基本的概念和功能是相通的。

以下是一些常用的正则表达式特殊字符和符号的示例：

- `.`: 匹配任意单个字符，除了换行符。
- `*`: 匹配前一个元素零次或多次。
- `+`: 匹配前一个元素一次或多次。
- `?`: 匹配前一个元素零次或一次。
- `[]`: 匹配括号内的任意一个字符。例如，`[abc]` 匹配字符 'a'、'b' 或 'c'。
- `[^]`: 匹配不在括号内的任意一个字符。例如，`[^abc]` 匹配除了 'a'、'b' 和 'c' 之外的任意字符。
- `\d`: 匹配任意一个数字字符。等价于 `[0-9]`。
- `\w`: 匹配任意一个字母、数字或下划线字符。等价于 `[A-Za-z0-9_]`。
- `\s`: 匹配任意一个空白字符，包括空格、制表符、换行符等。
- `^`: 匹配字符串的开头位置。
- `$`: 匹配字符串的结尾位置。
- `|`: 在模式中使用逻辑或，匹配两个或多个模式之一。

除了上述特殊字符外，正则表达式还支持使用括号进行分组、使用限定符指定匹配次数、使用转义字符来匹配特殊字符等。

正则表达式可以在不同的编程语言中使用，例如 C#、Java、Python、JavaScript 等。每种编程语言可能有自己的正则表达式类或函数，用于进行正则操作。在 C# 中，可以使用 `System.Text.RegularExpressions` 命名空间提供的类来处理和操作正则表达式。

### 委托

在 C# 中，委托（Delegate）是一种类型，用于表示对一个或多个方法的引用。委托可以将方法视为对象，并将其赋值给委托变量，以便在需要时调用这些方法。

委托提供了一种将方法作为参数传递、将方法存储在数据结构中和通过委托调用方法的机制。它可以帮助实现事件处理、回调函数和多播委托等功能。

以下是委托的主要特点和使用方法：

1. 类型安全：委托是类型安全的，它在编译时会进行类型检查，以确保委托变量与其引用的方法具有相同的签名（参数和返回类型）。
2. 方法引用：委托可以引用一个或多个具有相同签名的方法。通过将方法赋值给委托变量，可以将方法作为对象来操作。
3. 事件处理：委托经常用于实现事件处理机制。当事件发生时，相关的委托将被调用，并执行相应的方法。
4. 回调函数：委托可以用作回调函数的机制，允许将一个方法传递给另一个方法，在需要时进行回调调用。
5. 多播委托：委托可以合并多个方法，形成一个多播委托。调用多播委托时，所有合并的方法都会被依次调用。

以下是一个简单的委托示例：

```c#
using System;

class Program
{
    delegate void MyDelegate(string message);

    static void Main()
    {
        MyDelegate myDelegate = DisplayMessage;
        myDelegate("Hello, World!");

        myDelegate += DisplayUpperCaseMessage;
        myDelegate("Hello, World!");
    }

    static void DisplayMessage(string message)
    {
        Console.WriteLine("Message: " + message);
    }

    static void DisplayUpperCaseMessage(string message)
    {
        Console.WriteLine("Message in uppercase: " + message.ToUpper());
    }
}
```

在上述示例中，我们定义了一个委托类型 `MyDelegate`，它可以引用具有 `void` 返回类型和一个 `string` 参数的方法。然后，我们创建了一个委托变量 `myDelegate`，并将其赋值为 `DisplayMessage` 方法。

通过调用委托变量 `myDelegate`，我们实际上调用了被委托引用的方法 `DisplayMessage`，并传递了一个字符串参数。

在后续操作中，我们将另一个方法 `DisplayUpperCaseMessage` 添加到委托变量 `myDelegate` 中。当调用委托变量时，两个方法都会被依次调用，从而实现多播委托的功能。

总而言之，委托是一个类，可以定义自己的委托类型，也可以使用.NET框架提供的预定义委托类型。预定义的委托类型包括`Action`、`Func`、`Predicate`等，它们具有不同的签名和功能，可以用于不同的场景。

下面是一些常用的预定义委托类型：

- `Action`: 表示一个不返回值的委托。可以用于执行无返回值的操作。
- `Func`: 表示一个具有返回值的委托。可以用于执行带有返回值的操作。
- `Predicate`: 表示一个接受一个参数并返回布尔值的委托。通常用于进行条件判断。

预定义委托类型的使用示例：

```c#
using System;

class Program
{
    static void Main()
    {
        // 使用 Action 委托执行无返回值的操作
        Action<string> actionDelegate = DisplayMessage;
        actionDelegate("Hello, World!");

        // 使用 Func 委托执行带有返回值的操作
        Func<int, int, int> funcDelegate = AddNumbers;
        int result = funcDelegate(10, 20);
        Console.WriteLine("Result: " + result);

        // 使用 Predicate 委托进行条件判断
        Predicate<int> predicateDelegate = IsPositive;
        bool isPositive = predicateDelegate(5);
        Console.WriteLine("Is Positive: " + isPositive);
    }

    static void DisplayMessage(string message)
    {
        Console.WriteLine("Message: " + message);
    }

    static int AddNumbers(int x, int y)
    {
        return x + y;
    }

    static bool IsPositive(int number)
    {
        return number > 0;
    }
}
```

在上述示例中，我们使用了不同的预定义委托类型来执行不同的操作。通过使用泛型，我们可以指定委托的参数类型和返回类型。

使用 `Action<string>` 委托执行了一个接受一个字符串参数并且没有返回值的方法 `DisplayMessage`。使用 `Func<int, int, int>` 委托执行了一个接受两个整数参数并返回整数结果的方法 `AddNumbers`。使用 `Predicate<int>` 委托进行了一个判断，判断一个整数是否为正数。

预定义委托类型使得使用委托更加方便和简洁，并且减少了自定义委托类型的需要。

总结来说，委托是一个类，用于表示对一个或多个方法的引用。它可以定义自己的委托类型，也可以使用预定义的委托类型。委托在C#中提供了一种灵活的机制，可以将方法作为参数传递、存储和调用。它在事件处理、回调函数和多播委托等场景中非常有用。

> `Action` 委托是 C# 中预定义的一个委托类型，它用于表示一个不返回值的委托。`Action` 委托可以接受最多 16 个输入参数，但不返回任何值。
>
> `Action` 委托的定义如下：
>
> ```c#
> csharpCopy codepublic delegate void Action();
> public delegate void Action<in T>(T obj);
> public delegate void Action<in T1, in T2>(T1 arg1, T2 arg2);
> // ... 其他重载 ...
> ```
>
> `Action` 委托的命名规范为以 "Action" 开头，后面跟着输入参数的个数，最多可以有 16 个输入参数。
>
> 以下是一些使用 `Action` 委托的示例：
>
> 不带参数的 `Action` 委托：
>
> ```c#
> csharpCopy codeAction greet = () =>
> {
>     Console.WriteLine("Hello, World!");
> };
> 
> greet(); // 输出: Hello, World!
> ```
>
> 在这个示例中，我们创建了一个不带参数的 `Action` 委托变量 `greet`，并使用 Lambda 表达式来定义委托的方法体。通过调用委托变量 `greet()`，实际上会执行 Lambda 表达式中定义的方法，输出 "Hello, World!"。
>
> 带有一个输入参数的 `Action` 委托：
>
> ```c#
> csharpCopy codeAction<string> greet = (name) =>
> {
>     Console.WriteLine("Hello, " + name + "!");
> };
> 
> greet("John"); // 输出: Hello, John!
> ```
>
> 在这个示例中，我们创建了一个带有一个输入参数的 `Action` 委托变量 `greet`，并使用 Lambda 表达式来定义委托的方法体。通过调用委托变量 `greet("John")`，Lambda 表达式中定义的方法会接收到参数 "John"，输出 "Hello, John!"。
>
> 带有多个输入参数的 `Action` 委托：
>
> ```c#
> csharpCopy codeAction<int, int> add = (a, b) =>
> {
>     int result = a + b;
>     Console.WriteLine("Sum: " + result);
> };
> 
> add(5, 3); // 输出: Sum: 8
> ```
>
> 在这个示例中，我们创建了一个带有两个输入参数的 `Action` 委托变量 `add`，并使用 Lambda 表达式来定义委托的方法体。通过调用委托变量 `add(5, 3)`，Lambda 表达式中定义的方法会接收到参数 5 和 3，计算它们的和并输出结果 "Sum: 8"。
>
> `Action` 委托提供了一种方便的方式来定义和使用不返回值的委托。通过使用不同的重载，可以接受不同数量和类型的输入参数。这使得委托的使用更加灵活和简洁。

> `Func` 委托是 C# 中预定义的一个委托类型，它用于表示一个具有返回值的委托。`Func` 委托可以接受最多 16 个输入参数，并返回一个指定的结果类型。
>
> `Func` 委托的定义如下：
>
> ```c#
> csharpCopy codepublic delegate TResult Func<out TResult>();
> public delegate TResult Func<in T, out TResult>(T arg);
> public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2);
> // ... 其他重载 ...
> ```
>
> `Func` 委托的命名规范为以 "Func" 开头，后面跟着输入参数的个数，最多可以有 16 个输入参数。最后一个类型参数表示返回值的类型。
>
> 以下是一些使用 `Func` 委托的示例：
>
> 不带参数的 `Func` 委托：
>
> ```c#
> csharpCopy codeFunc<string> getGreeting = () =>
> {
>     return "Hello, World!";
> };
> 
> string greeting = getGreeting(); // greeting = "Hello, World!"
> ```
>
> 在这个示例中，我们创建了一个不带参数的 `Func` 委托变量 `getGreeting`，并使用 Lambda 表达式来定义委托的方法体。通过调用委托变量 `getGreeting()`，实际上会执行 Lambda 表达式中定义的方法，返回字符串 "Hello, World!"。
>
> 带有一个输入参数的 `Func` 委托：
>
> ```c#
> csharpCopy codeFunc<string, string> greet = (name) =>
> {
>     return "Hello, " + name + "!";
> };
> 
> string greeting = greet("John"); // greeting = "Hello, John!"
> ```
>
> 在这个示例中，我们创建了一个带有一个输入参数的 `Func` 委托变量 `greet`，并使用 Lambda 表达式来定义委托的方法体。通过调用委托变量 `greet("John")`，Lambda 表达式中定义的方法会接收到参数 "John"，返回字符串 "Hello, John!"。
>
> 带有多个输入参数的 `Func` 委托：
>
> ```c#
> csharpCopy codeFunc<int, int, int> add = (a, b) =>
> {
>     return a + b;
> };
> 
> int result = add(5, 3); // result = 8
> ```
>
> 在这个示例中，我们创建了一个带有两个输入参数的 `Func` 委托变量 `add`，并使用 Lambda 表达式来定义委托的方法体。通过调用委托变量 `add(5, 3)`，Lambda 表达式中定义的方法会接收到参数 5 和 3，计算它们的和并返回结果 8。
>
> `Func` 委托提供了一种方便的方式来定义和使用带有返回值的委托。通过使用不同的重载，可以接受不同数量和类型的输入参数，并指定返回值的类型。这使得委托的使用更加灵活和简洁。

#### 多播委托

多播委托（Multicast Delegate）是 C# 中的一种特殊委托类型，它可以持有一个或多个目标方法的引用，并且可以按顺序依次调用这些方法。

在 C# 中，多播委托是由 `System.MulticastDelegate` 类派生而来的委托类型。它具有以下特点：

1. 可以使用 `+` 运算符将多个委托合并为一个多播委托。
2. 可以使用 `-` 运算符将一个委托从多播委托中移除。
3. 调用多播委托时，会按照添加的顺序依次调用每个目标方法。

下面是一个简单的示例，演示如何使用多播委托：

```c#
using System;

public delegate void MyDelegate(string message);

class Program
{
    static void Main()
    {
        MyDelegate myDelegate = MethodA;
        myDelegate += MethodB;
        myDelegate += MethodC;

        myDelegate("Hello, World!");
    }

    static void MethodA(string message)
    {
        Console.WriteLine("MethodA: " + message);
    }

    static void MethodB(string message)
    {
        Console.WriteLine("MethodB: " + message);
    }

    static void MethodC(string message)
    {
        Console.WriteLine("MethodC: " + message);
    }
}
```

输出结果：

```c#
MethodA: Hello, World!
MethodB: Hello, World!
MethodC: Hello, World!
```

在上述示例中，我们定义了一个名为 `MyDelegate` 的委托类型，它接受一个字符串参数并且没有返回值。然后，我们创建了一个多播委托 `myDelegate`，并使用 `+=` 运算符将三个方法 `MethodA`、`MethodB` 和 `MethodC` 添加到委托中。最后，我们通过调用委托 `myDelegate("Hello, World!")`，依次调用了每个方法，并传递相同的字符串参数。

需要注意的是，多播委托的调用顺序与添加方法的顺序有关。在示例中，我们按照添加的顺序依次调用了 `MethodA`、`MethodB` 和 `MethodC`。

另外，通过使用 `-` 运算符，我们可以从多播委托中移除一个方法。例如，使用 `myDelegate -= MethodB;` 可以从多播委托中移除 `MethodB` 方法。

多播委托在某些情况下非常有用，例如事件处理程序、回调函数等场景，它允许将多个方法组合在一起，并且能够方便地按顺序调用这些方法。

### 事件

在 C# 中，事件（Event）是一种特殊的语言构造，用于实现发布-订阅模型（Publish-Subscribe Model）。事件允许一个对象（发布者）通知其他对象（订阅者）某个特定事件的发生，以便订阅者可以执行相应的操作。

事件由两个主要组成部分组成：事件声明和事件触发。事件声明定义了事件的名称、事件处理程序的委托类型以及可选的事件访问器。事件触发是在发布者对象中引发事件，以通知订阅者事件的发生。

以下是一个简单的示例，演示了如何在 C# 中声明和使用事件：

```c#
csharpCopy codeusing System;

class Program
{
    // 1. 事件声明
    public event EventHandler MyEvent;

    static void Main()
    {
        Program program = new Program();

        // 2. 事件订阅（添加事件处理程序）
        program.MyEvent += Program_MyEventHandler;

        // 3. 触发事件
        program.OnMyEvent();
    }

    // 4. 事件触发的方法
    protected virtual void OnMyEvent()
    {
        MyEvent?.Invoke(this, EventArgs.Empty);
    }

    // 5. 事件处理程序
    private static void Program_MyEventHandler(object sender, EventArgs e)
    {
        Console.WriteLine("Event handled!");
    }
}
```

在上述示例中，我们首先在 `Program` 类中声明了一个事件 `MyEvent`，它使用 `EventHandler` 委托作为事件处理程序的类型。然后，在 `Main` 方法中订阅了该事件，即将事件处理程序方法 `Program_MyEventHandler` 添加到事件的订阅列表中。最后，通过调用 `OnMyEvent` 方法触发了事件。

当事件触发时，订阅事件的事件处理程序方法 `Program_MyEventHandler` 将被执行，并在控制台输出 "Event handled!"。

需要注意的是，事件的访问器可以使用 `add` 和 `remove` 关键字进行自定义。这允许控制事件的订阅和取消订阅的行为。

事件是一种强大的机制，它可以实现对象间的松耦合通信，提供了一种简洁且可扩展的方式来处理对象间的交互。在 C# 中，事件被广泛用于处理用户界面的交互、异步操作的完成通知、消息传递和回调机制等各种场景。

### 匿名方法

匿名方法（Anonymous Methods）是 C# 中一种特殊的方法，它允许我们在代码中定义一个没有显式命名的方法。匿名方法可以作为委托的实例或事件处理程序使用。

匿名方法的语法形式如下：

```c#
delegate (parameters)
{
    // 方法体
};
```

在匿名方法中，我们可以定义方法体并传递参数，就像定义普通方法一样。匿名方法的参数列表和方法体放在一个委托类型的声明中，并且可以直接使用该委托类型的实例进行调用。

以下是一个使用匿名方法的示例，展示了如何将匿名方法作为委托的实例使用：

```c#
using System;

delegate void GreetingDelegate(string name);

class Program
{
    static void Main()
    {
        GreetingDelegate greeting = delegate (string name)
        {
            Console.WriteLine("Hello, " + name + "!");
        };

        greeting("John"); // 输出: Hello, John!
    }
}
```

在上述示例中，我们定义了一个名为 `GreetingDelegate` 的委托类型，它接受一个字符串参数并且没有返回值。然后，我们创建了一个匿名方法并将其赋值给委托变量 `greeting`。匿名方法的方法体是在 `delegate` 关键字后的大括号内定义的。最后，我们通过调用委托变量 `greeting("John")` 来调用匿名方法，并传递字符串参数 "John"。

匿名方法通常用于简单的委托场景，当我们只需要定义一个简短的方法体并且不想为其命名时，匿名方法提供了一种方便的方式。匿名方法可以与委托类型一起使用，允许我们以一种更简洁的方式定义委托实例。

### lambda表达式

Lambda 表达式是 C# 中的一种语法特性，它提供了一种简洁的方式来定义匿名函数。Lambda 表达式可以用于创建委托实例、LINQ 查询、事件处理程序等场景。

Lambda 表达式的基本语法形式如下：

```c#
(parameters) => expression
```

Lambda 表达式包括以下几个部分：

- 参数列表：一对括号中包含零个或多个输入参数，可以指定参数的类型或使用隐式类型推断。
- 箭头运算符 `=>`：表示从参数到表达式的映射。
- 表达式：定义 Lambda 表达式的方法体，可以是单个表达式或语句块。

以下是一些使用 Lambda 表达式的示例：

无参数的 Lambda 表达式：

```c#
() => Console.WriteLine("Hello, World!");
```

这个 Lambda 表达式没有参数，表达式体是调用 `Console.WriteLine` 方法输出 "Hello, World!"。

带有一个参数的 Lambda 表达式：

```c#
name => Console.WriteLine("Hello, " + name + "!");
```

这个 Lambda 表达式有一个参数 `name`，表达式体是调用 `Console.WriteLine` 方法输出 "Hello, " 加上参数值。

带有多个参数的 Lambda 表达式：

```c#
(a, b) => a + b;
```

这个 Lambda 表达式有两个参数 `a` 和 `b`，表达式体是将参数 `a` 和 `b` 相加并返回结果。

Lambda 表达式可以与委托类型一起使用，用于创建委托实例。例如，我们可以使用 Lambda 表达式创建 `Func` 委托的实例：

```c#
Func<int, int, int> add = (a, b) => a + b;
```

在这个示例中，我们创建了一个带有两个输入参数和一个返回值的 `Func` 委托实例 `add`，使用 Lambda 表达式 `(a, b) => a + b` 定义了方法体，实现了将两个参数相加并返回结果的逻辑。

Lambda 表达式的优势在于它的简洁性和灵活性。它可以减少编写冗余代码的需求，并且可以更直观地表达某些操作。Lambda 表达式在 LINQ 查询、集合操作、事件处理程序等场景中被广泛使用，提供了一种便捷的方式来编写简短而高效的代码。

### 扩展方法

在C#中，扩展方法（Extension Methods）是一种特殊的静态方法，允许你向已存在的类型添加新的方法，而无需修改原始类型的定义或创建子类。

扩展方法提供了一种在不修改已有类型的情况下，为它们添加额外的功能的方式。这使得你可以向.NET框架中的类型添加自定义的方法，或者为自己定义的类型添加更便利的操作。

下面是扩展方法的一般写法：

```c#
csharpCopy codepublic static class ExtensionClass
{
    public static ReturnType MethodName(this ExtendedType obj, parameters)
    {
        // 扩展方法的实现逻辑
    }
}
```

上述代码展示了一个扩展方法的示例，其中：

- `ExtensionClass` 是定义扩展方法的静态类。类必须是静态的，并且在命名空间中。
- `MethodName` 是你给扩展方法起的名字。
- `ExtendedType` 是你要扩展的类型。
- `ReturnType` 是扩展方法的返回类型。
- `this ExtendedType obj` 表示将该扩展方法应用于 `ExtendedType` 类型的对象，通过 `this` 关键字进行标识。

下面是一个示例，展示如何使用扩展方法为字符串类型添加一个自定义的方法：

```c#
csharpCopy codepublic static class StringExtensions
{
    public static string ReverseString(this string input)
    {
        char[] chars = input.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }
}
```

在上述示例中，我们为字符串类型添加了一个名为 `ReverseString` 的扩展方法，用于将字符串反转。

使用扩展方法时，需要注意以下几点：

- 扩展方法必须定义在静态类中。
- 扩展方法必须是静态的。
- 扩展方法的第一个参数必须使用 `this` 关键字标识，并指定要扩展的类型。
- 调用扩展方法时，编译器会自动将调用者作为第一个参数传递给扩展方法。

通过使用扩展方法，你可以在不修改已有类型的情况下，为其添加自定义的行为和功能。这对于扩展.NET框架中的类型或者自定义类型非常有用，可以使代码更加简洁、可读，并提供更便利的操作。

### Obsolete特性

在C#中，`Obsolete`特性用于标记已经过时（deprecated）的代码元素，例如类型、方法、属性或字段。通过将`Obsolete`特性应用于代码元素，可以向开发人员发出警告或错误，以指示该代码元素不再建议使用，并提供替代方案或建议。

`Obsolete`特性具有以下特征：

1. 编译器警告：使用`Obsolete`特性标记的代码元素将导致编译器生成警告。这提醒开发人员在使用过时的代码元素时需要注意，并考虑替代方案。
2. 自定义警告消息：`Obsolete`特性允许你提供自定义的警告消息，以解释为什么该代码元素被标记为过时，以及建议使用哪些替代方案。开发人员可以根据这些消息来了解如何迁移代码。
3. 错误级别：你可以选择将`Obsolete`特性的错误级别设置为`Error`，这将导致编译器将过时的代码元素视为错误，阻止编译通过。这对于强制要求开发人员立即处理过时的代码非常有用。
4. 版本控制：`Obsolete`特性还允许你指定过时的代码元素应该从哪个版本开始过时。这样，你可以明确说明从哪个版本开始推荐使用替代方案，并为使用较旧版本的代码的开发人员提供指导。

使用`Obsolete`特性有助于提高代码的可维护性和可读性，确保开发人员及时了解不再建议使用的代码

### onditional特性

在C#中，`Conditional`特性用于在编译时根据条件决定是否包含或排除代码。它允许您根据条件选择性地编译和执行代码。

`Conditional`特性有两个常见的用途：

1. 条件编译：通过使用条件预处理指令，您可以根据编译时定义的条件选择性地编译不同的代码块。`Conditional`特性可以与条件编译一起使用，以便在满足特定条件时才包含代码。这对于在不同的构建配置中包含或排除代码非常有用。

   例如，考虑以下代码：

   ```c#
   csharpCopy code#define DEBUG
   
   public class Example
   {
       [Conditional("DEBUG")]
       private static void DebugMethod()
       {
           Console.WriteLine("Debug method called.");
       }
   
       public static void Main()
       {
           DebugMethod();
       }
   }
   ```

   在这个例子中，`DebugMethod`方法带有`[Conditional("DEBUG")]`特性。这表示只有在定义了`DEBUG`预处理符号时，编译器才会将`DebugMethod`方法包含在生成的代码中。因此，只有当在代码的顶部定义了`#define DEBUG`时，`DebugMethod`方法才会在`Main`方法中调用。

2. 方法调用条件性：使用`Conditional`特性还可以根据特定条件选择性地调用方法。在这种情况下，方法本身将始终包含在生成的代码中，但方法调用可能会根据条件被排除。

   ```c#
   csharpCopy codepublic class Example
   {
       [Conditional("DEBUG")]
       private static void DebugMethod()
       {
           Console.WriteLine("Debug method called.");
       }
   
       public static void Main()
       {
           DebugMethod();
       }
   }
   ```

   在这个例子中，`DebugMethod`方法带有`[Conditional("DEBUG")]`特性。这意味着无论如何，`DebugMethod`方法都会被包含在生成的代码中，但是在调用`DebugMethod`方法时，它只会在定义了`DEBUG`预处理符号时执行。

需要注意的是，`Conditional`特性只能应用于无返回值（`void`）的静态方法。这是因为在编译时，编译器会根据条件决定是否调用方法，而无法处理方法的返回值。

总结来说，`Conditional`特性允许您根据条件选择性地编译和执行代码，或选择性地调用方法。它在进行条件编译和调试期间非常有用。

### 反射*

在C#中，反射（Reflection）是一种强大的机制，它允许你在运行时动态地探查、访问和操作程序集（assembly）、类型（type）和成员（member）的信息。通过反射，你可以在编译时不知道类型和成员的具体信息的情况下，通过名称来动态地获取并使用它们。

反射提供了一组类型（位于`System.Reflection`命名空间下）和方法，用于在运行时获取类型信息、创建对象实例、调用方法、获取和设置属性值等。下面是一些反射的常用用途：

1. 获取类型信息：你可以使用`Type`类来获取一个类型的信息，包括类型的名称、命名空间、基类、实现的接口、属性、方法等。你可以使用`typeof`运算符获取一个类型的`Type`对象，也可以使用`GetType()`方法获取一个对象的类型。
2. 创建对象实例：通过反射，你可以使用`Activator`类来动态地创建对象实例。你可以指定类型的名称或`Type`对象，然后使用`Activator.CreateInstance`方法来创建对象。
3. 调用方法和访问属性：反射允许你通过方法名称和参数信息来调用类型的方法，并获取方法的返回值。你可以使用`Type.InvokeMember`方法来实现。类似地，你也可以通过反射来获取和设置属性的值。
4. 动态加载程序集：反射可以在运行时动态地加载程序集。通过`Assembly`类，你可以加载和探查程序集，获取其中的类型信息，并在需要时创建对象实例或调用方法。

需要注意的是，反射是一个强大但复杂的机制，其使用可能会对性能产生一定的影响。在大多数情况下，建议优先考虑使用静态类型和编译时绑定，而将反射作为一种备选方案，用于处理那些在编译时无法确定的类型和成员。

#### Assembly程序集合类

在C#中，`Assembly`类是一个用于加载、探索和操作程序集（assembly）的核心类。下面是`Assembly`类的一些主要特点和功能：

1. 加载程序集：`Assembly`类提供了多种方法来加载程序集，包括从文件、字节数组、流或已加载的程序集中加载。你可以使用`Assembly.Load`或`Assembly.LoadFrom`方法来加载程序集，并获取一个`Assembly`对象，以便后续的操作。
2. 获取程序集信息：通过`Assembly`类，你可以获取程序集的各种信息，包括程序集的名称、版本号、公钥、特性、引用的程序集等。你可以使用`AssemblyName`类来访问这些信息，并通过`Assembly.GetCustomAttributes`方法获取程序集的自定义特性。
3. 探索类型信息：`Assembly`类允许你在程序集中探索类型信息。你可以使用`Assembly.GetTypes`方法获取程序集中定义的所有类型，并使用`Type`类的相关方法来访问类型的成员、特性和基类信息。
4. 创建对象实例：通过`Assembly`类，你可以在程序集中创建对象实例。使用`Assembly.CreateInstance`方法，你可以根据类型名称或`Type`对象创建对象实例。这对于实现插件系统或动态加载模块非常有用。
5. 动态调用方法和访问属性：`Assembly`类允许你通过反射动态调用程序集中类型的方法和访问属性。你可以使用`Assembly.GetType`方法获取类型的`Type`对象，然后使用反射的方法来调用方法和访问属性。
6. 扩展性和灵活性：`Assembly`类提供了一种扩展性和灵活性，使得在运行时动态加载和操作程序集成为可能。这对于实现插件系统、动态扩展功能或实现基于配置的行为非常有用。

需要注意的是，`Assembly`类的使用需要谨慎考虑，并且在处理程序集时需要确保安全性和性能。反射和程序集操作可能具有一定的开销，因此在应用程序中要适度使用，并考虑使用更高效的技术来满足需求。

## LINQ

在C#中，LINQ（Language Integrated Query）是一种强大的查询语言和查询操作符的集合，它被集成到语言和.NET框架中，用于对各种数据源进行统一的查询和操作。

LINQ提供了一种统一的语法和模型，用于查询和操作不同类型的数据，如集合、数组、数据库、XML等。使用LINQ，开发人员可以通过直观的查询表达式或方法调用的方式来执行各种数据查询和操作，而无需编写繁琐的迭代代码。

LINQ的核心思想是将查询看作是对数据源进行筛选、排序、分组和投影等操作的过程。它提供了一组标准的查询操作符，如Where、Select、OrderBy、GroupBy、Join等，以及强大的表达式树和延迟执行的机制。

以下是一个使用LINQ查询的简单示例：

```c#
var numbers = new int[] { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

var evenNumbers = from num in numbers
                  where num % 2 == 0
                  select num;

foreach (var number in evenNumbers)
{
    Console.WriteLine(number);
}
```

在上述示例中，我们使用LINQ查询从一个整数数组中筛选出所有的偶数。使用`from`关键字指定查询的数据源（`numbers`数组），然后使用`where`关键字指定筛选条件，最后使用`select`关键字选择要返回的结果。

LINQ不仅可以用于集合的查询，还可以用于数据库查询（LINQ to SQL）、XML文档查询（LINQ to XML）、数据集查询（LINQ to DataSet）等。它提供了一种统一的编程模型，使得对不同类型数据源的查询具有一致的语法和风格。

通过LINQ，开发人员可以更加简洁、直观地编写查询代码，提高开发效率和代码可读性。同时，LINQ还具有编译时类型检查和优化查询的能力，使得查询更加安全和高效。

> LINQ 可以类比为在 C#（或其他支持 LINQ 的语言）中的 SQL 语句。LINQ 提供了一种类似于 SQL 的查询语法，使开发人员可以以一种直观和声明性的方式编写查询。
>
> 通过使用 LINQ 查询表达式，你可以使用类似于 SQL 的关键字和操作符（如`select`、`from`、`where`、`join`、`group by`、`order by`等）来编写查询逻辑。这样，你可以以一种熟悉且易于理解的方式来表示数据的筛选、排序、投影和聚合等操作。
>
> 这里是一个示例，展示了使用 LINQ 查询表达式来筛选和排序一个整数集合：
>
> ```c#
> csharpCopy codeList<int> numbers = new List<int> { 5, 2, 8, 1, 9, 3, 7, 6, 4 };
> 
> var query = from number in numbers
>             where number > 5
>             orderby number descending
>             select number;
> 
> foreach (var result in query)
> {
>     Console.WriteLine(result);
> }
> ```
>
> 在上面的示例中，我们使用 LINQ 查询表达式从 `numbers` 列表中筛选出大于 5 的数，并按降序排序。然后，我们遍历查询结果并将每个结果打印到控制台。
>
> 虽然 LINQ 查询表达式的语法类似于 SQL，但它是在编译时进行类型检查的，因此可以提供更高的类型安全性和编译时错误检测。此外，LINQ 还提供了方法语法，可以通过使用 LINQ 扩展方法来编写查询，这种方式更接近于在代码中编写函数式操作。

#### LINQ集合联合查询

在LINQ中，可以使用联合操作符进行集合的联合查询。联合查询允许你将多个集合合并为一个结果集，去除重复项并保持元素的顺序。

LINQ提供了两个常用的联合操作符：

1. `Union()`：将两个集合的元素合并为一个结果集，去除重复项。
2. `Concat()`：将两个集合的元素按顺序合并为一个结果集，不去除重复项。

下面是使用这两个联合操作符进行集合联合查询的示例：

```c#
csharpCopy codevar numbers1 = new List<int> { 1, 2, 3, 4, 5 };
var numbers2 = new List<int> { 4, 5, 6, 7, 8 };

// 使用 Union() 进行联合查询
var unionQuery = numbers1.Union(numbers2);

Console.WriteLine("Union Query:");
foreach (var number in unionQuery)
{
    Console.WriteLine(number);
}

// 使用 Concat() 进行联合查询
var concatQuery = numbers1.Concat(numbers2);

Console.WriteLine("Concat Query:");
foreach (var number in concatQuery)
{
    Console.WriteLine(number);
}
```

在上述示例中，我们定义了两个整数列表 `numbers1` 和 `numbers2`，然后使用 `Union()` 方法和 `Concat()` 方法进行联合查询。

在使用 `Union()` 方法时，结果集中的重复项会被去除，最终输出结果为：1, 2, 3, 4, 5, 6, 7, 8。

在使用 `Concat()` 方法时，结果集中保留了所有的元素，并按照它们在集合中的顺序进行合并，最终输出结果为：1, 2, 3, 4, 5, 4, 5, 6, 7, 8。

需要注意的是，联合查询的结果集是一个延迟执行的查询，只有在访问结果集时才会执行实际的查询操作。

除了 `Union()` 和 `Concat()`，LINQ还提供了其他的集合操作符，如交集查询（`Intersect()`）、差集查询（`Except()`）等，可以根据具体需求选择合适的操作符进行集合联合查询。

#### join on联合查询

在 C# 中，可以使用 LINQ（Language-Integrated Query）来执行联合查询和使用 `join on` 子句。LINQ 是一种强大的查询语言，可以与各种数据源（例如集合、数据库等）进行交互。

下面是一个示例，展示了如何在 C# 中使用 `join on` 来执行联合查询：

```c#
using System;
using System.Collections.Generic;
using System.Linq;

public class Program
{
    public static void Main()
    {
        // 创建示例数据源
        List<Order> orders = new List<Order>
        {
            new Order { OrderId = 1, CustomerId = 1, OrderDate = new DateTime(2023, 1, 1) },
            new Order { OrderId = 2, CustomerId = 1, OrderDate = new DateTime(2023, 2, 1) },
            new Order { OrderId = 3, CustomerId = 2, OrderDate = new DateTime(2023, 3, 1) },
            new Order { OrderId = 4, CustomerId = 2, OrderDate = new DateTime(2023, 4, 1) }
        };

        List<Customer> customers = new List<Customer>
        {
            new Customer { CustomerId = 1, Name = "John" },
            new Customer { CustomerId = 2, Name = "Jane" }
        };

        // 执行联合查询
        var query = from order in orders
                    join customer in customers on order.CustomerId equals customer.CustomerId
                    where customer.Name == "John"
                    select new
                    {
                        OrderId = order.OrderId,
                        CustomerName = customer.Name,
                        OrderDate = order.OrderDate
                    };

        // 输出查询结果
        foreach (var result in query)
        {
            Console.WriteLine($"OrderId: {result.OrderId}, CustomerName: {result.CustomerName}, OrderDate: {result.OrderDate}");
        }
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int CustomerId { get; set; }
    public DateTime OrderDate { get; set; }
}

public class Customer
{
    public int CustomerId { get; set; }
    public string Name { get; set; }
}
```

在上面的示例中，我们创建了两个示例数据源：`orders`（订单）和 `customers`（客户）。然后，我们使用 LINQ 查询语法执行联合查询，使用 `join on` 将 `orders` 和 `customers` 根据 `CustomerId` 进行联接，并通过 `where` 子句筛选出 `Name` 为 "John" 的客户。最后，我们选择需要的字段，并将结果输出到控制台。

请注意，这只是一个简单的示例，你可以根据自己的实际需求进行调整和扩展。

## 设计模式

设计模式是一种被广泛接受和使用的解决特定软件设计问题的经验总结。它们提供了一套被认为是最佳实践的解决方案，可以帮助开发人员设计可维护、可扩展和可重用的代码。

以下是一些常见的设计模式：

1. 创建型模式（Creational Patterns）：
   - 工厂模式（Factory Pattern）
   - 抽象工厂模式（Abstract Factory Pattern）
   - 单例模式（Singleton Pattern）
   - 建造者模式（Builder Pattern）
   - 原型模式（Prototype Pattern）
2. 结构型模式（Structural Patterns）：
   - 适配器模式（Adapter Pattern）
   - 桥接模式（Bridge Pattern）
   - 组合模式（Composite Pattern）
   - 装饰者模式（Decorator Pattern）
   - 外观模式（Facade Pattern）
   - 享元模式（Flyweight Pattern）
   - 代理模式（Proxy Pattern）
3. 行为型模式（Behavioral Patterns）：
   - 责任链模式（Chain of Responsibility Pattern）
   - 命令模式（Command Pattern）
   - 解释器模式（Interpreter Pattern）
   - 迭代器模式（Iterator Pattern）
   - 中介者模式（Mediator Pattern）
   - 备忘录模式（Memento Pattern）
   - 观察者模式（Observer Pattern）
   - 状态模式（State Pattern）
   - 策略模式（Strategy Pattern）
   - 模板方法模式（Template Method Pattern）
   - 访问者模式（Visitor Pattern）
4. 并发模式（Concurrency Patterns）：
   - 生产者-消费者模式（Producer-Consumer Pattern）
   - 读者-写者模式（Reader-Writer Pattern）
   - 同步模式（Synchronization Pattern）

每种设计模式都有其特定的应用场景和解决问题的方式。选择适当的设计模式可以提高代码的可读性、可维护性和可扩展性，同时也有助于促进团队合作和共享设计经验。然而，设计模式并非万能的，使用时需要根据具体情况进行权衡和选择。

### 观察者设计模式

观察者设计模式（Observer Design Pattern）是一种行为设计模式，它定义了对象之间的一对多依赖关系，使得当一个对象的状态发生变化时，其相关依赖对象都能够自动得到通知并更新。这种模式也被称为发布-订阅（Publish-Subscribe）模式。

在观察者设计模式中，有两个主要的角色：

1. Subject（主题）：也称为被观察者或发布者，它维护一组观察者对象，并且提供了添加、删除和通知观察者的方法。当主题的状态发生变化时，它会通知所有注册的观察者。
2. Observer（观察者）：也称为订阅者，它定义了一个接口，用于接收主题的通知。当观察者接收到通知时，它可以执行相应的操作以更新自己的状态。

观察者设计模式的工作原理如下：

1. 观察者通过订阅主题来注册自己，以便接收主题的通知。
2. 主题维护一个观察者列表，用于记录所有注册的观察者。
3. 当主题的状态发生变化时，它会遍历观察者列表，并调用每个观察者的通知方法。
4. 观察者接收到通知后，可以根据需要进行相应的处理。

观察者设计模式的优点包括：

1. 松耦合：主题和观察者之间是松耦合的关系，它们之间通过接口进行通信，主题不需要知道观察者的具体实现。
2. 可扩展性：可以方便地添加新的观察者，主题和观察者之间的关系可以动态地建立和解除。
3. 一致性：主题和观察者之间的一致性得到了保证，无论有多少观察者，它们都能够接收到相同的通知。

然而，观察者设计模式也有一些潜在的缺点，包括：

1. 如果观察者过多或处理逻辑过于复杂，可能会导致性能问题。
2. 观察者可能会收到不必要的通知，需要谨慎设计以避免这种情况。

总之，观察者设计模式是一种常用的设计模式，它在许多应用场景中都有广泛的应用，例如事件处理、GUI 编程、消息队列等。通过使用观察者模式，可以实现对象之间的解耦和增加系统的灵活性。
