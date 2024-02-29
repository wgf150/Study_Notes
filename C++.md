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
	C++中，空白包括了，空格、制表符、回车，但部分情况下，有标记（逗号、括号）可以不需要空白来分开
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
	* C++（C也一样）不允许函数定义嵌套，即函数体中，不允许定义函数；但是函数体中，声明函数是允许的。
	```C++
	//下述代码语法正确
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


## 2 数据结构

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
	
| 符号常量 | 表示 |
|:------|:------|
| CHAR_BIT | char的位数 |
| CHAR_MAX | char的最大值 |
| CHAR_MIN | char的最小值 |
| SCHAR_MAX | signed char的最大值 |
| SCHAR_MIN | signed char的最小值 |
| UCHAR_MAX | unsigned char的最大值 |
| SHRT_MAX | short的最大值 |
| SHRT_MIN | short的最小值 |
| USHRT_MAX | unsigned short的最大值 |
| INT_MAX | int的最大值 |
| INT_MIN | int的最小值 |
| UNIT_MAX | unsigned int的最大值 |
| LONG_MAX | long的最大值 |
| LONG_MIN | long的最小值 |
| ULONG_MAX | unsigned long的最大值 |
| LLONG_MAX | long long的最大值 |
| LLONG_MIN | long long的最小值 |
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

#### 2.2.4 整型常量的类型
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
	`\u`后面加8个十六进制位（`\uXXXX`）,`\U`后面加16个十六进制位（`\UXXXXXXXX`）。
	**注：** 编码名保证 名一致，而不是编码一致。
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
	char16_t和char32_t 为C++11特有类型，都是无符号
	char16_t用前缀u表示，char32_t用前缀U表示。
#### 2.2.6 “字符是如何输出的”

本节主要为个人探究，结合了实际测试（ubuntu gcc 7.5.0环境）、网上查询和个人理解。本节内容则都是围绕着这个核心议题展开：“字符是如何输出的”。
##### 1. 字符集与编码规则
首先，我们需要了解一些基本概念
字符集，是字符字面值的集合，例如 a~z 26个小写字母就可以构成一个字符集；
编码规则，则是规定字符集中的字符，如何转化为可供机器识别的序列。
通常，一个字符，可以存在于多个字符集，一个字符集，也可以对应多个编码规则

常见的字符集有 **ASCII**、**ANSI**、**Unicode**

| 字符集 | 介绍 | 编码规则 |
| ---- | ---- | ---- |
| **ASCII** | 最常见的基础字符集，包含英文大小写字母、阿拉伯数字等 128个字符。 | 唯一，详见ASCII码表 |
| **ANSI** | 扩展ASCII字符集，编码时使用 0x00~0x7f表示基础编码，0x80~0xffff表示拓展编码。 | 不同的国家和地区制定了不同的标准（如中文的 GB2312、GBK，日文的JIS） |
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
> 对locale的深入解读，以及为何 `codecvt_byname` 会是wcout输出的关键，见后续章节： [[C++#5. C++中的locale（字符相关）]] 

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

#### 2.4 浮点数
编码通常由两部分，一是表示值，二是表示小数点的位置，即控制小数点的移动，因此得名浮点数

#### 2.4.1 浮点数的字面值
通常有两种表示形式：
1. 标准小数点表示：
	如：2.24、3.0
	注：广义上的小数点，在如欧洲等一些地区数制里，小数点可能是逗号而不是句号
2. E表示法（类似科学计数法）：
	如：2.2E6、-12e-12
	E表示法的标准模板：整数或小数+(E/e)+整数。
	注：这里的后缀 En，表示乘 10的n次方，而不是 2。

#### 2.4.2 浮点类型
C++浮点类型主要有三类：float、double、long double

