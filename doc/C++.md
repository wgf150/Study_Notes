## 1 基础知识
### 1.1 main()

通常格式： `int main()`

1. main()返回值：
* 返回值默认为int，部分编译器允许其他返回值（如 void），但是不利于代码兼容性。
* 有返回的main()，函数体结尾若无返回语句，默认执行 `return 0` （仅main()支持）
* （C99之前）C标准下，函数不定义返回值，会被默认定义为返回int。
```C++
//部分场景符合语法，但是不建议这么写
main()    // original C style
```
2. main()参数：
* main()，C++ 中，空括号表示不接受参数，C 中，空括号表示待定（可接受参数）
* main(void)，将明确表示main()不接受任何参数


### 1.2 注释
1. C++风格注释：双斜杠(//) 打头，行注释
2. C风格注释：斜杠+星号(/\* \*/)，通常C++也可以使用，此格式支持多行注释

### 1.3 头文件
1. C++风格，头文件通常不带.h后缀，例：`include <iostream>` 
2. C风格，头文件以.h结尾，C++中依旧可用
3. 部分C 头文件，被转化为了C++形式（c前缀），例：`<math.h>` -> `<cmath>`

### 1.4 名称空间
名称空间（namespace）可以防止不同源文件出现命名冲突
1. 很多通用的类、函数、对象，都被囊括在std中（C++标准模板库）
2. 编译指令`using`可以声明名称空间，让作用域内不必使用名称空间前缀，例：
```C++
//声明整个名称空间
using namespace std;
...
	cout << "no std" << endl;

//声明名称空间中的单个名称；
using std::cout;
using std::endl;
...
	cout << "no std::cout & std::endl" << endl;
```
3. `using`指令受作用域限制，仅在当前作用域下生效。

### 1.5 代码格式化
1. 标记和空白
	标记（token）：代码中不可分割的元素
	空白（white space）：用于分开两个标记的元素 
	C++中，空白包括了，空格、制表符、回车，但部分情况下，某些标记（逗号、括号）可以不需要空白来分开
```C++
//C++中，回车可以与空格等效替换
int
main
()
{ return 0; }

//各类token & white spcae使用场景
	return 0；        // INVALID
	return (0);       // VALID
	intmain();        // INVALID
	int main ( );     // VALID

```
2. 编码风格（书中描述，仅做参考）
	* 每条语句占一行
	* 每个函数都有一个开始花括号和一个结束花括号， 这两个花括号各占一行
	* 函数中的语句都相对于花括号进行缩进
	* 与函数名称相关的圆括号周围没有空白

### 1.6 函数
1. 函数原型
	* 函数原型，描述了函数名称、返回值类型、参数类型及顺序，例如
	```C++
	//二者均正确，即函数原型不一定要包括形参名称
	double sqrt(double)     // valid
	double sqrt(double x)   // valid
	```
	* 函数原型 通常作为函数声明的一种形式，而目前函数声明的主要都是通过函数原型来实现
		目前C99之后的C 以及 C++已经不支持隐式函数声明，意味着函数使用前必须要有函数声明，且需要是函数原型格式 或 函数定义格式
2. 函数定义
	* C++（C也一样）不允许函数定义嵌套，即函数体中，不允许定义函数；不过，部分编译器（如 gcc）提供了对嵌套函数的拓展支持。实际开发时不建议使用
	```C++
	//下述代码在 gcc 中可以使用
	#include <stdio.h>
	int main()
	{
		int func(int, int);
		func(1, 2);
	}
	
	int func(int a, int b)
	{
		printf("%d %d\n", a, b);
	}
	```


## 2 基本类型

内置的C++数据类型分为：基本类型、复合类型。

### 2.1 变量
#### 2.1.1 变量名命名规范：
1. 强制规范
	* 名称中只能使用字母字符、数字、下划线
	* 名称的第一个字符不能是数字
	* 字符大小写敏感
	* 不能使用C++关键字作为名称
2. 隐性规范：
	* 下划线开头的变量，通常会有特殊含义
		* 单下划线+大写字母/双下划线 开头的名称，通常留给编译器相关使用
		* 单下划线 开头的名称，通常被编译器作为全局标识符
		* 部分风格下，下换线开头的名称会用来定义类成员，减少与函数同名
		用户使用的变量，建议避免定义下划线开头的全局变量，以免产生冲突。
	* C++对于名称长度没有明确限制，部分平台可能会有限制
		注：C语言会有所不同，C99下，名称仅保证前63个字符有效
3. 命名风格：
		通常因人而异，常见的有下换线分隔式（如：`apple_count`），驼峰式（如：`appleCount`）

#### 2.1.2 变量初始化
1. 初始化将赋值和声明合并在一起，除了通过C传统的赋值语句，还有部分C++的方式（如 构造形式）
	```C++
	int num1 = 1;
	int num2(2);
	
	int num3 = {};
	int num4 = {4};
	int num5{5};
	```
	C++特有的大括号{} 初始化方式，针对类变量，也有通用性
	
2. 初始化的赋值可以用到其他变量、表达式等，但赋值涉及到的变量必须提前声明
3. 变量未初始化，变量的值将是它被创建前，内存中保存的值


### 2.2 整型

C++的基本整型，包括：char、short、int、long、long long
#### 2.2.1 长度范围
1. 类型最小长度
	统一整型，在不同平台下的长度可能不同，但是C++（C）做了最小长度的限制：
	* short至少16位
	* int至少与short等长
	* long至少32位，且至少与int等长
	* long long至少64位，且至少与long等长
2. 获取整型的长度范围：
	* 运算符sizeof，用于返回类型或变量的长度（单位为字节）
	```C++
	//gcc 7.5.0 下
		sizeof(char)       // 返回 (unsigned long)1UL
		sizeof(short)      // 返回 (unsigned long)2UL
		sizeof(int)        // 返回 (unsigned long)4UL
		sizeof(long)       // 返回 (unsigned long)
		sizeof(long long)  // 返回 (unsigned long)
	```
	* `<climits>`头文件（C中为`limits.h`），通过符号常量，描述了关于整型限制的各种信息
	
| 符号常量       | 表示                     |
| :--------- | :--------------------- |
| CHAR_BIT   | char的位数                |
| CHAR_MAX   | char的最大值               |
| CHAR_MIN   | char的最小值               |
| SCHAR_MAX  | signed char的最大值        |
| SCHAR_MIN  | signed char的最小值        |
| UCHAR_MAX  | unsigned char的最大值      |
| SHRT_MAX   | short的最大值              |
| SHRT_MIN   | short的最小值              |
| USHRT_MAX  | unsigned short的最大值     |
| INT_MAX    | int的最大值                |
| INT_MIN    | int的最小值                |
| UNIT_MAX   | unsigned int的最大值       |
| LONG_MAX   | long的最大值               |
| LONG_MIN   | long的最小值               |
| ULONG_MAX  | unsigned long的最大值      |
| LLONG_MAX  | long long的最大值          |
| LLONG_MIN  | long long的最小值          |
| ULLONG_MAX | unsigned long long的最大值 |

#### 2.2.2 整型溢出
1. 无符号溢出
	无符号溢出，通常会做取模运算（也可看做舍弃溢出位）
	```C++
	unsigned char x = 0xff;     // x = 255
	unsigned char y = x + 1;    // y = 0,溢出后与2^8(256)求模 
	```

2. 有符号溢出
	有符号溢出，在C&C++标准下，会被定义为 未定义的行为（undefined behavior），具体看编译器实现
	```C++
	// gcc 7.5.0 
	//加减运算溢出
	signed char x = 0x7f;       // x = 127
	signed char y = x + 1;      // y = -128,溢出后，变为了signed char的最小值
	
	//乘法溢出
	signed char x1 = 0x7f;      // x1 = 127
	signed char x2 = 0x05;      // x2 = 5
	signed char y = x1 * x2;    // y = 123
	```
	可以看出，当前编译器下，有符号整型溢出和无符号整型溢出相同，会舍弃溢出位

3. 溢出危害
	通常情况下，很多整型运算都有溢出的风险，在出现时会引发一些不可预期的结果
	```C++
	//
	```
	针对溢出问题：
	* 做好溢出校验
	* 使用安全方法，避免溢出

#### 2.2.3 整型字面值
1. 通常有八进制、十进制、十六进制，整型默认输出时，通常都是十进制
	八进制通常以 0 开头，如 077
	十六进制通常以 0x 或 0X 开头，如 0xff；
```C++
int a = 30;
int b = 30;
int c = 30;

std::cout << a << std::endl;                 // 输出 30
std::cout << std::hex << b << std::endl;     // 输出 1e
std::cout << std::oct << c << std::endl;     // 输出 36

```

如上，`iostream` 提供了更改输出格式的**控制符**：dec、hex、oct

#### 2.2.4 整型常量
1. 未加额外修饰，且范围允许的情况下，C&C++下整型常量默认为 int 类型
2. C&C++可以通过后缀修饰，来限定整型常量的类型
	* 后缀 l 或 L，表示为long类型
	* 后缀 u 或 U，表示unsigned int类型
	* 后缀 ul（大小写任意，下同），表示 unsigned类型
	* 后缀 ll，表示long long类型
	* 后缀 ull，表示unsigned long long类型

#### 2.2.5 char类型
1. char类型是以整型存储的，会根据一定规则（如：ASCII），与计算机中的字符一一映射
2. char字面值：
	通常由单引号括起，表示字符字面值，如'M'代表字符M，实际存储为77（ASCII转换）
	部分字符的字面子有特殊含义，或无法通过键盘直接输入，表示其字面值时，需要使用**转义序列**，如：

| 字符名称 | ASCII符号 | C++字面值 | 十进制ASCII码 |  |
| ---- | ---- | ---- | ---- | ---- |
| 换行符 | NL(LF) | \n | 10 |  |
| 反斜杠 | \ | \\\ | 92 |  |
| 问号 | ? | \\? | 63 |  |
| 单引号 | ' | \\' | 39 |  |
| 双引号 | " | \\" | 34 |  |
	转义序列可以使用八进制或十六进制表示
	例：Ctr+Z ASCII码为26，可以使用 `'\032'`或`'\x1a'`来表示
3. `std::cin` 和 `std::cout` 的特殊处理
	会更具类型进行不同处理（C++特殊）
```C++
	char a;
	std::cin >> a;
	int b;
	std::cin >> b;
	//例：同样输入按键5，a='5',b=5

	char a = 'M';
	int b = a;
	std::cout << a << b;
	//打印结果，a会打印成 M ，b会打印成 77
	std::cout << 'M';
	//注：这将会打印 77
	std::cout.put('M');
	//注：这将会打印 M
```
4. 通用字符名/通用编码名
	与字符一一对应的数字称作码点，
	C++支持 ISO10646 & Unicode（1991年后，两个组织已经统一标准）
	通过`\u`或`'\U'`后面加十六进制位（码点）来表示字符
	`\u`后面加4个十六进制位（`\uXXXX`）,`\U`后面加8个十六进制位（`\UXXXXXXXX`）。
	**注：** 编码名保证 名（码点）致，而不是编码一致。
	* 同样的编码名，在不同规范下对应的编码可能不同（如UTF8、UTF16）
	* 同样的编码规范，不同的编码名的编码规则可能不同（UTF8为变长编码）
	* 通常使用与标识符 或 字符串，不用在字符中，因为编码长度不可控

5. signed char 和 unsigned char
	char默认情况不区分符号，是否有符号，有（C++）语言实现决定
	* 表示码字时，有无符号并无区别
	* 表示整形时，为了确保范围合适，用户可自行选择有符号/无符号
6. wchar_t、char16_t、char32_t
	宽字符，用于表示大型字符集（码字长度超过1字节）
	wchar_t会用两字节（64位下为4字节）存放字符，对应的适配
```C++
	wchar_t a = L'A';                            //宽字符常量，使用L前缀
	std::wcout << a << std::endl;                //wcin、wcout用于宽字符流输出
	std::wcout << L"ABC" << \                    //宽字符串，同样使用L前缀
	" size is" << sizeof(L"ABC")<< std::endl;    //结果为 ABC size is 16
```
---
	char16_t和char32_t 为 **C++11** 特有类型，都是无符号，主要用于存储UTF-16和UTF-32编码的字符。
	char16_t用前缀u表示，char32_t用前缀U表示。
#### 2.2.6 “字符是如何输出的”

本节主要为个人探究，结合了实际测试（ubuntu gcc 7.5.0环境）、网上查询和个人理解。本节内容则都是围绕着这个核心议题展开：“字符是如何输出的”。
##### 1. 字符集与编码规则
首先，我们需要了解一些基本概念
字符集，是字符字面值的集合，例如 a~z 26个小写字母就可以构成一个字符集；
编码规则，则是规定字符集中的字符，如何转化为可供机器识别的序列。
通常，一个字符，可以存在于多个字符集，一个字符集，也可以对应多个编码规则

常见的字符集有 **ASCII**、**ANSI**、**Unicode**

| 字符集         | 介绍                                                          | 编码规则                                         |
| ----------- | ----------------------------------------------------------- | -------------------------------------------- |
| **ASCII**   | 最常见的基础字符集，包含英文大小写字母、阿拉伯数字等 128个字符。                          | 唯一，详见ASCII码表                                 |
| **ANSI**    | 扩展ASCII字符集，编码时使用 0x00~0x7f表示基础编码，0x80~0xffff表示拓展编码。         | 不同的国家和地区制定了不同的标准（如中文的 GB2312、GBK，日文的JIS）     |
| **Unicode** | 统一码，即上文C++中的“通用字符名”，包括世界大部分的字符，每个字符可与 0-0x10FFFF数字（码点）一一对应。 | 存在不同版本，主要是编码长度和方式有差异（常见的UTF-8、UTF-16、UTF-32） |

**一些要点：**
1. ANSI、Unicode作为拓展字符集，很多都是向下兼容的，例如UTF-8、GBK都可以向下兼容ASCII。
2. 注意区分码点和编码，码字唯一，但对应的编码不唯一，例如Unicode下 字符`∫`对应的码字是 U-222b，然而，经过UTF-8编码后的字节序列为“0xE2 0x88 0xAB”，经过UTF-16编码后的字节序列为“0x22 0x2B”。
3. **纠正一个误区：C/C++中使用ASCII编码**
	这个说法是片面的，只能说C/C++中的字符使用ASCII编码（由于char也代表一字节整型，长度无法适配拓展编码），但是字符串通常都是支持拓展编码的。具体支持和编译器及环境配置有关。
	```C++
	
	printf("中国\n");
	std::cout << "中国" << std::endl;
	//两种方式，均可以输出汉字
	
	```
	
##### 2. C中的宽&窄字符标准输出流

**输入输出流：**
用于程序与外部环境进行数据交互 的抽象概念。
任何流都有输入和输出。程序中的输入流通常是从程序输入，输出到外部文件、设备等，输入流则是从外部文件、设备等输入，输出到程序内部。

篇幅有限，这里只介绍 标准输出流、针对标准输入流、文件输入/输出流等，原理是类似的

**C中的标准输出流：**
```C++
/* Standard streams.  */
extern struct _IO_FILE *stdin;		/* Standard input stream.  */
extern struct _IO_FILE *stdout;		/* Standard output stream.  */
extern struct _IO_FILE *stderr;		/* Standard error output stream.  */
/* C89/C99 say they're macros.  Make them happy.  */
#define stdin stdin
#define stdout stdout
#define stderr stderr
```
上面是C标准库 `stdio.h`中的定义。
可以看到 `stdout`即代标标准输出流，类型为`struct IO_FILE *`(即常用的 FILE 类型，IO_FILE为其底层实现)，侧面说明标准输出流和文件输出流，本质上是近似的。
针对窄字符输出，C提供`printf()`、`puts()`等一系列标准函数
针对宽字符输出，C提供`wprintf()`、`putws()`等一系列标准函数

除此之外，C 标准输出流下 宽&窄字符的输出还有一些特点：
1. 宽&窄字符输出 会 共享标准输出流，使用相同的输出缓冲区
2. C 的流结构（FILE）具有方向的概念，这里的方向通常只有两种：窄字符流 和 宽字符流。
	流 默认是没有方向的，当宽字符 / 窄字符的输入/输出应用与流上时，流便会被定义为相应的方向。
	下面是IOS C中的描述，可以看到流的方向一旦确定，除非调用特定的函数，否则无法改变

> [!NOTE] IOS C standard
> The definition of a stream was changed to include the concept of an orientation for both text and binary streams. After a stream is associated with a file, but before any operations are performed on the stream, the stream is without orientation. If a wide-character input or output function is applied to a stream without orientation, the stream becomes wide-oriented. Likewise, if a byte input or output operation is applied to a stream with orientation, the stream becomes byte-oriented. Thereafter, only the `fwide()` or `freopen()` functions can alter the orientation of a stream.

3. 如果出现宽&窄字符 同时输出，例：`printf()`，`wprintf()`同时使用，会出现未定义的结果。
	* 尝试先调用`printf()`
	```C
	#include <stdio.h>
	
	int main()
	{
		printf("narrow string\n");
		wprintf(L"wide string\n");
		printf("fwide result: %d\n", fwide(stdout, 0));
	}
	```
	输出为：
	```bash
	narrow string
	fwide result: -1
	```
	
	* 尝试先调用`wprintf()`
	```C
	#include <stdio.h>
	
	int main()
	{
		wprintf(L"wide string\n");
		printf("narrow string\n");
		wprintf(L"fwide result: %d\n", fwide(stdout, 0));
	}
	```
	输出为：
	```bash
	wide string
	fwide result: 1
	```

	这里的`int fwide( FILE* stream, int mode )`函数，mode设置为0，可以读取对应流的方向。
	对比可以看到，先调用的输出函数决定了输出流方向，输出流的方向右决定了输出函数的有效性。

##### 3. C++中的宽&窄字符标准输出流

**C++中的标准输出流：**
```C++
  extern istream cin;		/// Linked to standard input
  extern ostream cout;		/// Linked to standard output
  extern ostream cerr;		/// Linked to standard error (unbuffered)
  extern ostream clog;		/// Linked to standard error (buffered)

#ifdef _GLIBCXX_USE_WCHAR_T
  extern wistream wcin;		/// Linked to standard input
  extern wostream wcout;	/// Linked to standard output
  extern wostream wcerr;	/// Linked to standard error (unbuffered)
  extern wostream wclog;	/// Linked to standard error (buffered)
#endif
```
上面是C++ `<iostream>`下的定义。
其中针对窄字符和宽字符分别定义了 cout 和 wcout，且都连接至标准输出。具体使用方式，之前已经反复出现，就不赘述了。

之前用不少篇幅介绍了C中的标准输出流，其实是为了做铺垫，因为C++标准输出流有一个默认特性：**标准输入输出流 会与C 标准输入输出流（stdio）同步**，C++作为后来者，会在很多机制上去兼容C，流的同步，可以避免乱序、冲突等一系列问题。

然而，同步也会带来一些新的问题：
* 效率问题，iostream 的输入输出会同步到 stdio的缓冲区中，使得 iostream 的函数通常会慢于 stdio中的对等功能函数。（以前写编程题时，建议使用printf代替cout提高效率，基本就是这个原因导致的）
* 使 iostream 同样无法同事支持宽&窄字符，上文介绍的 C 标准下 流的特性 同样会影响 iostream。下面是C++标准中的描述：

> [!NOTE] [C++11: 27.4.1/3]
> Mixing operations on corresponding wide- and narrow-character streams follows the same semantics as mixing such operations on `FILE`s, as specified in Amendment 1 of the ISO C standard.

下面同样尝试混用 `cout`、`wcout`：
* demo 1 ：先调用`cout`
```C++
#include <stdio.h>
#include <iostream>
using namespace std;
int main()
{
    cout << "narrow string ∫∫∫" << endl;
    wcout << L"wide string ∫∫∫" << endl;
    cout << "fwide result: " << fwide(stdout, 0) << endl;
}
```
得到的输出如下：
```bash
narrow string ∫∫∫
wide string +++
fwide result: -1
```
* demo 2 ：先调用`wcout`
```C++
#include <stdio.h>
#include <iostream>
using namespace std;
int main()
{
    wcout << L"wide string ∫∫∫" << endl;
    cout << "narrow string ∫∫∫" << endl;
    wcout << L"fwide result: " << fwide(stdout, 0) << endl;
}
```
得到的输出如下：
```bash
wide string ???
fwide result: 1
```

对比结果，可以总结出（在同步stdio的情况下）：
1. `cout` 和 `wcout` 同样会改变 stdout 的方向
2. `cout` 和 `wcout` 的共同使用，同样会有非预期的结果：
	* 宽字符 流向下，cout无法输出，这点和使用 stdio 函数结果相同
	* 窄字符 流向下，wcout可以输出，但是结果异常（个人理解，应当描述为未定义行为，当前输出，只是当前编译器和环境下的特定结果，并非规范的输出）
	* demo1中，输出`+`，个人理解是wcout在同步到stdio时，会根据字节流宽度兼容，如果是窄字符，会截断成窄字符大小，再按窄字符的编码输出。


**C++ 如何同时输出宽字符&窄字符：**
**注：这里只是探讨方法，实际使用最好避免混用的情况，使用不当易造成缓冲区混乱**

下面看demo3：
```C++
#include <iostream>
#include <locale>
using namespace std;
int main()
{
    ios::sync_with_stdio(false);
    locale::global(locale(locale("C"), new codecvt_byname<wchar_t, char, mbstate_t>("")));
    // locale::global(locale(locale("")));
    cout.imbue(locale());
	wcout.imbue(locale());
    cout << "narrow string ∫∫∫" << endl;
    wcout << L"wide string ∫∫∫" << endl;
    wcout << L"fwide result: " << fwide(stdout, 0) << endl;
}
```
输出结果为：
```bash
narrow string ∫∫∫
wide string ∫∫∫
fwide result: 0
```
可以看到宽字符串和窄字符串都能输出，且拓展字符正常输出。
主要有两个步骤：
1. 解除iostream与stdio的同步，保证cout、wcout同时输出
	`ios::sync_with_stdio(false)`可以设置为非同步，当前环境默认为同步。
	C++ 为面对对象编程，多个stream作为多个实例，有着自己的locale及缓冲区，本质上是可以多个流互不影响 同时输出的。
	不过仅仅解除同步，wcout并不能正常输出
	```C++
	ios::sync_with_stdio(false);
	wcout << L"wide string ∫∫∫" << endl;
	```
	上述代码，最终结果是无输出。
	原因是内部编码与外部编码不匹配，C++宽字符采用UTF-32编码，而外部系统中，采用UTF-8编码，无法匹配，wcout调用会失败，可以通过`wcout.fail()`获取到失败。
	（这同样是未定义行为，当前环境下结果为无输出，其他环境可能出现报错）
	
2. 设置locale，保证wcout正常输出
	**首先介绍下locale：**
	 “locale”作为一个抽象概念，通常用于描述与特定地区或语言相关的软件设置，包括一系列的本地化信息，如语言、字符集、日期格式、货币符号等。
	 linux下，通过locale命令，可以查看本地locale：
	```bash
	enjoymove@enjoymove:/em_code/CodeRoot$ locale
	LANG=zh_CN.UTF-8
	LANGUAGE=zh_CN:en
	LC_CTYPE="zh_CN.UTF-8"
	LC_NUMERIC=zh_CN.UTF-8
	LC_TIME=zh_CN.UTF-8
	LC_COLLATE="zh_CN.UTF-8"
	LC_MONETARY=zh_CN.UTF-8
	LC_MESSAGES="zh_CN.UTF-8"
	LC_PAPER=zh_CN.UTF-8
	LC_NAME=zh_CN.UTF-8
	LC_ADDRESS=zh_CN.UTF-8
	LC_TELEPHONE=zh_CN.UTF-8
	LC_MEASUREMENT=zh_CN.UTF-8
	LC_IDENTIFICATION=zh_CN.UTF-8
	LC_ALL=
	```
	 软件中也需要维护自己的”locale“。C/C++中都有着自己的“locale”，
	 C 中的 locale 为全局配置，`<locale.h>`提供了相关的函数；
	 C++中的 locale更为灵活，它被封装成了一个类 `std::locale`，`<locale>`提供了一系列与之相关的类和函数。
	 C++下的cout&wcout都维护着自己的 locale 实例，locale内容默认继承 C的全局locale，即默认为locale C
	```C++
	//通过此方法可以获得cout的locale名称，打印出来为 C
	locale loc = std::cout.getloc();
	std::cout << loc.name() << std::endl;
	```
	
	默认使用locale C，就是导致wcout无法在当前环境正常输出的根本原因，因为locale C 中没有设置宽字符与多字节的转换规则。解决问题的方法就是设置它。
	回到demo3中：
```C++
	locale::global(locale(locale("C"), new codecvt_byname<wchar_t, char, mbstate_t>("")));
    // locale::global(locale(locale("")));
    cout.imbue(locale());
	wcout.imbue(locale());
```
---
	demo3中给出了C++上述设置locale的方法。其中：
	`locale::globle()`用于设置全局locale。
	`codecvt_byname`是C++中描述locale中多字节/宽字符 转换的模板类
	两者结合使用，在添加了多字节/宽字符 转换规则的同时，保证了全局locale的其他元素不变。
	`cout.imbue()`和`wcout.imbue()`则是为输出流设置locale，`locale()`即是获取全局locale。

> [!NOTE] 注：
> 对locale的深入解读，以及为何 `codecvt_byname` 会是wcout输出的关键，见后续章节： [[C++#5. C/C++中的locale（字符相关）]] 

至此C++下的宽窄字符可以同时输出了。尽管如此，还是再次强调**同一个程序尽量单独使用一类字符输出**，以免产生一些未定义的结果。

##### 4. 开发过程中的字符集

计算机中，多处都存在着字符编码和解码，可能涉及到不同的字符集。如：操作系统有自己默认字符集；文本编辑器和IDE会支持一种或多种字符集；编译器也有自己的字符集配置等等。
这里主要结合gcc理解开发过程中常见的两个字符集设置：
1. 源码字符集：
	通常编译器会按特定编码格式解码 源文件，如gcc中默认为UTF-8，可以通过`-finput-charset=charset`设置。
	这可以解释为何经常出现，代码中使用中文常量会出现乱码的情况，例如：开发者编写源码时，编辑器使用ANSI格式存储，编译器按照UTF-8格式解析，由于大部分的变量、函数、及实现都是英文书写，且针对英文 ANSI编码和UTF-8编码存在兼容性，所以很多时候编译不会出错，但是中文在两种格式下会按照不同格式解析，所以出现的是乱码。
2. 运行字符集：
	程序运行时，（宽）字符解析的编码格式，如gcc中默认窄字符为UTF-8，宽字符为UTF-16/UTF-32。可以分别通过`-fexec-charset=charset`和`-fwide-exec-charset=charset`设置。
	换句话说，运行字符集决定目标文件中的字符串按什么格式编码，或是说决定了程序运行时字符串按什么编码方式存放在内存中（sizeof计算出的就是字符串按此方式编码后 占用的大小）。

下面是gcc中关于两个字符集设置的描述：

> [!NOTE] man g++
> **-fexec-charset=charset**
> 	Set the execution character set, used for string and character constants.  The default is UTF-8.  charset can be any encoding supported by the system's "iconv" library routine.
> 	
>** -fwide-exec-charset=charset**
> 	Set the wide execution character set, used for wide string and character constants.  The default is UTF-32 or UTF-16, whichever corresponds to the width of "wchar_t".  As with -fexec-charset, charset can be any encoding supported by the system's "iconv" library routine; however, you will have problems with encodings that do not fit exactly in "wchar_t".

---

可以看出，代码中的字符串字面值，是两个字符集的公共部分，蕴含了“解码+编码”的过程：
源码中的字符串，按照源码字符集解码为 编译器认为的字符串字面值
字符串字面值，又按照运行字符集编码为 目标文件中的实际数据

结合gcc，我们可以用一个具体的案例来感受：
```C
#include <stdio.h>
int main()
{
	unsigned char *buf="abc中";
	for(int i=0; buf[i]; i++)
	{
		printf("%02x ", buf[i]);
	}
	printf("\n%s\n", buf);
}
```
linux环境，在不同情况下，上述代码使用gcc编译运行后的输出如下：
```bash
#源文件采用UTF-8编码，gcc无额外配置
61 62 63 e4 b8 ad
abc中

#源文件采用UTF-8编码，gcc添加选项 -finput-charset=utf-8 -fexec-charset=gbk
61 62 63 d6 d0
abc��

#源文件采用GBK编码，gcc无额外配置
61 62 63 d6 d0
abc��

#源文件采用GBK编码，gcc添加选项 -finput-charset=gbk -fexec-charset=utf-8
61 62 63 e4 b8 ad
abc中
```

测试的过程和结果，有些有意思的点：
1. gcc默认`finput-charset`和`fexec-charset` 都是 UTF-8，gbk格式源文件为何可以正常编译，且输出字符串符合gbk编码？
	首先，这个过程中仍然符合前面说的“解码+编码”的过程，对于英文代码，两种编码方式没有区别，所以可以正常编译，对于`"abc中"`这个字符串，gbk格式经过UTF-8解码后，含义已经变化了，编译器会解读为`"abc��"`类似的乱码，不过编码使用的规则也是UTF-8，所以最终的结果从数值角度没有发生变化
	但这其实是一种“欺骗”编译器的行为，如果反过来，结果会不同：
```bash
#utf.c代表UTF-8格式代码，源码字符集和运行字符集都选择gbk
xxx@exxx:~$ gcc -finput-charset=gbk -fexec-charset=gbk -o test2 utf.c
cc1: error: failure to convert gbk to UTF-8
#运行字符集修改，依旧报错
xxx@exxx:~$ gcc -finput-charset=gbk -fexec-charset=utf-8 -o test2 utf.c
cc1: error: failure to convert gbk to UTF-8
```
---
	在本人的linux环境下，gcc编译根本无法通过。
	所以，尽量保证：**源码字符集和源文件编码匹配。**
2. 即使经过编译配置后，gbk格式源码可以在UTF-8环境下编译，但仍不能完全兼容。
	输出结果可以看到，gbk源码编译运行后，终端输出的中文字符依旧为乱码，这是因为linux终端本身只支持UTF-8格式。
	所以，尽量保证：**运行字符集与系统locale编码类型保持匹配。**

> [!NOTE] 注：源码字符集的修改，不要改变字符集的宽窄性质。
> 例如linux下，即使你可以使用宽字符编码（如UTF-16）去编写新开发的代码，但是标准头文件和第三方头文件，都是用系统默认字符集（UTF-8）编写的。这种情况下，引入的头文件会因与源码字符集不匹配而变成乱码。

##### 5. C/C++中的locale（字符相关）

关于locale可以讨论的细节太多了，本小节主要基于一个问题展开，说一些个人的理解。
这里先抛出之前困扰我很久的问题：
	**locale C 默认的编码集是ASCII，为何通过过printf/cout能够打印出拓展字符串（如中文）？**

C中提供了接口`nl_langinfo`，去获取当前全局locale中的编码集设置：
```C
#include <stdio.h>
#include <langinfo.h>
#include <locale.h>
int main()
{
    printf("loclae C : %s\n", nl_langinfo(CODESET));
    printf("中文\n");
    setlocale(LC_ALL, "");
    printf("loclae \"\": %s\n", nl_langinfo(CODESET));
    printf("中文\n");
}
```
上述代码，输出为：
```bash
loclae C : ANSI_X3.4-1968
中文
loclae "": UTF-8
中文
```
结果正如问题所述，似乎不管locale中的编码集设置如何，中文（拓展字符）都能够正常显示。

经过一段时间学习，我对这个问题有了新的解读：
1. 正确理解locale在软件过程中的定位。
	locale的作用 是描述本地化规则，它是连接软件和本地环境的桥梁，从编码集的角度，locale是为软件编码集 到 本地编码集的适配提供一系列规则和方法（诸如 字符转换、字符比较等等）。但是locale本身，不会决定软件 和 本地环境采用什么样的编码。
	经过之前的章节，我们知道：
		软件的编码集，是由编译器决定的；
		本地环境的编码集，是由操作系统决定的。
	而locale中所定义的编码集（如：`ANSI_X3.4-1968`，`UTF-8`）,可以理解为是目标编码集，而不是实际编码集，locale所描述的方法也是围绕目标编码集展开的。
	放到C++就很好理解了，locale 影响的并不是软件中的字符和字符串，而是流（stream），流则是沟通软件与外部的媒介。
	
	再回到问题，就可以理解，为何在编码集为 ASCII的locale C下，程序仍能够理解拓展字符，终端也仍然能够打印拓展字符。

2. 正确理解“多字节”的概念
	就字符而言，我们通常可以划分为窄字符和宽字符
	就字符串/字符序列而言，我们划分为多字节串 和 宽字符串更为合适。
		宽字符串，是由固定长度（大于1字节）的字符构成的串；
		多字节串，是由变长（一个或多个字节）字符构成的串；
	可以看出，多字节串不仅包含普通的窄字符串（char），也包含了变长字符串（utf-8等）。
	这是由于，软件在处理这些字符串时的逻辑是相同，都是一个字节逐一读取。

> [!NOTE] 注：
> 从软件逻辑的角度出发，前文提到的“窄字符串”/“窄字符流”，替换为“多字节串”/“多字节流”应该更为合理

	
	目前大部分场景，多字节编码更为普遍，原因有：
	* **兼容性**：一是体现在不同编码规则上，大部分变长字符编码 是向下兼容 传统单字符编码ASCII的；二是体现在不同平台上，多字节编码不需要考虑大小端限制，不同平台上的存储格式是一致的
	* **高效性**：主要体现在存储上，变长字符会根据实际字符定义编码长度，而宽字符则是固定长度，存储时可能存在冗余
	* **标准化**：多字节编码，尤其是UTF-8，已经成为了很多标准的指定格式，如HTML、XML等，这反向由导致了更多的标准去使用多字节编码
	
	回到问题中，ASCII与UTF-8同为多字节编码，从数据流的角度来说，二者没有区别，数据流更关心的是数据的形式，而不是含义。从printf/cout的角度，标准输出流按ASCII形式输出的是多字节流，终端按UTF-8形式读入的也是多字节流，不存在沟通的障碍，而字节流中的字符串，已经是UTF-8编码了（编译器决定），所以能够正常输出。

综上，我们可以对问题做出总结：
	软件中字符串正确输出到终端的两个关键条件：
		1.  输出流的两端要采用一致的编码规则
		2.  输出流 locale 指定编码集 所采用的编码类型（宽字符/多字节），要与两端编码规则的类型一致

---

然而C/C++中除了普通字符，还有宽字符和宽字符串的概念，他们通过wprintf()/wcout之类函数输出。那软件是如何保证这些宽字符串正常输出到终端的呢？

其实本质上，输出条件和之前提到的两个关键条件一样。不过大多数情况，外部编码（终端）都是采用多字节编码，在输出流 输出宽字符串前，会将宽字符流转化为与外部编码匹配的多字节编码格式。**“编码转换”** 就是宽字符串输出的关键

C++ locale类中，有这多个策略集（facet），而编码转换策略（codevrt）就是众多策略集中的一个（C中的形式还不清楚），默认的locale C中，并没有制定 宽字符串 至 多字节串的转换策略，所以就可以解释，之前直接调用wprintf()/wcout输出宽字符串，并不会得到正常的输出结果。

说到这就可以再次回顾之前[[C++#3. C++中的宽&窄字符标准输出流]]提到的locale设置方法
```C++
	locale::global(locale(locale("C"), new codecvt_byname<wchar_t, char, mbstate_t>\
	("zh_CN.UTF-8")))
```
`std::codecvt`是C++描述编码转换策略的类， 而`codecvt_byname<wchar_t, char, mbstate_t>()`，是`std::codecvt`的一个派生，描述的其实就是内部和外部编码的转换策略，前两个参数，就代表着：内部编码为 wchar_t（gcc下是宽字符的UTF-32），外部编码为char（linux下是多字节的UTF-8）。
再往外一层，则用到了locale的一种构造函数`template<typename _Facet> locale(const locale& __other, _Facet* __f)`，会在已有locale上，替换指定策略（facet）来生成新的locale。

所以这行调用，其实就是在C locale的基础上，添加了宽字符/多字节的转换策略。从而使得wcout可以正常输出。

##### 6. 小结
这一小节，很多内容已经不局限于C++了，还有对字符集、编码规则、编码本地化等等知识点的探讨，这些知识点延伸开来，也是一个很复杂的领域。
本节 “字符集的拓展研究”，来来回回也耗费了一周时间整理，内容大多都是结合实际编码和测试总结出来的。整理的目的，一是为了记录下自己思考的过程，方便反思，二是为了让读者能够结合实际开发，更好的理解这些知识点。
当然，想更全面的了解其中的一些细节，自行去查阅相关的官方文档，更为可靠。

#### 2.2.7 bool类型
ANSI/ISO C++标准添加的新类型，字面值有两类：true 和 false

**bool的隐式转换：**
* bool类型可以被提升转换为int类型：
	true被转化为1，false被转化为0
	```C++
	int ans = ture;       //转换为 1
	int promise = false;  //转换为 0
	```
* 任意数字值可以被隐式转换为bool类型
	非零被转化为true，零被转化为false
	```C++
	bool start = -100;    //转化为1
	bool stop = 0;        //转化为false
	```

### 2.3 const限定符
const限定符用于描述常量，常量一旦定义则不可被修改（所以常量在声明的同时需要初始化）

**const 常量区别于 \#define**：
1. const可以明确常量类型
2. C++下const常量作用域与一般变量相同，有特定的作用域
3. const可以用于描述更复杂的数据类型（如 数组）

### 2.4 浮点数
编码通常有两部分，一部分表示值，一部分表示小数点的位置，即控制小数点的移动，因此得名浮点数
#### 2.4.1 浮点数的字面值
通常有两种表示形式=、】

1. 标准小数点表示：
	如：2.24、3.0
	注：广义上的小数点，在如欧洲等一些地区数制里，小数点可能是逗号而不是句号
2. E表示法（类似科学计数法）：
	如：2.2E6、-12e-12
	E表示法的标准模板：整数或小数+(E/e)+整数。
	注：这里 E/e 后面跟随的整数‘n’，表示乘 10的n次方，而不是乘 2的n次方。

#### 2.4.2 浮点类型
C++浮点类型主要有三类：float、double、long double。
针对这三种浮点类型：

|             | 有效值范围             | 指数范围                |
| ----------- | ----------------- | ------------------- |
| float       | 位数不少于32           | 十进制下位数需超过\[-37,37\] |
| double      | 位数不少于48，且不少于float | 十进制下位数需超过\[-37,37\] |
| long double | 位数不少于double       | 十进制下位数需超过\[-37,37\] |
实际语言中浮点类型的取值范围，需要参考头文件`<cfloat>`（C中是`<float.h>`），下面以gcc7.5.0为例，列举一些其中的配置（其中 `"//"`后的注释为个人添加）：
```C
// <float.h>

/* Radix of exponent representation, b. */
#undef FLT_RADIX                          // 避免重定义
#define FLT_RADIX   __FLT_RADIX__         // 2，所有浮点类型 指数的基数

/* Number of base-FLT_RADIX digits in the significand, p.  */
......
#define FLT_MANT_DIG	__FLT_MANT_DIG__     // 24, b进制下有效值的位数（包含符号部分）
#define DBL_MANT_DIG	__DBL_MANT_DIG__     // 53
#define LDBL_MANT_DIG	__LDBL_MANT_DIG__    // 64

/* Number of decimal digits, q, such that any floating-point number with q
   decimal digits can be rounded into a floating-point number with p radix b
   digits and back again without change to the q decimal digits,

	p * log10(b)			if b is a power of 10
	floor((p - 1) * log10(b))	otherwise
*/
......
#define FLT_DIG		__FLT_DIG__             // 6, 十进制，小数点后确保精度的最大位数
#define DBL_DIG		__DBL_DIG__             // 15
#define LDBL_DIG	__LDBL_DIG__            // 18

/* Minimum int x such that FLT_RADIX**(x-1) is a normalized float, emin */
......
#define FLT_MIN_EXP	__FLT_MIN_EXP__         // -125， b进制下，规格化浮点数，指数最小位数+1
#define DBL_MIN_EXP	__DBL_MIN_EXP__         // -1021
#define LDBL_MIN_EXP	__LDBL_MIN_EXP__    // -16381

/* Maximum int x such that FLT_RADIX**(x-1) is a representable float, emax.  */
......
#define FLT_MAX_EXP	__FLT_MAX_EXP__         // 128， b进制下，可表示浮点数，指数最大位数+1
#define DBL_MAX_EXP	__DBL_MAX_EXP__         // 1024
#define LDBL_MAX_EXP	__LDBL_MAX_EXP__    // 16384
......
```

这里有一个较为独特的地方，以`float`为例，`__FLT_MAX_EXP__`为128，而`__FLT_MIN_EXP__`为-125，这两个值是如何算出来的？

要解释这两个值，就要介绍下浮点数在实际使用中的存储格式：

#### 2.4.3 浮点数存储格式

常用的浮点数格式，由IEEE指定，目前C/C++使用的浮点数格式，就是来自IEEE 754标准。

标准下，二进制浮点数通用格式：$(-1)^S * M * 2^E$ 
其中：
$(-1)^S$ 表示符号位，S为0，浮点数为正，S为1，浮点数为负；
$M$ 表示有效数字，范围为 \[1, 2\)；
$2^E$ 表示指数位。
浮点数存储时，会分别存储S、E、M，记作符号位、指数、尾数

> [!NOTE] 注：
> M的形式为1.xxxxxx，意味着其首位必定为1，所以IEEE规定，存储时，会省去首位的1，只保存后续尾数。
> 这样做可以额外节省一个有效位。

对应到实际：
单精度浮点数 float，格式为：符号位(1bits) + 指数位(8bits) + 尾数位(23bits)
双精度浮点数 double，格式为：符号位(1bits) + 指数位(11bits) + 尾数位(52bits)

这里比较特殊的是指数位：
当前格式下，指数以无符号数存储，只能取非负数。实际数值中的E可正可负，为了做到这一点，引入了**中间数**的概念，之间的关系为：
	**E = 指数位数值 - 中间数**
以float为例：指数位占 8 bits，数值范围为\[0, 255\]，而在换算成真实值时，会统一减去中间数 127，从而使得 E 真实值的取值范围达到 \[-127, 128\]。

但是，E取值为两端时（全0/全1）,会代表其他的含义

| 指数位 | 尾数位  | 含义                                                              |
| --- | ---- | --------------------------------------------------------------- |
| 全为0 | 全为0  | 正常值，表示浮点数0                                                      |
| 全为0 | 不全为0 | 非规格化数，这时E=1-中间数（float下即-126），M不在补充首位1，仅有尾数构成小数部分。表示无限趋近于+0/-0的值 |
| 全为1 | 全为0  | 特殊值，表示+∞/-∞                                                     |
| 全为1 | 不全为0 | 特殊值，Nam（Not a Number），通常表示计算中错误或非法的结果                           |
补充定义下“规格化浮点数” 和 “可表示浮点数”：
**可表示浮点数：**
表示根据编码，可以映射到具体数值的浮点数
**规格化浮点数：**
通常是可表示浮点数的子集，在此基础上，它还要求有有效值采用补0策略，即M需要为 1.xxxxxx
例如：在float格式下，规格化浮点数 $1.1*2^{-128}$ 的指数 -128，是无法用 8bits的指数位表示的，但是，如果转化为 $0.011 * 2^{-126}$ 的形式，指数为-126，是可以用 8bits的指数为表示的。按照上文浮点数的规则，可以编码为 0000 0000 0011 0000 0000 0000 0000 0000， 这就是一个可表示但非规格化的浮点数。
可以看出，出去规格化限制，浮点数往往可以表示得更小。

回过头来看之前`<float.h>`中的定义（以float为例）：
```C
/* Minimum int x such that FLT_RADIX**(x-1) is a normalized float, emin */
......
#define FLT_MIN_EXP	__FLT_MIN_EXP__         // -125

/* Maximum int x such that FLT_RADIX**(x-1) is a representable float, emax.  */
......
#define FLT_MAX_EXP	__FLT_MAX_EXP__         // 128

```
我们将注释翻译过来，并结合之前的知识做了解读：
*  **最小值x，使得 $2^{x-1}$ 为规格化浮点数**
	float下，指数位最小为全0，然而全0时，除了0以外，只能表示非规格化浮点数，规格化下，指数位取到的最小值为 1，对应的E 为 -126(1-127)。规格化下，满足2的整数幂的M只有1，此时尾数位为全0。综合可得，$2^{-126}$是满足情况的极限，此时x= -125。
	而float下是有比 $2^{-126}$ 更下的2的整数幂呢？当然是有的，但是会超出规格化的范畴，这就是最小值和后面最大值描述有区别的原因。
*  **最大值x，使得 $2^{x-1}$ 为可表示浮点数**
	float下，指数位最大为全1，然而全1的情况都为特殊值，不属于可表示浮点数范畴。规格化下，指数位取到的最大致为 254 ($2^{8}-1$)，对应的E 为 127(254-127)，规格化下，满足2的整数幂的M只有1，此时尾数位为全0。综合可得，$2^{127}$ 是满足情况的极限，此时x=128。


#### 2.4.4 浮点常量

浮点常量与整型常量类似，书写时，使用浮点数字面值的格式即可。浮点常量的默认类型为double。然而可以使用添加后缀的方式来限定浮点数类型：
	f或F后缀，表示常量为float类型
	l或L后缀，表示常量为long double类型

下面是一些例子
```C++
#include <iostream>
using namespace std;

int main()
{
	float num1 = 1.1f;
	float num2 = 2.0E3F;
	long double num3 = 2.25L;
	long double num4 = 36893488147419103232L;      // 2^65
	long double num5 = 36893488147419103232.0L;    // 2^65
	long double num6 = 368935e14L;                 // 约为2^65
    cout << num1 << endl << num2 << endl;
    cout << num3 << endl << num4 << endl << num5 << endl << num6 << endl;	
}
```
测试结果如下：
```bash
xxx@exxx:~$ g++ -o test test.cpp
test.cpp:9:24: warning: integer constant is too large for its type
     long double num4 = 36893488147419103232L;
xxx@exxx:~$ ./test 
1.1
2000
2.25
0
3.68935e+19 
```

注：l/L后缀同样用于描述 整型中的long类型，具体含义，要看字面值格式
从程序的结果我们可以发现，同一个数字（这里取 $2^{65}$，超出了long的范围，但是未超过long double的范围），在未加浮点时，会被判断为整数，在加浮点时，会被判断为浮点数。

**综上，在书写浮点常量时，即使不含小数部分，最好也要将“浮点”显示表现出来。**

#### 2.4.5 浮点数的优劣（相较于整数）
优点：
1. 能够填补相邻整型之间的空缺。
2. 相同长度，可以表示的数值范围更大。
缺点：
1. 相同长度，可以表示的精度更小。
2. 运算效率往往会更低

下面的代码可以看出浮点型的精度问题：
```C++
#include <iostream>
using namespace std;

int main()
{
    float num1 = 1.2E6f;
    float num2 = num1 + 1;
    cout << num1 << endl << num2 << endl << num2-num1 << endl;
    float num3 = 1.2E15f;
    float num4 = num3 + 1;
    cout << num3 << endl << num4 << endl << num4-num3 << endl;
}
```

输出结果为：
```bash
1.2e+06
1.2e+06
1
1.2e+15
1.2e+15
0
```
其中，
num1+1，相当于在有效数的小数点后第6位加1，在十进制进度 `__FLT_DIG__`(6)的范围内，所以num2-num1可以正常计算出偏差。
注：这里num1、num2终端打印出来数值一致，是因为终端浮点数打印精度不足导致，而不是C++浮点数进制不足。
num3+1，相当于在有效数的小数点后第15位加1，超出了`__FLT_DIG__`的范围，所以从num4-num3来看，加1并没有产生数值上的偏差。

---
> [!NOTE] 无符号整型+有符号整型统称为整型，整型+浮点型统称为算术类型。
> 
---


### 2.5 算术运算符

#### 2.5.1 概念
算术运算符也可叫算术操作符，是运算符/操作符的一个子集，主要用于处理两个值（操作数）来得到算术结果。
常见的算术运算符有五个：
加法（+）、减法（-）、乘法（\*）、除法（/）、取模（%）。
算术运算符的对象可以是前文提到的各种基本类型，可以是变量也可以是常量，可以是整数也可以是浮点数（部分运算符会有限制，例如%的操作数只能是整数）。
往后可能还会出现，在复合类型上使用这些算术运算符，这在C中通常是不允许的，但是C++中“运算符重载”的概念，可以实现诸如 string+string之类的操作。

#### 2.5.2 运算符优先级和结合性

优先级和结合性，主要是用于处理同一个表达式中存在多个远算符的情况：
1. 当出现不同运算符时，优先级起作用，先使用优先级高的运算符
2. 当出现优先级相同的运算符作用同一个操作数，结合性起作用，根据结合性决定使用顺序：从左到右（L-R）或是从右到左（R-L）。
3. 当出现优先级相同的运算符，但作用不同的操作数时，由实现决定。
	注：“由实现决定”，意味着不同系统可能存在不同定义。

例：
```C++
int a = 3 + 4 * 2;          //a=11，*优先
int b = (3 + 4) * 2;        //b=14, ()用于值构造，优先级高于几乎所有运算符
int c = 3 * 4 / 2;          //c=6，*、/优先级相同，所以按L-R结合顺序运算
```
对于算术运算符，`'*'`、`'/'`、`'%'` 优先级相同，`'+'`、`'-'`优先级相同，前者优先级高于后者优先级，结合性均是从左到右（L-R）。
C++中除了算术运算符以外的其他所有运算符，都应当参考规定的优先级和结合性，其中优先级可以分为18个等级
具体规则，可以参考[C++PrimerPlus]中附录D的"运算符优先级"章节。

优先级表中，可以看到括号 ()，拥有第二级优先级，所以在表达式复杂 或者优先级不确定时，尽量使用括号来明确运算顺序，例如 `(a*b)+(c*d)` 这样的写法，MISRA C++中也是这样建议的。

> [!NOTE] 编码时，需明确：正确性 > 可读性 > 简洁性
> 

### 2.6 类型转换

#### 2.6.1 示例
结合上一节运算符的介绍，这里先展示一个除法运算的例子：
```C++
#include <iostream>
using namespace std;

int main()
{
    cout.setf(ios_base::fixed, ios_base::floatfield);
    cout << "int: 9/5 = " << 9/5 << endl;
    cout << "flt: 9.0f/5.0f = " << 9.0f/5.0f << endl;
    cout << "mix: 9.0f/5 = " << 9.0f/5 << endl;
    cout << "huge flt:  = " << 1e7f/9.0f << endl;
    cout << "huge double: 9/5 = " << 1e7/9.0 << endl;
}
```


> [!NOTE] cout.setf(ios_base::fixed, ios_base::floatfield);
> setf函数用于设定cout的输出格式，这里设置浮点数为定点输出，便于观测输出

输出为：
```bash
int: 9/5 = 1
flt: 9.0f/5.0f = 1.800000
mix: 9.0f/5 = 1.800000
huge flt:  = 1111111.125000
huge double: 9/5 = 1111111.111111
```
从结果我们可以发现一些规律：
* 两组操作数类型不同，运算方式可能不同。
	C++中，运算符根据上下文不同可能含义不同，C++将这种相同运算符的多种操作叫做“运算符重载”
* 一组操作数内，类型不同，会先转为相同类型，在进行运算。
	如 `9.0f/5` ，(int)5 被转化为了 float类型，在进行运算。


`9.0f/5`中，(int)5 到 (float)5 的转换，就是C++中的一种（隐式）类型转换。

#### 2.6.2 转换规则

通常的类型分为两类：
自动的类型转换（**隐式类型转换**）和强制的类型转换（**显式类型转换**）。

一些典型转换情景：
##### 1. (初始化)赋值时的转换
C++下，部分情况下，允许将一种类型的值赋给另一种类型的变量，值将被转化为接收变量的类型。
通常情况下，
赋值给 大范围、高精度的类型时，是安全的；
赋值给 小范围、低精度的类型时，会产生一些潜在的问题。

下面列出一些可能存在问题的转换场景：
1. 大整型->小整型
```C
int a = 0x7FFFFFFFu;     // 2147483647
int b = 0x1FFFFFFFFu;    // -1（储值为0xFFFFFFFF）
unsigned int c = 0x1FFFFFFFFu;    // 4294967295（储值为0xFFFFFFFF）
```
无论有符号，还是无符号，整型溢出时，都是截取低有效位

2. 大浮点型->小浮点型
```C
//大浮点->小浮点
double qr(int i);  // 假定此函数返回 2^i (double类型)
double a1 = qr(-128);
float a2 = a1;
double b1 = qr(128);
float b2 = 1;
double c1 = qr(-256);
float c2 = c1;
double d1 = 0.0123456789;
float d2 = d1;
printf("%e\n%016lx\n", a1, *((uint64_t *)(&a1)));  //2.938736e-39  37f0000000000000
printf("%e\n%08x\n", a2, *((uint32_t *)(&a2)));    //2.938736e-39  00200000
printf("%e\n%016lx\n", b1, *((uint64_t *)(&b1)));  //3.402824e+38  47f0000000000000
printf("%e\n%08x\n", b2, *((uint32_t *)(&b2)));    //inf           7f800000
printf("%e\n%016lx\n", c1, *((uint64_t *)(&c1)));  //8.636169e-78  2ff0000000000000
printf("%e\n%08x\n", c2, *((uint32_t *)(&c2)));    //0.000000e+00  00000000
printf("%.10e\n%016lx\n", d1, *((uint64_t *)(&d1)));
//1.2345678900e-02  3f8948b0f8fab5e6
printf("%.10e\n%08x\n", d2, *((uint32_t *)(&d2)));
//1.2345679104e-02  3c4a4588
```
- 2^-128，已超出了float的最小格式化数范围，不过可以看到float是可以存储的，只是会转化成非格式化存储（指数位全0）。
- 2^128，同样超出了float的最大格式化数范围，此时表现为无穷大（**取决于实现**）。
- 2^-256，已超出了float非格式化数的范围，直接转化为 0（**取决于实现**）。
- 0.0123456789，超出了float的精度限制（7位十进制左右的精度），此时精度会有损失。根据存储值可以推算出真实值：
	double：0x3f8948b0f8fab5e6    ->   0x1.948b0f8fab5e6 3 * 2^(-7)
	float：0x3c4a4588   ->   0x1.948b10 * 2^(-7)
可以看出，在当前环境下，浮点精度有限时，采用了舍去有效位的低位，按四舍五入取近似取值（**取决于实现**）。

> [!NOTE] printf打印浮点数
>* 使用%f或%lf以格式化打印浮点数，使用%.nf可以限定小数点后输出的位数
>	注意，printf中%f 和 %lf实质上没有区别，是由于C语言中的默认参数提升规则，即printf的浮点型参数会被提升为范围更大的double；这并不代表%f 和 %lf没有区别，例如 scanf中，%f用于读入float，而%lf用于读入double
>* 使用%e以科学记数法打印浮点数，使用%.ne可以限电有效位小数点后输出的位数

3. 整型->浮点型
```C++
int a1 = 123456789;
float a2 = (float)a1;
printf("%08x\n", a1);                           //075bcd15
printf("%f\n%08x\n", a2, *((uint32_t *)(&a2))); //123456792.000000  4ceb79a3
```
通常浮点型的数值范围会大于整型，但精度会小于整型
- 123456789，超出了float的精度限制，同样的，根据存储值推算出真实值：
	int：0x75bcd15   ->   0x1.d6f3454 * 2^(26)
	float：0x4ceb79a3   ->   0x1.d6f346 * 2^(26)
可以看出，这里浮点在精度有限时，同样采用舍去有效位的低位，按四舍五入近似取值（**取决于实现**）。

4. 浮点型->整型
```C++
float a1 = 1.5;
int a2 = a1;
float b1 = qr(33);
int b2 = b1;
unsigned int b3 = b1;
printf("%d\n%08x\n", a2, a2);  //1  00000001
printf("%d\n%08x\n", b2, b2);  //-2147483648  80000000
printf("%u\n%08x\n", b3, b3);  //0  00000000
```
* 1.5，转换为整型时，会直接舍弃小数位
* 2^33，超出了整型的最大范围（2^31），会被转换为可表示的最小整型，无符号为0，有符号为 -2147483648（**取决于实现**）。

5. 整型<->bool型
整型非0 转换为 true，整型0 转换为 false；
true 转换为 1，false 转换为 0。
其他基础数字类型（浮点、无符号等）与bool型转换规则类似。

> [!NOTE] 存储格式不同的数据类型转换，都是危险的！不建议使用！
> * 转换后会得到意料之外的值，例如：大整型->小整型，正数向上溢出会得到负数
> * 转换后会产生未定义的行为，例如：浮点->整型，数值溢出，某些情况会得到 INT_MIN，某些情况会得到 INT_MAX，某些甚至会直接报错。这都取决于系统实现，C/C++本身未定义。

##### 2. 以{}方式初始化时的转换（C++11）
C++11后允许的列表初始化，在初始化复杂数据类型时，会更常见，用于提供值列表。
相较于普通初始化，列表初始化会更严格：列表初始化不允许**缩窄(narrowing)**，即变量类型可能无法表示赋给他的值
```C++
//展示 int -> char
const int code = 66;
int x = 66;
char c1 = {31325};     //不允许，31325超出char范围 [-Enarrowing]
char c2 = {66};        //允许
char c3 = {x};         //不允许，x为 int变量，存在超出char范围的可能 [-Wnarrowing]
char c4 = {code};      //允许，code为常量
int y = 31325;
char c5 = y;       //允许，普通初始化时的转换不考虑缩窄
char c6 = 31325;   //不允许，产生了溢出 [-Woverflow]
```

> [!NOTE] 注：不允许，代表被编译器判定为问题，不过判定为Error/Warning取决于编译器和编译选项
> 

##### 3. 表达式中的转换
主要存在两种转换：
1. 一些类型，在表达式中出现时便会被自动转换。
	例如：bool、char、unsigned char、signed char、short，在表达式中会被自行转换为int，称为**整型提升**，因为int格式更便于计算机运算。
	整型提升的前提是确保不会产生数据损失，根据范围需要，会一次选择：int、unsigned int、long、unsigned long。
	例如：short和int长度相同时，unsigned short会被整型提升为unsigned int。
```C++
short a = 1;
short b = 2;
short c = a + b;  //(short)((int)a+(int)b)
```

> [!NOTE] 整型提升是自行的，与表达式中的其他行为无关
> 

2. 表达式中存在不同类型时，较小类型会被转换为较大类型，最终保证转换后操作数类型一致
	针对这类场景，编译器会遵循一份校验表，以C++11的校验表为例，依次校验：
	* 操作数中存在浮点型，优先选择高级的浮点类型。
		浮点型从高到低：long double、double、float
	* 操作数中无浮点型，即都为整型，且同为有符号/无符号，优先选择级别高的类型。
		整型从高到低：(unsigned)long long、(unsigned)long、(unsigned)int、(unsigned)short、(unsigned)char
	* 操作数分别为有符号和无符号时，无符号类型级别高，选择无符号操作数类型
		例：unsigned long long + long -> unsigned long long + unsigned long long
	* 无符号类型级别较低，且有符号类型可表示无符号类型所有取值，选择有符号操作数类型
		例：unsigned int + long -> long + long
	* 有符号类型无法表示无符号类型所有值，选择有符号操作数类型的无符号版本
		例：unsigned long + long long -> unsigned long long + unsigned long long
		
	**注意，这种转换不能保证数据准确，存在数据损失或异常的情况**

##### 4. 传递参数时的转换
针对未声明函数原型的函数，可能存在参数提升。例如：旧式C中存在不带参数类型的函数声明，类似`void func(a, b);`，这种情况可能出现整型升级等带来的转换。

> [!NOTE] 注：当前C/C++通常都要求或建议使用函数原型来声明函数，所以此类型转换场景可以忽略。
> 


##### 5. 强制类型转换
强制类型转换，即显式类型转换，需要通过某种强制类型转换机制进行显式的类型转换。
通常有几种形式：
* `(typeName)value`，C/C++方式
*  `typeName(value)`，C++方式，类似于构造函数
*  4种强制类型转换运算符，例：`static_cast<typeName>(value)`，具体用法后续会有详细介绍

> [!NOTE] 强制类型转换 不会直接转换原始数据
> 例如：
> 	int a = 1;
> 	long b = (long)a;
> 实际会有个临时变量作为类型转换的媒介，过程近似为：
> 	int a = 1;
> 	long tmp = a;
> 	long b = tmp;

强制类型转换，主要用于编译器类型转换无法覆盖，但实际需要的场景
	例如：想让字符类型数据按整型格式输出，可以用到` std::cout << (int)('a');`

**注意：强制类型转换，有时会绕过编译器的类型检查，是危险的行为，需要慎用！**

### 2.7 C++的auto声明

C++中的 "auto" 关键字，用于类型推导，根据初始值的类型推断变量的类型。
```C++
auto a = 100;           //a 为 int
auto b = 100.0;         //b 为 double
std::vector<double> c;
auto pv = c.begin();    //c 为 std::vector<double>::iterator
```
在使用复杂数据类型时，如声明STL容器的迭代器，“auto” 会更有用

> [!NOTE] C中的 auto
> C中同样有关键字 auto，变量声明时使用，表示声明的变量在函数调用时创建，函数返回时销毁。即变量具有**自动存储持续时间**。
> 目前局部变量默认都具有 自动存储持续时间，所以auto已经很少使用了。



## 3 复合类型

C++中，复合类型是指由基本类型通过组合、引用或指针等方式构成的类型
### 3.1 数组

用于存储多个同类型的值
格式：
```C++
typeName arrayName[arraySize];
```
typeName 指明了数组类型，不存在通用数组，只存在特定类型的数组（char 数组，int数组等）
arraySize 需要在编译前确定，即只能是常量。
可以通过下标访问数组中的指定数据，下标范围 **\[0, arraySize-1\]**。

```C++
int a[2] = {1,1};
int b[2] = {0,0};
std::cout << b[-1] << std::endl;
std::cout << b[0] << std::endl;
std::cout << b[2] << std::endl;
```
上述代码，存在越界行为，然而编译时并不会报错，**某次**输出如下：
```bash
1           # 向前越界
0           # b[0]
1677983488  # 向后越界
```

> [!NOTE] 针对下标访问，编译器不会检查有效性，所以须人为确保下标有效性，防止越界
> 

#### 3.1.1 数组初始化

未初始化的数组元素为内存初始值，如果需要保证数组的初始值，则需要在定义数组时进行初始化

C++数组通常使用列表初始化（类似C 静态初始化），一些正确示例
```C++
// 列表初始化
int a1[4] = {1,2,3,4};         // 结果为 {1,2,3,4}
int a2[4] = {1,2};             // 结果为 {1,2,0,0}，未填充部分自动填0
int a3[4] = {0};               // 结果为 {0,0,0,0}，同上
int a4[] = {1,2,3,4};          // 结果为 {1,2,3,4}，编译器会根据列表元素个数推定数组大小
char c[] = "HelloWorld";       // 结果为 "HelloWorld"，大小推断 在初始化字符串时常用

// C++11 特性
int a5[4] {1,2,3,4};            // 省略'='
int a6[4]={};                   // 结果为 {0,0,0,0}
```
一些**错误示例**
```C++
int b1[4] = {1,2.0};            // 错误，C++会判定为 narrawing（C可能不报错）
int b2[4] = {[0]=1, [2]=2};     // 错误，C++不支持 （C静态初始化支持）
int b3[] = {}                   // 危险，部分编译器视作空数组，部分视为非法
```

除了普通数组，C++(STL)提供了模板类 vector，C++11新增了模板类array，有着更丰富的用法，详见后文。

### 3.2 字符串

字符串是存储在内存连续字节中的一系列字符
常见的有两种形式：
* char数组，C-风格字符串（C-style string）
* string类，C++基本库

#### 3.2.1 C-style string

C-style string，通常存储在char 数组中，
格式有一个固定要求：需要以 空字符 结尾，空字符为`'\0'`，ASCII码为0。

##### 1. 初始化
字符串初始化，除了数组通用的方式外，还可以使用字符串常量来初始化
```C++
char str[4] = {'a','b','c','\0'}; // 正确
char str1[11] = "Helloworld";     // 正确
char str2[15] = "Helloworld";     // 正确，剩余字节会被赋为'\0'
char str3[] = "Helloworld";       // 正确

char str4[10] = "Helloworld";     // 错误，字符串常量结尾隐含'\0',所以定义数组长度不足
                                  // （C编译器可能支持，但会进行截断，截去末尾'\0'）
```

> [!NOTE] C 中 char str3[10] = "Helloworld"；
> 不会报错，但是可能得到非预期结果。读取str3时，会读到内存中下一个`'\0'`为止，可能会超出原先设定的字符串范围

##### 2. 字符串常量拼接
C++特有，任何两个由空白分隔的字符串常量（字符串字面值）都将自动拼接成一个
```C++
char s[15] = "Hello" " "
			"World";         // 结果为 "Hello World"
```

##### 3. 字符串输入
* std::cin
	std::cin 使用空白（空格、制表符、换行符）来确定字符串的结束位置
```C++
char s1[20];
char s2[20];
std::cin >> s1;
std::cin >> s2;
```

针对上述代码，如果一次性输入一行"Hellow world"字符串，最终读入 s1="Hello" s2="world"，空格作为空白，使原输入被cin分块读取。

* std::cin.getline()
	面向行的输入，使用换行符来确定输入结尾。结尾添加空字符，换行符舍弃
	使用示例：
```C++
std::cin.getline(s1,20);    //读取最多19个字符到s1中，结尾填充空字符 
```

* std::cin.get()
	面向行的输入，使用换行符来确定输入结尾。结尾添加空字符，换行符保留，存在输入队列中，这导致了如果不加以手段消化掉换行符，后续输入将无法继续。
	使用用例：
```C++
std::cin.get(s1,20);         //读取最多19个字符到s1中，结尾填充空字符 
std::cin.get();              //消化换行符
```
无参数的get()，表示读取一个字符（包括换行符），通常用于消化换行时遗留在队列中的换行符
	由于getline()/get()均返回一个cin对象，可以拼接调用，用于多次读取
```C++
std::cin.getline(s1,20).getline(s2,20);
std::cin.get(s1,20).get();
```

> [!NOTE]  cin 数字与字符串混合输入问题
> cin 读取数字时，往往会以回车结尾，剩余的回车可能回影响下次字符串的输入。可以尝试使用get解决，例如：
> 	std::cin >> num;
> 	std::cin.get();
> 	或
> 	(std::cin >> num).get(); 



* get(name, length)/getline(name, length)部分异常情况
输入空行：
	getline(name, length)  会读入空字符串（仅读入一个空字符），下一条输入语句从新一行开始读取。
	get(name, length)  会设置失效位（falibit），并且关闭后续输入。此问题很常见，因为只要未处理换行符，单个换行符就会形成空行。
输入字符串长度大于length
	getline(name,length)  会读入至最大长度，设置失效位，并且关闭后续输入。（个人理解，getline此时无法区分是正好读到行尾，还是行内容超出了上限，故标志位区分）
	get(name, length)  会读入至最大长度，并将余下字符留在输入队列中。
	
一些代码示例 (test)：
1. get 空行场景
```C++
char s[10];
std::cin.get(s, 10);                       
std::cout << strlen(s) << std::endl;
std::cin.get(s, 10);
std::cout << std::cin.fail() << "   " << strlen(s) << std::endl;
// 错误处理
std::cin.clear();
std::cin.ignore(100, '\n');
std::cin.get(s,10);
std::cout << std::cin.fail() << "   " << strlen(s) << std::endl;
```
输出:
```bash
xxx@xxx:~$ ./test
abcd
4
1  0
abcd
0  4
```
第一次get后，未处理换行符，第二次get会直接读取后续换行符，并视为空行，通过`cin.fail()`可以获取错误状态，返回true，则说明错在错误。
后续的错误处理：cin.clear() 可以清除错误位，cin.ignore() 可以行剩余内容，这里用于跨过空行，否则后续还会报错。

2. getline 读写过长字符串
```C++
char s[10];
std::cin.getline(s, 5);                       
std::cout << strlen(s) << std::endl;
std::cin.getline(s, 5);
std::cout << std::cin.fail() << "   " << strlen(s) << std::endl;
// 错误处理
std::cin.clear();
std::cin.getline(s, 5);
std::cout << std::cin.fail() << "   " << strlen(s) << std::endl;
```
第一次getline，输入8字节，只能读入4字节，第二次geline无法读入，通过`cin.fail()`可以获取错误状态。
后续的错误处理：cin.clear() 清除错误位，再次调用getline，发现多余字符依然存在输入队列中。

#### 3.2.2 C++ string

C++库提供了string类，专门用于处理字符串，名称空间位std，头文件为 `<string>`。
同样用于处理字符串，string与字符数组有许多相同之处：
* 可以使用C-风格字符串来初始化string对象，包括列表初始化，字符串常量初始化等
* 可以使用cin来将键盘输入存储到string对象中
* 可以使用cout来显示string对象
* 可以使用数组表示法（下标）来访问存储在string对象中的字符

##### 1. string类一些特有操作：
* string间可以进行赋值、拼接、附加。
* string类具有一些方法，如`string::size()`。
* std::getline(std::istream& is, std::string& str, char delim = '\n); 读取流数据至目标string中。
示例：
```C++
using namespace std;
...
string str1 = "abcd";
string str2;
str2 = str1;                 // 赋值操作，str2 = "abcd"
string str3 = str1 + str2;   // 拼接操作，str3 = "abcdabcd"
str2 += str1;                // 附加操作，str2 = "abcdabcd"
int len = str1.size()        // 类防范， len = 4
getline(cin, str1);          // 从cin按行读入数据到str1中
```

> [!NOTE] std::getline 相较 std::cin.getline 的优点：
> std::cin.getline 依赖字符数组作为输入缓冲，std::getline 依赖string作为输入缓冲，string无溢出风险，所以std::getline更为安全


#### 3.2.3 字符串字面值

##### 1. 宽字符串
之前提到过几类宽字符类型：wchar_t、char16_t、char32_t
字符串中，用前缀来表示类型，例如：
```C++
wchar_t s1[] = L"abcd";     // w_char string (编码方式看平台)
char16_t s2[] = u"abcd";    // char_16 string (UTF-16)
char32_t s3[] = U"abcd";    // char_32 string (UTF-32)
char s4[] = u8"abcd";       // char string (UTF-8)
```

##### 2. 原始字符串
C++11的新特性，原始字符串中，字符表示自己本身，例：`\n` 不在代标换行符的转义字符，而是表示两个字符`\`和`n`。
使用前缀R，使用`"(`和`)"`作为默认定界符，可以在`"`和`(`间加入任意数量基本字符（除 空格、括号、斜杠和控制字符），替代默认定界符，成为自定义定界符，这样可以保证`"` 和 `(` 的正常显示。
如：
```C++
std::string str1 = R"(\n can display, )";
std::string str2 = R"+a("(\n)" can display too
)+a";
std::cout << str1 << str2;
```
最终输出结果为：
```bash
xxx@xxx:~$ ./test
\n can display, "(\n)" can display too
xxx@xxx:~$ 
```
其中回车符可以直接用回车输入，而不需要转义字符了。

原始字符串 前缀R可以与其他字符串前缀结合使用，例如：Ru、uR都可以表示char16_t类型的原始字符串。

### 3.3 结构(体)

用户自定义类型，用于合并存储多个不同数据类型的数据
#### 3.3.1 声明结构类型&声明结构变量

```C++
struct sTest{
	std::string name;// object can be a member of struct
	float f;
	double d;
};
struct sTest s1;     // ok
sTest s2;            // ok，struct is unnecessary
```
注意：
* 声明结构类型时，结构中每一个列表项都是声明语句。
* 声明结构类型时，结构体中的每一项及结构自身，都需要以引号结尾。
* 声明结构类型时，可以在函数内部，也可以在函数外部，与其他变量声明的作用域类似。
* C++中，对象也可以作为结构体成员
* （C++特有）声明结构变量时，可以省略struct。

#### 3.3.2 结构初始化
```C++
struct sTest{
	std::string str;            
	int a;
	double b;
};

sTest s1 = {"string", 1, 1.5};   //
sTest s2 {"string", 1, 1.5};     // C++11 allowed
sTest s3{};                      // initial with 0

```
C++结构基本的初始化方式 与 C相同；
C++11后，产生了格式类似的列表初始化方法，其中等号`=`可以省略。
列表初始化的特性在之前数组章节 [[C++#2. 以{}方式初始化时的转换（C++11）]]已经提到过，初始化数值不允许**缩窄**。

#### 3.3.3 其他属性
下述属性源于 C 中结构属性，相互兼容
* 两个相同结构变量可以直接通过等号`=`赋值
```C++
sTest s1 = {"string", 1, 1.5};
sTest s2 = s1;
```
* 声明结构类型、声明结构变量可以同时完成
```C++
struct sTest{
	std::string str;            
	int a;
	double b;
}s1 = 
{
	"string",
	1,
	1.5
};
```
* 可以在不声明结构类型名的情况下，直接声明结构变量。这使得此后都无法创建该类型变量
```C++
struct {
	int a;
	int b;
}s1;
```

#### 3.3.4 结构数组
元素为结构的数组，初始化需要注意，可以结合数组初始化和结构初始化
```C++
sTest sArray[2] ={
	{"string", 1, 1.5},
	{"string", 1, 1.0}
}
```

#### 3.3.5 位字段
声明结构类型时，可以用在字段后加冒号和数字，来限定该字段占用的位数，构成位字段。
这种方式可以节省内存，特别是在存储多个布尔标志位或小范围整数的情况下，在嵌入式编程等场景下会比较常用
例如：
```C++
struct s_register
{
	int S:4;
	int :4;       // 没有名称的字段作为填充，提供间距
	bool t1:1;
	bool t2:1;
}

s_register s={24, 1, 2};
cout << s.S << endl << s.t1 << endl << s.t2 << endl;
```
`s_register`存在匿名位字段，实际只有三个可访问字段，按上述初始化得到的结果如下：
```bash
xxx@xxx:~$ ./test
-8,            # 24 = b11000，截断后，1000被视作 -8
1,             # 正常
1,             # 编译会报 Woverflow
xxx@xxx:~$ 
```
使用位字段时，需要注意：
* 位字段可以和普通字段同时使用。
* 位字段通常使用int、bool、enum等整数类型，具体依赖编译器实现；不可使用浮点类型，编译器都会报错。
* 位字段同样存在着内存对齐，由编译器决定。
* 位数不可超过数据类型的最大位数，如int 位字段最大位数为32。
* 不可取位字段的地址，因为位字段可能位于一个字节或一个整型中的一部分。

### 3.4 共用体

可以存储不同数据类型，但同时只能存储其中一种。
共用体格式与结构体类似，只是含义不同；共用体的存储格式会由访问时的上下文决定。
```C++
union uTest{
	int int_val;
	float float_val;
};

uTest u;
u.int_val = 1;             // 写入int
std::cout << u.float_val;  // 读取float，此时int被读作float，输出为 1.4013e-45
u.float_val = 1.5;         // 写入float，此时int被覆盖
std::cout << u.int_val;    // 读取int，此时float被读作int，输出为 1069547520
```

与结构体类似，同样存在匿名共用体，用于表示地址相同的不同变量。
较常见的是用在结构体或类中：
```C++
struct sTest{
	union{
		int int_val;
		float float_val;
	}
};

sTest s;
s.int_val = 1;           //
s.float_val = 1.5;       //可以分别按int和float形式访问同一段地址
```

共用体的主要优点，是可以节省内存，其占用内存的大小，由其中长度最大的字段决定。

### 3.5 枚举

枚举类型，用于创建一组符号常量，相较于const，它还允许定义新的类型。
例：
```C++
enum eTest{
	num1,     // 0
	num2,     // 1
	num3      // 2
};
```
默认情况下，枚举中的枚举量会被从0依次赋值为递增的整数。

#### 3.5.1 枚举特性：

1. 不经过强制类型转换的情况下，不允许将其他类型变量赋给枚举变量
```C++
eTest eNum;
eNum = num1;      // 合法
eNum = 1;         // 非法
eNum = eTest(1)   // 合法，但存在风险
eNum = eTest(100) // 未定义行为，100超出了枚举量范围（我的环境下，eNum为100）
```

2. 枚举类型之间只定义了赋值运算符，没有定义算数运算符；在表达式中，枚举类型允许提升为int，但int不会自动转换为枚举类型
```C++
eTest eNum1, eNum2;
eNum1 = eNum2;                // 合法
++eNum1;                      // 非法，枚举未定义++运算
int intVal = eNum1;           // 合法，eNum1被提升为整型
intVal = eNum1 + eNum2;       // 合法，eNum1和额Num2被提升为整型
eTest eNum3 = eNum1 + eNum2;  // 非法，整型无法赋值枚举型
```

3. 省略枚举名称，以声明匿名枚举类型

> [!NOTE] 上述部分特性，在某些编译器下是合法的，但是为了兼容性，使用时尽量全部遵守
> 

#### 3.5.2 枚举量的取值及取值范围

之前提到的，默认情况下，枚举中的枚举量会被从0依次赋值为递增的整数。
这里涉及到两个问题：枚举型的取值 和 取值范围

* 取值
除了默认取值，还可以显示地定义枚举量的值：
```C++
enum eTest1{num1=1, num2=4, num3=8};        // {1,4,8}
enum eTest2{zero, null=0, one, num_one=1};  // {0,0,1,1}
```

其中，还可以显示定义部分枚举量，而未定义枚举量的取值为前一个枚举量的值加 1。

* 取值范围
枚举类型的取值范围，由编译器决定；同时，取值范围也是根据枚举类的取值动态变化的
普遍情况下，枚举类型默认取值为int，当枚举量取值超过int时，取值为long，依次类推。

C++11后，允许在定义枚举类型时，指定底层类型，方式如下
```C++
    enum eTest1{
        n1 = 1,
    };
    cout << sizeof(eTest1) << endl;       // 结果为4
    enum eTest2 : long{                   // C++11 特性，限定底层类型
        m1 = 1,
    };
    cout << sizeof(eTest2) << endl;       // 结果为8
```


> [!NOTE] 枚举量名称冲突
> 按上述方法定义枚举型，存在枚举量冲突的情况，此时编译器会报错
> C++11后提供了 作用域内枚举，可以解决这个问题，具体见后文

### 3.6 指针

常规变量中，值是指定量，地址是派生量
使用指针时，地址时指定量，值是派生量。

指针和值之间可以用两个运算符联系：
* 地址运算符（&）：获得变量的地址
* 间接值或解引用运算符（\*）：获得地址处存储的值

> [!NOTE] 乘法运算符也是 \*，需要通过上下文来决定符号含义
> 

#### 3.6.1 指针声明和初始化

指针声明时需要指定指向值的类型，通过类型名+`*`即可定义指定类型的指针。
```C++
int* p1;        // 正确，强调强调int*为指向int的指针类型
int *p2;        // 正确，强调*p是一个int类型的值
int*p3;         // 正确
int *p4, p5;    // 正确，但p4为 int*，p5为 int
```
需注意声明的每个指针变量名前，都需要一个 `*`。
```C++
int a=5;
int *p = &5;
```
指针初始化时，初始的也是地址，而不是其指向的值。

> [!NOTE] 注意：创建指针，只会分配存储地址的内存，但不会分配用来存储指针所指向数据的内存。未初始化时，指针会指向任意可能的内存，使用它会产生未定义的行为。
> 


> [!NOTE] C++11：nullptr
> C++98中，空指针的字面值为0，也可以使用宏 NULL来表示
> C++11中，专门提供了nullptr来表示空指针，减少歧义


#### 3.6.2 new和delete

普通变量的内存是在 **编译时** 分配的，指针变量可以在 **运行时** 分配内存空间。
C中，提供了库函数malloc()等，来分配内存。
C++中，除此之外还提供了 new运算符，用于在堆（heap）/自由存储区（free store）上分配内存；
与new对应的，是delete运算符，用于释放已分配的内存。
```C++
int *p = new int;
...
delete p;
```
除了创建简单变量的内存，new还可以创建数组等复合变量的内存
使用new可以在运行时创建数组，称为 **动态数组**
```C++
int *ps = new int [10];     // 使用[]限定数组长度
...
delete [] ps;               // 使用[]释放整个数组
```
动态数组，与普通的静态数组，在使用上类似，都可以使用下标进行访问、赋值等。
不过二者还是有一些区别，例如：
* 动态数组无法通过sizeof运算符来获取数组占用字节数
* 动态数组的值可以被修改（本质上是指针），但是静态数组不可以
* 动态数组可在运行时分配长度，而静态数组在编译时分配长度
```C++
int p1[10];
p1[5] = 10;
cout << p1[5] << "  " << sizeof(p1) << endl;   // 输出 10 40
// p1 = p1 + 1;                                // 非法
int *p2 = new int [10];
p2[5] = 10;
cout << p2[5] << "  " << sizeof(p2) << endl;   // 输出 10 8
p2 = p2 + 1;                                   // 合法
```

##### new&delete注意事项：
* new与delete应配对使用，new \[\]与delete\[\]应配对使用。
* 不要申请内存但未释放（有new无delete），会产生内存泄漏（memory leak）
* 不要释放未分配的内存（无new有delete）或者多次delete同一内存，会产生未定义的行为
* delete空指针是安全的

#### 3.6.3 指针&数组

数组与指针存在一定联系，在C++中，下面几组表示形式等价：
```C++
int array[10];
int *p = array;   //可以将数组名赋给对应类型指针
```
* `array == &array[0]`
	数组名表示第一个元素的地址
* `array[1] == *(array + 1) == *(p + 1) == p[1]`
	都表示数组第一个元素，下标法等价于指针加法
* 数组和指针仍有区别，同3.6中动态数组和静态数组的区别


> [!NOTE] 数组指针&指针数组
> array解释为array\[0\]的地址，而&array则解释为数组array的地址(数组指针)
> &array可以看做 `int(*)[10]`类型，&array+1跨度为sizeof(array)，即40B
> 声明时，可以写作：
> 	int (\*pp)\[10\] = &array;
> 这里括号必要，这里int（类型标识）和 \[\]优先级都比 \* 高一级，括号保证了 \* 优先与 pp 结合，pp被首先解释为指针，否则：
> 	int \*pp\[10\];
> int（类型标识）优先级更高，会优先结合 \*，结合后，int \*会被整体解析为指针类型标识，此时，pp被解读为 int\*类型的数组，长度为10 （指针数组）。
>  

#### 3.6.4 二维数组
仅以二维数组代表多维数组，说明一些特性
```cpp
int array[5][5];
int (*p)[5] = array[0];    // 这里数组指针p 指向长度为5的int数组
p++;                       // 这里指针跨度为 5*sizeof(int) 字节
```

#### 3.6.5 指针与字符串
在cout和多数C++表达式中，char数组名、char指针、引号括起的字符串常量，都被解释为字符串第一个字符的地址（持续读到 ‘\\0’ 为止）

```C++
char a[20] = "test1";        // 使用字面值 初始化字符串
const char *b = "test2";     // 将字面值地址 初始化给指针
char *c = "test3";           // 不安全 ！！！
```

注：
	代码中 引号括起的字符串，编译器会用特定的空间存储，并与其地址关联。
	C++中，这些字符串被视作常量，所以需要通过常量指针存储其地址。

#### 3.6.6 变量存储方式

C++管理数据内存的方式有三种：自动存储、静态存储、动态存储
* 自动存储
在函数内部定义的常规变量使用自动存储空间，被称为自动变量
自动变量 即是局部变量，占用栈空间，在进入代码块中申请内存，并在退出代码快后释放
```C++
{
	int a = 1;
}
a = 2;        // 错误！！！此处退出代码块，a 未定义
```

* 静态存储
静态存储 保证整个程序执行期间都存在，定义静态存储的变量有两种方式：
	* 函数外部定义
	* static 关键字修饰

* 动态存储
动态存储 生命周期由程序员控制，占用堆空间，通过 new/delete，malloc/free 等特定运算符或函数，可以为变量申请动态存储的空间

各存储方式的主要区别，就是存储位置和生命周期不同，动态存储最为灵活，但要保证及时释放，避免内存泄漏

#### 3.6.7 vector & array


## 4 控制语句

### 4.1 循环语句
#### 4.1.1 表达式和语句

##### 表达式
任何值 或 任何有效的值和运算符的组合 都是表达式，每个表达式都有值。
例：
```C++
22+27            // 值为49
x=20             // 值为20，赋值表达式的值为左侧成员的值
x=y=z=0          // 值为0，同时将x,y,z均赋为0
//
int toad;        // 无值，非表达式
```
表达式后添加分号，即构成了表达式语句
##### 复合语句（语句块）
通过一对花括号可以构造一条复合语句（语句块）。

外部语句块声明的变量内部语句块可见，但内部语句块声明的变量外部不可见
同时声明时，内部语句块会隐藏外部变量
```C++
int x = 20;
{
	std::cout << x << endl;   // x = 20
	int x = 100;
	std::cout << x << endl;   // x = 100
}
std::cout << x << endl;       // x = 20
```

#### 4.1.2 for循环

使用关键字：for，构建的循环语句
大致流程：
```C++
for(initialization; test-expression; update-expression)
	body
```

* for循环为入口条件循环（entry-condition）
* test-expression 结果会被强转为bool型
* C++允许在初始化时声明变量，变量的生命周期只存在于该循环。

##### 循环语句中一些常用的特殊运算符：

***递增运算符（++）& 递减运算符（--）：***
递增运算符（++）和递减运算符（--）常被用在循环中，注意事项：
  1. 后缀 a++的值为a，前缀 ++a的值为(a+1)，但最终对a的影响一样
  2. 同一个表达式中多次使用递增/递减，会产生未定义的结果，需尽量避免
	 如：``` x=2 * x++ * (3 - ++x);```
  3. 前缀运算比后缀运算的效率高，后缀需要创建副本保存原始值 

***组合赋值运算符：***
每个算数运算符都有对应的组合赋值运算符，包括：```+=, -=, *=, /=, %= ```

***逗号运算符：***
将多个表达式合并 并完成同样的任务，例如
```C++
int i,j;                                 // 不作为运算符，用于分隔列表变量
for(i = 0, j = 0; i < 10; i++, j++){}    // 运算符，试用了两次
```

注意，逗号作为运算符同样有作用顺序以及优先级
逗号运算符的优先级最低，同时会先计算左侧表达式，再计算右侧表达式
```C++
x = 17, 240;   // x=17, 240不起作用

x = (17,240);  // x=240，(17,240)结果为逗号右侧表达式的值
```


#### 4.1.3 while循环


## 5. 函数

### 5.1 基础知识：
函数的使用，包括三个步骤：
* 提供函数定义
* 提供函数原型
* 调用函数

```C++
void func();                 // 函数原型

int main()
{
	func();                  // 函数调用
}

void func()
{
	std::cout << "func";     // 函数定义
}
```

#### 函数定义
函数可以分为无返回值函数和有返回值函数，注意：
* 有返回值的函数，必须使用返回语句（C++中，有返回值的函数不return，会产生未定义的行为，而且容易出现古怪的错误，一定要避免）
* 返回值可以是数组以外的任意类型
* 实际返回值类型需与定义的返回值类型一致，或可以被转换为定义的返回值类型
* 函数尽量不出现多个return（代码规范，例如misra C++中就有规定）

#### 函数原型
原型描述了函数到编译器的接口，明确了函数返回值的类型以及参数的类型和数量；再多文件、库文件参与编译的情况下，原型有着重要作用

原型的格式：
```C++
double funcA(int a);       
double funcB(int);        
void funcC(...);
```
注：
* 参数可以不包含变量名，变量名也不必与函数定义的变量名相同。
* C++中函数原型是必须的，而ANSI C中，函数原型是可选的。
* 针对参数列表，C++中，```( )```等价于```(void)```，而 ANSI C中，```()```表示可变参数；C++中的可变参数用```(...)```表示。

```C++
#include <stdio.h>

int main() {
    func(2.6f);
}

int func(int a)
{
    printf("%d\n", a);
}
```
上述代码，使用g++无法编译通过，但使用gcc可以（可能会报warning），不过结果是不确定的，危险的。

#### 函数参数
C++的函数参数传递为按值传递，会将传给函数的值（实参 argument）用创建的副本变量（形参 parameter）接收，并用于函数内部；形参与其他函数内部创建的变量一样，在函数被执行过程中，自动被分配和释放。

但数组作为参数传递时，会有一些注意事项
 * C++中，可以使用类似 ```int a[]```或```int a[5]``` 来表示传递数组a[5]，与```int *a```效果相同；即使使用```int a[5]```也不会传递数组的长度，数组长度需要通过另外的参数传递。
 例如，下面几种函数的原型等效：
 ```cpp
 void fun(int a[5]);
 void fun(int a[]);
 void fun(int *a);
 void fun(int []);
 void fun(int *);
```
* 参数传递按时，只是对数组首地址进行了按值传递，而数组内容是共享的；如想保证数组不被修改，可以增加const修饰，如```const int a[]```


> [!NOTE] 指针常量和常量指针
> * 常量指针（如 const int\* p），此时 \*p 为常量。
> 注：在数据本身非指针的情况下，非const数据的地址，是可以赋值给const 指针的。
> ```cpp
> const int* p;
> int a = 10;
> p = &a;                 // 合法
> int *p2 = &a;
> p = p2;                 // 不合法，非const指针无法赋值给const指针
> 
> const int** pp;
> pp = &p2;               // 不合法，p2本身是指针
> ```
> * 指针常量（如 int * const p），此时 p 为常量。

* 参数传递指针/数组，且仅做输入时，尽可能使用const，因为
	* 避免修改函数外部的数据
	* 允许接收非const和const的数据
* 二维数组做参数时，应声明成指向一维数组的指针，如：
```cpp
int func(int (*ar2)[4], int size);
int func(int ar2[][4], int size);
int func(int ar2[4][4]);
```
这里的ar2仅可限定列数为4，行数无法限定

### 5.2 函数探幽

5.2.3之后介绍的特性，为C++特有，有别于C
#### 5.2.1 函数递归
C++允许函数（main函数除外）调用自己，这被称为递归调用。
如：
```cpp
// 单次调用递归
void func(){
	if(test)
		func();
}

// 多次调用递归
void func2(int i)
{
	if(test){
		func2(i+1);
		func2(i+2);
	}
}
```
注：不合理的递归会影响程序效率，甚至带来死循环，慎用（MISRA中甚至不允许使用递归）

#### 5.2.2 函数指针
函数指针的声明和使用：
```cpp
//声明函数指针
double (*pf)(int);

//获取函数地址
double func(int);
pf = func;               // "func"即代表了func存储位置

//函数指针使用
pf(2);
(*pf)(2);                // 二者皆可
```

注：
* 注意括号的使用，\* 优先级较低，如果是 ```double *pf(int)```，则代表返回值为double指针
* C++中，pf、(\*pf)，都可以用作函数调用

****函数指针数组***
声明示例：
```cpp
double *(* pa[3])(int x);
auto pb = pa;                // auto 简化了重复声明时的语句
```
注：需要注意运算符的排布，'[ ]'的优先级要大于'\*' ，确保了pa首先为三个元素的数组

函数数组使用时与访问其他类型数组类似：
```cpp
double x = *pa[1](100);
double y = *(*pb[1])(100);
```

指向函数指针数组的指针(同样注意运算符的排布)：
```cpp
double *(*(*ppa)[3])(int x);
ppa = &pa;
auto ppb = ppa;
```
ppa和pa 从数值角度相同，值都是数组第一个元素的地址（&pa\[0\]），但实际含义不同：
ppa 可以理解为指针数组的指针，或简单理解为数组指针，指向整个数组，所以步长为3\*8 bytes；
pa可以理解为指针数组，指向第一个元素，所以步长为 8 bytes

***typedef简化***
与其他类型类似，我们也可以使用typedef 来为函数指针申请别名：
```cpp
typedef double *(* func)(int x);
func p1;

func pa[3] = {p1, p1, p1};       // 函数指针数组
func (*paa)[3] = &pa;            // 函数指针数组的指针
```


#### 5.2.3 内联函数
C++ 通过关键字 ```inline``` 声明和定义内联函数，内联函数在编译时，调用函数的代码段会直接使用函数副本，而不是函数调用；即以空间换时间，用更多的内存占用节省了函数调用时上下文切换的时间。

使用场景：
* 函数代码量较少
* 函数调用次数很多
注：
* ```inline```只是请求编译器将函数作为内联函数，但不一定生效，例如编译器认为函数过大，或者函数中存在递归（内联函数是禁止递归的）；目前的编译器优化选项各不相同，甚至包含了内联功能，所以实际编译效果也各不相同。

***内联函数 和 宏定义***
C++内联函数 与 C 的宏定义（预处理语句 \#define） 有些类似，都是编译优化的一种方式；不过二者还是有本质上的区别
* 内联函数可以进行值传递，模拟实际函数调用效果
* 宏定义仅进行文本替换。
例：
```cpp
inline int square(int a){return a*a};
#define SQUARE(A) A*A;

int b = 2, B = 2;
int c = square(b++);         // 结果为 2*2=4，b=3
int C = SQUARE(B++);         // 结果等价于 B++*B++，危险，产生未定义行为
```
上述案例中，```SQUARE()``` 仅进行文本替换，导致替换后的表达式中存在多次修改同一个变量，属于未定义行为；而```square()```将b++后的结果作为值传递给函数，结果更符合预期。


#### 5.2.4 引用变量

引用变量是C++特有的复合类型，使用 & 作为类型标识符的一部分，来声明引用变量。
```cpp
int a = 1;
int &b = a;
```
引用变量的一些特性：
* 引用类似变量的别名，与原变量拥有同样的值和地址
* 引用必须在声明时初始化，且引用关系定义后无法修改

> [!NOTE] 引用和指针常量
> 引用和指针常量虽然类型不同，但效果有些类似
> ```cpp
> int a = 1;
> int ta = a;                       
> int * const pa = &a;        // ta、pa将永远指向 变量a，不可更改
> ```

##### ***引用传递***
引用常被用作函数参数，又称引用传递，相较于普通的值传递，引用传递允许被调函数访问调用函数的变量；特别是在参数变量较大时，引用传递可以起到节省空间的作用。

下述代码通过引用传递可以交换外部变量：
```cpp
void swap(int &a, int &b)
{
	int temp = a;
	a = b;
	b = temp;
}
```

##### ***const+引用***
编写引用传递时，如果想避免调用者修改实参，可以外加 const 修饰
```cpp
void func(const int &a);
```
如果函数定义中存在修改实参的情况，则编译报错。

此外，常量引用传递和引用传递还有一处差异：
* 引用传递始终使用实参本身，如果实参类型与参数类型不匹配 或 非左值时，编译无法通过
* 常数引用传递通常使用实参本身，但部分场景下，允许生成临时变量（形参），满足编译。
注：引用传递允许生成临时变量，当且仅当传递常数引用时


> [!NOTE] 左值和非左值
> 左值参数是可被引用的数据对象， 例如， 变量、 数组元素、 结构成员、 引用和解除引用的指针都是左值。 非左值包括字面常量（用引号括起的字符串除外， 它们由其地址表示） 和包含多项的表达式。 在C语言中， 左值最初指的是可出现在赋值语句左边的实体， 但这是引入关键字const之前的情况。 现在， 常规变量和const变量都可视为左值， 因为可通过地址访问它们。 但常规变量属于可修改的左值， 而const变量属于不可修改的左值。


生成临时变量的场景（前置条件为const）：
1. 实参类型正确，但是不是左值
2. 实参类型不正确，但是可以转换为正确的类型
例：
```cpp
void func1(const int& x);
void func2(int &x);

...

	int a = 1;
	double b = 1.5;
	func1(a);              // 合法
	func1(b);              // 合法，会产生形参；b是类型不符，不过可以转换
	func1(a+1);            // 合法，会产生形参；a+1类型正确，不过不是左值
	func2(b);              // 不合法，int引用无法作用于double
```
不过，即使生成了临时变量，const依旧生效，即无法修改参数本身。

这里的引用皆是左值引用，后文还将提到C++11的新特性，右值引用（&&）

##### ***引用返回***
引用类型同样可以作为函数的返回值，与引用参数一样，可以节省输入输出时的内存拷贝。

但需要注意的是，不要将函数中的局部变量作为返回值返回，这回产生未定义的行为：
```cpp
int &func1(int &a){
	return a;                      // 合法
}

int &func2(int &a){
	int b = 1;
	return b;                       // 危险，因为函数返回后，局部变量已失效
}

const int &func3(int &a){
	return a;                       // 合法，const返回可以接收非const输出
}

int &func4(const int &a){
	return a;                       // 非法，普通返回无法接受const输出
}
```

另外，和引用参数一样，引用返回也建议加上```const```修饰：
* 避免含义混淆，const引用返回值一定为不可修改的左值（通常函数返回是 不可修改的右值，普通引用返回确是 可修改的左值），const保证了返回值不可修改。
* 可以接纳 const和非 const类型

##### ***指针传递&引用传递***
很多时候，指针和引用能够起到相同的作用，结合书中的内容，整理了以下的使用建议：
* 传递内置类型变量，直接值传递，若需修改，则用指针
* 传递数组类型，只能用指针
* 传递结构体，指针和引用皆可
* 传递对象，使用引用（同为C++特性，一起使用更为简约）
* 上述所有场景，若传递的实参不在函数中修改，一律添加const修饰

#### 5.2.5 默认参数：
C++允许定义默认参数，在未传入实参时，函数使用默认值作为输入：
```cpp
int func(int a, int b=1)
int func(int a, int b)
{
	return a + b;
}

std::cout << func(2,2);   // 输出 4（2+2）
std::cout << func(2);     // 输出 3（2+1）            
```

默认参数简化了代码编写，减少了部分重载的编写

使用默认参数时，需注意：
* 默认参数必须从右向左设置，即省略的参数一定分布在参数列表右边
* 实参输入必须从左向右，可以省略但不可以跳过默认参数（如：```func(1, ,3)```是不合法的）
* 默认参数在原型和定义中，仅允许出现一次

#### 5.2.6 函数重载
C++允许函数重载（也叫函数多态），即在函数参数列表（也叫函数特征标）不同的情况下，可以定义一系列同名函数来实现不同功能；
只有个参数类型、数目、排布都相同，才可称为特征标相同。

注意事项：
* 重载需要特征标不同，而不在意返回值；仅返回值不同无法重载，但重载函数的返回值可以不同
* 重载函数特征标各不相同，但在使用时亦可能产生冲突，例如：
```cpp
// 1. int、const int
void func(int a);
void func(const int a);    // 定义时即报错，特征标不区分同一类型的变量和常量

// 2. int、int&
void func(int a);
void func(int &a);        // 定义不报错，因为int 和 int& 是两个类型

int b = 1;
func(b);                  // 报错，int、int&皆可传递变量b
func(1);                  // 正常，常量为右值，int&无法传递

// 3. char*、const char*
void func(char *p);
void func(const char *p);

const char p1[10] = "xxx";
char p2[10] = "yyy";
func(p1);                 // 选择 func(const char* p)，char* 无法接收 const char*
func(p2);                 // 选择 func(char *p)，理论二者都可以接收，但是const char*会
                          // 接收会涉及类型转换，所以优先选 char*

```
上述情况，主要是因为函数特征标语义存在重叠，例 char* 和 const char* 都可以接收 char* 类型，所以使用时，尽量规避这类容易产生歧义的场景。

#### 5.2.7 函数模板

函数模板属于一种泛型编程，将类型作为参数传递个模板。
函数模板并不创建函数，而是告诉编译器如何定义函数。

例，一个用于交换两个AnyType类型变量的函数模板
```cpp
template <typename AnyType>
void swap(AnyType &a, AnyType &b)
{
	AnyType temp = a;
	a = b;
	b = temp;
}

...

int i=1, j=2;
swap(i,j);                   // typename为int，编译器会定义int版本的swap模板函数
```

模板同样可以重载，重载的模板需保证函数特征标不同，例：
```cpp
template <typename AnyType>
void swap(AnyType *a, AnyType *b, int){...}
```
注：函数模板可能无法匹配一些类型（见《CPP》 616）
##### 显式具体化：
针对同一个函数名，可以有多个函数版本：
* 非模板函数（普通函数）
* 模板函数
* 显式具体化模板函数（原型和定义以template<>开头）
* 上述函数的重载版本
匹配优先级：非模板函数 > 具体化模板函数 > 常规模板函数

例：
```cpp
void Swap(int&, int&);             // 非模板函数原型

template <typename T>
void Swap(T&, T&){...};                 // 模板函数原型

template<> void Swap<S>(S&, S&){...};   // 上述模板函数的具体化（<S>可省略）
```
##### 实例化：
在[[C++#5.2.7 函数模板]]初介绍到：函数模板不会定义函数，而是一个生成函数定义的方案；
而通过函数模板为特定类型生成函数定义，得到模板实例的过程，就是一个 ***实例化*** 的过程。

实例化分为隐式实例化 和 显式实例化：
* 隐式实例化：编译器识别到后自行实例化，前文提到的都是隐式实例
* 显式实例化：使用关键字 template，编译器会根据指定的类型生成特定实例，例
```cpp
template void Swap<S>(S&, S&); 
```

**隐式实例化、显式实例化、显式具体化 统称为具体化**
更新之前提到的匹配优先级：非模板函数 > 具体化模板函数 > 显式实例化的模板函数 > 隐式实例化的模板函数


> [!NOTE] 函数模板注意事项：
> * 模板声明和定义需要都放在调用者看得到的地方，比如一起放在头文件中；
> 	这是由于实例化在编译时进行，编译器访问到模板函数调用处时，会进行实例化，需要用到模板函数定义，否则实例化失败
> * 显示实例化仅声明即可，而具体化需要额外添加定义


##### 重载解析：
当同一个函数名存在多个备选函数时，如何选择最优函数。
规则非常复杂，具体阅读 《CPP》8.5.5

##### C++11的模板新特性：具体阅读 《CPP》8.5.6
* 关键字 ```decltype``` ：用于模板中，声明类型不明的变量
* 后置返回值：用于明确返回值类型



## 6. 内存模型和名称空间（本章略读，具体看《CPP》）

### 6.1 数据的几种特性：
#### 6.1.1 存储持续性
存储持续性描述数据保留在内存中的时间
C++中已有的几种：
* 自动存储持续性：普通且常见，存储在栈，生命周期宇代码块一致
* 静态存储持续性：函数定义外定义或使用关键字static定义，存储在静态区，生命周期与程序一致
* 动态存储持续性：使用new&delete管理生命周期，存储在堆
* 线程存储持续性：C++11 新特性，关键字 thread_local 描述，生命周期与线程一致

#### 6.1.2 作用域
#### 6.1.3 链接性


> [!NOTE] register
> C++11前以及C：表示建议编译器使用CPU寄存器来存储自动变量
> C++11：显式指出变量是自动的，与古早的auto功能相同

静态持续变量根据链接性不同分为三类：
* 外部链接性（全局可访问），在代码块外部声明
* 内部连接性（当前文件可访问），在代码块外声明，并使用static
* 无链接性（代码块内可访问），在代码块内声明，并使用static
所有静态变量默认值为0

cv-限定符：
* const
* volatile：告诉编译器，变量可能被代码外的因素修改，避免编译优化（仅读缓存，不读内存）
mutable：用来修饰const结构（或类）中，可以被修改的成员

注：C++中，全局const定义也被看做是内部链接性（允许了头文件中定义）

#### 6.1.4 new
* 常规new运算符
* 定位new运算符：制定分配的内存位置，且不与delete搭配使用
```cpp
int buff[50];
int *p1 = new(buff) int[10];
```



## 7. 类与对象

C++是面对对象编程（OOP）设计的，类 就C++是基于OOP的主要改进
OOP主要特性：
* 抽象
* 封装和数据隐藏
* 多态
* 集成
* 代码的可重用性

### 7.1 基础
#### 7.1.1 访问控制

C++提供关键字，来描述对类成员的访问控制：
* private，私有成员，类的成员默认都是private，可以通过公有成员函数访问私有成员
* public，公有成员，意味着外部可以直接访问
* protected，与类的继承有关

> [!NOTE] C++中的结构和类
> C++中，除了定义了类，对结构的定义也有了拓展；C++的结构几乎拥有和类一样的特性（可以定义成员函数，定义访问限制等等），只在默认访问限制上有区别：
> ***类成员 默认为private，结构成员 默认为protected***

#### 7.1.2 成员函数

成员函数可以在声明时定义（此时默认为内联函数）；也可以在外部定义。外部定义成员函数时，使用作用域解析运算符 “```::```”
```cpp
class A{
	int a;
public:
	void func1(){a++;}       // 内部直接定义，默认为内联
	void func2();
};

void A::func2(){a--};        // 外部定义
```

注：类内部直接定义的函数默认为内联函数，通常用于定义一些短小的、调用频率高的成员函数
定义内联成函数方法：
* 类声明时定义
* 和类定义在一个头文件中，使用 inline 定义在外部（很多编译器也允许定义在其他文件里）
##### const成员函数
针对 const 对象，只能调用const成员函数
```cpp
class B{
	int b = 25;
public:
	void show() const;                          // 声明cosnt成员函数
};

void B::show() const{std::cout << b << std::endl;}    // 定义时，也必须加const

int main()
{
	const B classB;
	classB.show();
}
```
良好的代码习惯：针对不会修改成员变量的成员函数，尽可能定义成const成员函数

#### 7.1.3 构造&析构

##### 特点：
* 与类同名（析构会加上~）
* 无返回值
* 需要有默认构造&析构函数，未自行定义时，编译器会为类生成默认的构造&析构函数
	针对构造函数：如果显示定义了构造函数，编译器则不会提供默认构造函数，默认构造函数有两种情况：
		1. 无参数，如：```A::A(){...}```
		2. 均设置了默认参数，如：```A::A(int a=1){...}```
		确保默认构造函数有，且不冲突。
	针对析构函数：由于析构函数无参数，则 定义了，即为默认析构函数
* 通常不会显示访问对象的构造&析构函数
	构造函数无法通过对象访问，因为构造必然在生成对象之前
	析构函数也不建议通过对象显式访问，可能导致资源重复释放

构造函数通常是在对象初始化是调用：
* 显式调用，如：```A a = A(1,2);```
	（这种情况可能生成临时对象，也可能直接初始化，取决于编译器）
* 隐式调用，如：```A a(1,2);```
* 配合初始化相关的运算符使用，如：```A* a = new A(1,2);```

完整示例：
```cpp
class A{
    int a;
    int b;
public:
    A(int x, int y);  // 自定义构造，则编译器不在提供默认构造函数
    ~A();          // 若自定义析构，类中需要显式声明析构函数
};

A::A(int x, int y){
    a=x;
    b=y;
}

A::~A()
{
    std::cout << a << " " << b << std::endl;
}

int main()
{
    A a(1,2);        // 隐式调用，如若使用 A a(); 错误，此时无默认构造函数
}                    // main()返回，对象a生命周期结束，自动调用析构，打印输出
```


> [!NOTE] 隐式调用默认构造函数
> 隐式调用时，不要加括号，会被误认为函数声明：
> ```cpp
> 	classA a( );   // 编译器认为是，声明一个返回值为 classA类型的函数 a()
> 	classA a;      // 会隐式调用默认构造函数
> 	classA a{};    // 列表初始化，列表为空，等效于默认构造函数
> ```

注：列表初始化为C++11特性，列表需与某个构造函数的参数列表匹配

#### 7.1.4 this 指针
this 是一个指向当前对象的指针。它是每个非静态成员函数的隐含参数，在函数内部可以使用 this 指针来访问对象的成员变量和成员函数；在 const 成员函数中，this 指针的类型是指向常量对象的常量指针。

示例：
```cpp
class A{
public:
    int a;
    A(int a){this->a = a;}
	const A& topA(const A& classA);
};

const A& A::topA(const A& classA){
	if(classA.a > a)
		return classA;
	else
		return *this;                  // 返回的即是当前对象
}

int main()
{
	A cA1(1);
    A cA2(2);
    const A cA3 = cA1.topA(cA2);
    std::cout << cA3.a << std::endl;
}

```

#### 7.1.5 对象数组

声明对象数组，与其他基本类型类似，注意：声明对象数组所用的类必须有默认构造函数

声明对象数组并初始化的示例：
```cpp
A arrayA[4] = {
	A();
	A(1,2);
}
```
这里，只初始化了 arrayA\[0\]，arrayA\[1\]，其余的成员使用默认构造函数初始化
编译器初始化对象数组的顺序：
1. 使用默认构造函数创建数组元素
2. 使用花括号中的构造函数创建临时对象
3. 将临时对象复制到对应元素
由此可见，使用对象数组时，默认构造函数是必须的。

#### 7.1.6 类作用域

类中定义的名称作用域为整个类，类外是不可知的，必须通过对象间接方位（使用 ' . '，' :: '，' -> '等运算符）。

##### 类作用域内常量

作用域内的常量，可以在同一个类的不同对象间共享
注：直接声明常量并初始化是不可行的，以为类在声明时必不会申请空间，只有在定义对象时申请空间。
下面是一些实现共享的方法：
```cpp
class A{
	const int a1 = 10;                  
	double array1[a1];            // 不合法，此时a1并未真正初始化
	
	enum{a2 = 10};
	double array2[a2];            // 合法
	
	static const int a3 = 10;
	double array3[a3];            // 合法
};

```
其中：
* a1，无法在类中直接声明并初始化常量，故不能用来声明数组（C++11提供了成员初始化，未报错）
* a2，枚举并为定义成员变量，而是创建了符号常量，不占用对象空间，可以成立
* a3，添加静态属性，使得成员定义在静态区中，不占用对象空间，可以成立（C++98只允许以这种方式初始化整型或枚举，C++11消除了这种限制）
注：静态成员的详细描述：[[C++#7.2.4 静态成员]]

> [!NOTE] 枚举类
>C++11提供了作用域内枚举（枚举类），来解决多个枚举的枚举成员冲突问题：
>```cpp
>//声明
>enum class A{one, two, three};
>enum class B{one, two, three};
>enum class C : short{one, two, three};
>//使用
>int a = int(A::one);
>```
>上述的枚举A、B，虽然成员名称相同但并未冲突。
>此外，C++11还允许指定枚举类类型，如C，指定为short型；通常未指定时，默认为整型
>注：C++11提高了枚举的类型安全，如上述对a赋值，```A::one```无法隐式转换为整型，需要显示转换。

***------------------注：CPP书中对枚举类指定类型的用法有误***

### 7.2 特性延伸

#### 7.2.1 运算符重载

C++允许用户重新定义现有的运算符，并作用于自定义数据类型上。
运算符重载，依赖于运算符函数，格式为``` operatorOP(argument-list)```，其中，OP必须为C++现有的运算法，例如：operator+()，operator%()。

实际使用时，当检编译检测到，运算符两侧（或单侧）的对象 符合 自定义运算符参数，则会按自定义的运算符函数进行运算，例：
##### * 成员函数 重载运算符
```cpp
using namespace std;
class A{
public:
	int x;
    A(int a){x=a;}
    A operator+(A &obj) const{
	    A temp(0);
	    temp.x = this->x + obj.x;
	    return temp;
	}
};

int main()
{
    A obj1(3),obj2(4);

    A obj3 = obj1 + obj2;
    A obj4 = obj1.operator+(obj2);
    cout << obj3.x << " " << obj4.x << endl;
}
```
其中，obj3 、obj4 赋值时用的相加方式等价。
上述，是将运算符函数作为 ***成员函数***：
	运算符函数作为成员函数时，可以访问私有成员，并且在使用时，对象自身会作为其中一个操作数（隐式）；
	这也有个弊端，例如 operator+()，无论参数是什么，使用时 左端操作数 必须为当前类的对象，即必须满足 ```obj.operator+()``` 的调用形式
##### * 非成员函数 重载运算符
```cpp
A operator+(A& obj1, A& obj2)
{
    A temp(0);
    temp.x = obj1.x + obj2.x;
    return temp;
}
...
int main()
{
    A obj1(3),obj2(4);

    A obj3 = obj1 + obj2;
    A obj4 = operator+(obj1, obj2);
    cout << obj3.x << " " << obj4.x << endl;
}          
```
其中，obj3、obj4赋值时用的相加方式等价
运算符函数，亦可作为 ***非成员函数***
	运算符函数作为非成员函数时，不与类绑定，也不可以直接访问操作数对象的私有成员。

注：上述两种重载方法，都可以满足```A obj3 = obj1 + obj2```，且同时定义两种重载并不会冲突，不过会造成行为上的不确定性，所以实际使用时，需避免重载的二义性。


> [!NOTE] 递增&递减运算符重载
> 递增（++）和递减（--）运算符随着操作数位置不同，有前置和后置两种语义。所以在重载时，需要通过特殊的方法来区分两种语义：
> ```cpp
> 
> ```
> 见链接：https://blog.csdn.net/weixin_50464560/article/details/117663324
> 稍后补充

##### 重载限制：

C++下多数运算符都可以重载，不过也有以下限制：
1. 重载后的运算符至少有一个操作数 使用户定义类型（避免了为标准类型重载运算符）
2. 不能违反运算符 原有的句法规则，也不能改变优先级
	例如，无法将双目运算符 '%' 改为单目运算符
3. 不能创建新运算符
	例如，operator@()，非法，因为@原本不是运算符
4. 特例，下述运算符不可重载：
	* sizeof： sizeof运算符。
	* .： 成员运算符。
	* .\*： 成员指针运算符。
	* ::： 作用域解析运算符。
	* ?:： 条件运算符。
	* typeid： 一个RTTI运算符。
	* const_cast： 强制类型转换运算符。
	* dynamic_cast： 强制类型转换运算符。
	* reinterpret_cast： 强制类型转换运算符。
	* static_cast： 强制类型转换运算符。
5. 特例，下述运算符只可作为成员函数重载：
	* =： 赋值运算符。
	* ( )： 函数调用运算符。
	* [ ]： 下标运算符。
	* ->： 通过指针访问类成员的运算符


#### 7.2.2 友元

通常，公有方法是访问私有成员的唯一途径，这不方便某些编程场景；
为了打破这一限制，C++提供了一种新的访问权限：友元
友元有三种形式：
* 友元函数
* 友元类
* 友元成员函数

##### 友元函数：
使 非成员函数访问类的私有成员，通过friend关键字
例：
```cpp
class A{
	int x;
public:
    A(int a){x=a;}
    A operator+(A &obj) const{
	    A temp(0);
	    temp.x = this->x + obj.x;
	    return temp;
	}
	friend A operator+(A& obj1, A& obj2);    // 类中声明友元函数
};

A operator+(A& obj1, A& obj2)
{
    A temp(0);
    temp.x = obj1.x + obj2.x;
    return temp;
}
```
注：friend 用在类声明中，定义时不要加friend

##### 重载 <<：

比较特殊，有技巧性，例：
```cpp
class A{
...
	friend std::ostream & operator<<{std::ostream &os, const A& objA}
...
};
```
* 由于左操作数不能为对象本身，所以只能使用友元
* 返回值为 ```ostream&```，应对 ```cout << x << y```这类的情况

#### 7.2.3 类的自动转换和强制类型转换

##### 其他转换为当前类对象：
C++ 通过定义支持单参数的构造函数，则可以将其他对象转换为当前类的对象：

```cpp
class A{
	int a;
public:
	A(int x){a = x;}
};

A obj a = 3;
```
这种转换可以隐式进行，具体场景：

。。。 。。。

可以通过关键字 explicit 关闭隐式转换，不过仍可以通过构造显式转换，如：
```cpp
class A{
	int a;
public:
	explicit A(int x){a = x;}
};

A obj a = (A)3;
```
要注意转换过程中，二义性的问题

##### 当前类对象转换为其他（转换函数）

需要定义转换函数，形式：``` operator typeName() ```
例：
```cpp
class A{
	int a;
public:
	A(int x){a = x;}
	operator int() const{ return a;}
};

...
A a(3);
int x = a;      // 隐式转换
int y = int(a)  // 显式转换

```
注：转换函数不需要声明返回值，但实现时需要返回所需类型的值
此外，转换函数也存在二义性可能，C++11 允许explicit用于转换函数。

#### 7.2.4 静态成员

关键字 static 修饰类的静态成员，注意：
* 类的所有对象的静态成员公用一个副本
* 静态成员无法在类中初始化（除了const static），所以只能在外部初始化
注：以前类成员都无法再类中初始化，C++11后，提供了默认成员初始化，允许在类中初始化非静态成员
* 静态成员必须在类外部定义（可以不初始化）
* 静态成员和静态变量一样，定义时需保证全局唯一
```cpp
class A{
	static int sA;
}

int A::sA = 10;
```

##### 静态成员函数
几个关键点：
* 只能通过类名调用
* 只能访问静态成员
* 没有this指针
* 与类的生命周期相同

#### 7.2.5 特殊成员函数

C++ 会在某些情况下，为类自动定义一些特殊成员函数：
* 默认构造函数， 如果没有定义构造函数；
* 默认析构函数， 如果没有定义；
* 复制构造函数， 如果没有定义；
* 赋值运算符， 如果没有定义；
* 地址运算符， 如果没有定义；
* C++11：移动构造函数，移动赋值运算符（18章介绍）

这里介绍下之前未介绍的：
##### 赋值构造函数：
复制构造函数用于将一个对象复制到新创建的对象中，只用于初始化
```cpp
Class_name(const Class_name &);
```
在新建一个对象，并将其初始化为同类现有对象时，可能会被隐式调用：
```cpp
ClassA objA2(objA1);                  // 隐式调用
ClassA objA3 = objA1;                 // 隐式调用
ClassA objA4 = ClassA(objA1);         // 显式调用
ClassA* pObjA5 = new ClassA(objA1);   // 显式调用
```

##### 赋值运算符：
C++允许对象赋值，因为C++会自动重载类运算符：
```cpp
Class_name & Class_name::operator=(const Class_name &);
```
注意：
* 赋值运算符通常返回调用对象的引用，以解决连等情况（如：```x=y=z```）
* 类中定义了类成员的情况
	* 类的默认赋值会自动调用类成员的默认赋值；
	* 当显示定义了赋值时，需要显示调用所有类成员的赋值

#### 7.2.6 动态内存和类

在创建对象时，可以通过new、delele来维护对象的动态内存（提供了灵活性，但更危险）
通常，在构造函数中使用new申请，在析构函数中就必须使用delete销毁，一一对应。

注意：
有些特殊成员函数（[[C++#7.2.5 特殊成员函数]]）中，也隐含着成员的new和delete，切勿忽视，
------------------------**详见《CPP》12.1-12.2中关于自定义string类的例子**

> [!NOTE] 浅拷贝和深拷贝
> 使用默认赋值构造时易忽视的点
> 针对 如：char *ptr = new char[10];
>  浅拷贝通常 只拷贝指针，而深拷贝 还会拷贝指针指向的有效内存。




### 7.3 类继承

C++类继承允许在原有类的基础上拓展生成新的类，新的类可以继承原有类的特征：
原始类称为基类，继承类称为派生类。
语法示例：
```cpp
class A{...};

class A_plus : public A{...};          // public 代表公有派生
```
派生类对象特征：
* 派生类对象存储了基类的数据成员（派生类继承了基类的实现）；
* 派生类对象可以使用基类的方法（派生类继承了基类的接口）；
* 派生类需要自己的构造函数；
	派生类构造函数要点：
	* 首先创建基类对象；
	* 派生类构造函数可以通过成员初始化列表将基类信息传递给基类构造函数；
	* 派生类构造函数应初始化派生类新增的数据成员。
	注意：派生类构造函数中，会先调用基类的构造函数，可以通过初始化列表显式调用构造函数，否则程序会先隐式调用默认构造函数；派生类的析构函数中，会先隐式调用基类的析构函数
* 派生类可以根据需要添加额外的数据成员和成员函数。

派生类显示调用基类构造函数方法：初始化列表
```cpp
using namespace std;
class A{
private:
	int a;
	double b;
public:
	A(int a, double b){this->a=a, this->b=b;}
    void print(){cout << a << " " << b << endl;}
};

class AP: public A{
private:
    int c;
public:
    AP(int a, int b, int c):c(c),A(a,b){}
    void print(){A::print();cout << c << endl;}
};

int main(){
    AP objAP(1,2,3);
    objAP.print();
}
```

##### 继承中指针和引用的兼容性：
两个特性：
* 基类指针可以在不进行显示类型转换的情况下指向派生类对象
* 基类引用可以在不进行显式类型转换的情况下引用派生类对象
不过反之不行
这衍射了一系列特殊但合理的用法
```cpp
A objA;
A_plus *pAP = &objA;
A_puls &AP = objA;            // 赋值

A_plus objAP1(objA);          // 默认拷贝构造
objAP1 = objA;                // 默认赋值
```

#### 7.3.1 多态公有继承

基类的方法在派生类中有不同的实现，途径：
* 派生类中重新定义基类的方法
* 使用虚方法

基类必须有虚析构函数


## 9 C++库

### 9.1 string 类

前置：需包含\<string\>头文件
#### 9.1.1 string 构造

string 构造被重载为多个版本
```cpp
// 初始化为s指向的NBTS（空字符结束的字符串）
string(const char * s)

// 创建一个包含n个元素的string对象， 其中每个元素都被初始化为字符c
string(size_type n, char c)

// 将一个string对象初始化为string对象str（拷贝构造函数）
string(const string & str)

// 创建一个默认的sting对象， 长度为0（默认构造函数）
string()

// 将string对象初始化为s指向的NBTS的前n个字符， 即使超过了NBTS结尾
string(const char * s,size_type n)

// 将string对象初始化为区间[begin, end)内的字符， 其中begin和end的行为就像指针， 用于指定位置， 范围包括begin在内， 但不包括end
template<class Iter> string(Iter begin, Iter end)

// 将一个string对象初始化为对象str中从位置pos开始到结尾的字符， 或从位置pos开始的n个字符
string(const string & str,string size_type pos = 0,size_type n = npos)

// 这是C++11新增的， 它将一个string对象初始化为string对象str， 并可能修改str（移动构造函数）
string(string && str)noexcept

// 这是C++11新增的， 它将一个string对象初始化为初始化列表il中的字符（列表初始化）
string(initializer_list<char>il)
```
* ```template<class Iter> string(Iter begin, Iter end)```，这里的begin、end指向内存中的两个位置（通常，可以使迭代器），例如：
```cpp
char alls[20] = "aaaaaaaaaaaaaaaaaaaa";
string six(alls+6, alls+10);
string six2(&alls[6], ^alls[10]);      // 这里begin和end都是 char*类型
```

#### 9.1.2 其他重载
除了构造函数，string还重载了其他方法：
* <<：
	string以字符的形式输出到输出流中
* +=：
	添加字符串至string尾部
* +：
	合并两个操作数，得到一个string
* \[\]：
	可以使用数组表示法访问string中的各个字符
* <、>、== 、!= 等关系运算法：
	string之间、string与C-风格字符串间的比较


#### 9.1.3 其他方法：
* string::npos：字符串可存储的最大字符数
* size()、length()：返回字符串中的字符数
* size_type find(const string & str, size_type pos = 0)const
	从pos开始查找子字符串，并返回首次出现时首字符的索引；若为出现，返回string::npos
	find()还有诸多重载版本

#### 9.1.4 字符串种类

string通常是基于char类型的，事实上，string库实际上是基于一个模板类的：
```cpp
template<class charT, class traits = char_traits<charT>, class Allocator = allocator<charT>> basic_string{...};
```
根据模板衍生出了各个类实例：
```cpp
typedef basic_string<char> string;
typedef basic_string<wchar_t> wstring;
```

### 9.2 智能指针模板类












## 面试准备：

### C++（OOP）三大特性：
* 封装
* 继承
* 多态

### C++多态：
* 静态多态（编译时多态）：函数重载、函数模板
* 动态多态（运行时多态）：虚函数、继承
虚函数（virtual）可以在派生类中重写（override），基类指针或引用调用虚函数，会根据实际对象实际（派生类）类型调用对应函数
注：重写时，添加override关键字代表显式重写

纯虚函数：基类中声明但不实现（声明时添加 =0）
抽象类：包含纯虚函数的类，不能实例化，只能作基类

派生类定义基类同名的非虚函数为隐藏；如果定义基类同名但特征标不同的虚函数，也会隐藏。可以用 using 引入基类被隐藏的函数


静态多态&动态多态衍生的说法：
* 静态联编：编译时确定函数调用
* 动态联编：运行时确定函数调用，典型场景就是 虚函数：通过基类指针或引用调用派生类函数
动态联编通过虚标（vtable）实现，每个包含虚函数的类都有一个虚表，存储虚函数的地址

### C++继承：

主要就是：通过继承基类，定义派生类

继承访问控制：
* public，公有继承，基类的public、protected成员在派生类中属性不变
* protected，保护继承，基类的public、protected成员在派生类中均为protected
* private，私有继承，基类的public、protected成员在派生类中均为private
* 上述所有，继承的private成员仍为private

继承方式：
* 单继承
```cpp
class A{};
class B : public A{};
```
* 多继承
```cpp
class A1{};
class A2{};
class B : public A1, public A2{};
```
* 多层继承
```cpp
class A{};
class B{} : public A{};
class C : public B{};
```

菱形继承，例 B、C分别都继承了A，D继承了B和C
易产生二义性和数据冗余，如：D继承了A中的成员a，但不知道来自B还是C

解决方法：虚继承
```cpp
class A{};
class B{} : virtual public A{};
class C{} : virtual public A{};
class D : public B, public C{};
```

### C++封装：

主要体现在访问控制，通过三种访问修饰符实现：public、private、protected


### C++智能指针：

注：需要头文件\<memory\>

智能指针可以自动管理对象的生命周期，避免内存泄露和悬空指针等问题

* unique_ptr：独占所有权，禁止多个unique_str指向同一对象，不能拷贝，只能移动
* shared_ptr：共享所有权，共享时，引用计数管理对象的声明周期
* weak_ptr：弱引用，解决shared_ptr的循环引用问题

unique_str：
```cpp
#include <iostream>
#include <memory>  // std::unique_ptr

class MyClass {
public:
    MyClass() { std::cout << "MyClass 构造函数" << std::endl; }
    ~MyClass() { std::cout << "MyClass 析构函数" << std::endl; }
    void print() { std::cout << "Hello, MyClass!" << std::endl; }
};

int main() {
    std::unique_ptr<MyClass> ptr(new MyClass());  // 创建 unique_ptr
    ptr->print();  // 使用指针访问成员函数

    // std::unique_ptr<MyClass> ptr2 = ptr;  // 错误：unique_ptr 不能拷贝
    std::unique_ptr<MyClass> ptr2 = std::move(ptr);  // 正确：unique_ptr 可以移动

    if (!ptr) {
        std::cout << "ptr 为空" << std::endl;
    }

    return 0;
}
```

shared_str：
* 循环引用问题，如：A 有成员持有 B 的shared_ptr，B 有成员持有 A 的 shared_ptr，则引用计数始终无法降为0，无法释放，造成内存泄露，
	解决方法：B 改持有 A 的 weak_ptr
```cpp
#include <iostream>
#include <memory>  // std::shared_ptr

class MyClass {
public:
    MyClass() { std::cout << "MyClass 构造函数" << std::endl; }
    ~MyClass() { std::cout << "MyClass 析构函数" << std::endl; }
    void print() { std::cout << "Hello, MyClass!" << std::endl; }
};

int main() {
    std::shared_ptr<MyClass> ptr1(new MyClass());  // 创建 shared_ptr
    std::shared_ptr<MyClass> ptr2 = ptr1;  // 共享所有权

    ptr1->print();
    ptr2->print();

    std::cout << "引用计数: " << ptr1.use_count() << std::endl;  // 输出引用计数

    return 0;
}
```

weak_ptr：
* 不增加引用计数
* 通过lock() 转为 std::shared_ptr 来访问对象
```cpp
#include <iostream>
#include <memory>  // std::shared_ptr, std::weak_ptr

class MyClass {
public:
    std::weak_ptr<MyClass> other;  // 弱引用

    MyClass() { std::cout << "MyClass 构造函数" << std::endl; }
    ~MyClass() { std::cout << "MyClass 析构函数" << std::endl; }
    void print() { std::cout << "Hello, MyClass!" << std::endl; }
};

int main() {
    std::shared_ptr<MyClass> ptr1(new MyClass());
    std::shared_ptr<MyClass> ptr2(new MyClass());

    ptr1->other = ptr2;  // 弱引用
    ptr2->other = ptr1;  // 弱引用

    if (auto shared = ptr1->other.lock()) {  // 通过 weak_ptr 获取 shared_ptr
        shared->print();
    }

    return 0;
}
```

常见创建：
* 直接构造，如：```std::shared_ptr<MyClass> ptr(new MyClass());```
	不一定安全，构造失败时可能造成泄露
* 原始指针构造，如：
```cpp
	MyClass* rawPtr = new MyClass();  // 原始指针
    std::shared_ptr<MyClass> ptr(rawPtr);  // 从原始指针构造 shared_ptr
```
这里须确保 rawPtr不被重复管理，或被手动释放，缺乏安全
* 方法```std::make_unique、std::make_shared```， 
	如```std::shared_ptr<MyClass> ptr = std::make_shared<MyClass>();```


常见销毁：
* 根据生命周期管理，自动销毁（weak除外）
* 手动调用reset()（weak除外）
* weak_ptr 不管理声明周期，构造也是通过 std::shared_ptr的方法


### C++ 类型转换：
* static_cast
* dynamic_cast
* const_cast
* reinterpret_cast


### C++类模板
```cpp
template <typename T>
class 类名 {
    // 类的成员
};
```


### C++ STL


### C++11 新特性
* auto关键字：自动类型推断
* 列表初始化：花括号初始化，包括基本类型、数组、结构、类
* 右值引用：
* 移动语义：
* lambda表达式：

#### 右值引用：
* 左值：可以取地址的表达式，分可修改左值和不可修改左值（const）
* 右值：不可以取地址的表达式，如临时变量、字面值
右值引用使用```&&``` 定义，表示绑定到右值的引用
```cpp
int &&rref = 10;             // 10 为字面值
```
主要作用是支持 移动语义 和 完美转发，主要提高C++程序的性能和灵活性

#### 移动语义：
通过 移动资源 而不是 拷贝资源 来提高程序性能。
C++11 以前，对象的拷贝 时通过 拷贝构造函数 和 拷贝赋值运算符 实现的
C++11，移动语义引入了 移动构造函数 和 移动复制运算符，将资源从一个对象转移到另一个对象

移动构造函数：
```cpp
class MyString{
private:
	char *data;
public:
	MyString(const char* str = ""){
		data = new char[strlen(str) + 1];
        strcpy(data, str);	
	}
	MyString(MyString&& other) noexcept {
        data = other.data;               // 转移资源
        other.data = nullptr;            // 置空原对象的资源
    }

	~MyString() {
        delete[] data;
    }	
};

int main() {
    MyString s1("Hello");
    MyString s2(std::move(s1));  // 调用移动构造函数
}
```
* std::move 将s1 转换为右值，触发移动构造函数
* 被转移时 other.data 置空关键，否则析构时，会出现重复delete

#### 完美转发：
在函数模板中，将参数原封不动的传递给其他函数，保留参数的左值和右值属性
示例：
```cpp
#include <iostream>
#include <utility>  // std::forward

void process(int& x) {
    std::cout << "左值: " << x << std::endl;
}

void process(int&& x) {
    std::cout << "右值: " << x << std::endl;
}

template <typename T>
void wrapper(T&& arg) {
    process(std::forward<T>(arg));  // 完美转发
}

int main() {
    int a = 10;
    wrapper(a);       // 传递左值，调用 process(int&)
    wrapper(20);      // 传递右值，调用 process(int&&)
    return 0;
}

```

在不使用完美转发的情况下，传递a（左值）和20（右值）总是会调用左值版本
使用完美转发的情况下，传递20，会调用右值版本

* std::forward 用于在函数模板中保留参数的左值或右值属性
* 保留右值熟悉，可以触发移动语义，减少拷贝，提高性能


















---
## 边写边记：

1. C/C++中尤其注意：“行为交给实现”，意味不同的实现下，行为可能不同。所以实际开发时，尽量避开这些雷区。
2. 注意C++存在多个标准，很多功能的实现在不同标准下可能是不同的，优先类、函数可能只在特点版本下存在。
3. 待补充索引：
	3.1.1 vector、array
	3.2.1  3.字符串输入 cin的高级特性
	3.2.2 1 string cin友元概念
	3.5.2 作用域内枚举
	3.6.6 vector、array
	7.1.1 protected
	7.1.6 静态成员
	7.2.2友元，待补充
	7.2.5 移动构造函数