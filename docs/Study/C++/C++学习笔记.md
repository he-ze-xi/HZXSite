﻿﻿@[TOC](目录)
# 前言
`大学期间浅学过C++语言，最近工作之余想再细致学习下C++语言。`
`该笔记部分摘抄自他人并改写，部分自己总结`

---

![在这里插入图片描述](C++学习笔记.assets/fcaa640c6b0046129a0e6818449b282e.jpeg)
### 1. 输入输出
iostream库包含2个基础类型：istream和ostream。

- stream(流)：一个流就是一个字符序列，是从IO设备流出或写入设备的。”流”表达的就是，随着时间的推移，字符是顺序生成的或消耗的。
- istream流：input stream，输入流。
- ostream流：output stream，输出流。

cin与键盘配合形成流，提取流过来的字符，并将其作为数值存放到变量中。

标准库定义了4个IO对象：
1. cin：istream类型的对象，标准输入(standard input)
2. cout：ostream类型的对象，标准输出(standard ouput)
3. cerr：ostream类型的对象，标准错误(standard error)
4. clog：ostream类型的对象，用来输出程序运行时的一般信息。

代码举例：

```cpp
#include<iostream>//标准库的头文件

/*
简单主函数：读取2个数并求出他们的和
*/
int main()
{
	std::cout << "Enter two number:" << std::endl;	//前缀std::是标准库的命名空间
	int v1 = 0,v2 = 0;
	std::cin >> v1 >> v2;	//  1.>>是输入运算符，左边必须是一个istream对象;2.cin可以跳过空格、制表符、换行符等空白字符
	std::cout << "The sum of" << v1 << "and" << v2 << "is" << v1 + v2 << "!"<<std::endl; 	// 1.<<是输入运算符，右边必须是一个ostream对象;2.endl是操作符，用于结束当前行，将设备相关的缓冲区内容刷到设备中。
	return 0;
}

```

### 2. 指针

**指针的作用：** 可以通过指针间接访问内存

* 内存编号是从0开始记录的，一般用十六进制数字表示
* 可以利用指针变量保存地址
#### 1. 指针变量的定义和使用

指针变量定义语法： `数据类型 * 变量名；`

**示例：**

```cpp
int main() {

	//1、指针的定义
	int a = 10; //定义整型变量a
	
	//指针定义语法： 数据类型 * 变量名 ;
	int * p;

	//指针变量赋值
	p = &a; //指针指向变量a的地址
	cout << &a << endl; //打印数据a的地址
	cout << p << endl;  //打印指针变量p

	//2、指针的使用
	//通过*操作指针变量指向的内存
	cout << "*p = " << *p << endl;

	system("pause");
	return 0;
}
```

指针变量和普通变量的区别

* 普通变量存放的是数据,指针变量存放的是地址
* 指针变量可以通过" * "操作符，操作指针变量指向的内存空间，这个过程称为解引用

> 总结1： 我们可以通过 & 符号 获取变量的地址

> 总结2：利用指针可以记录地址

> 总结3：对指针变量解引用，可以操作指针指向的内存

指针也是种数据类型，那么这种数据类型占用多少内存空间？

> 总结：所有指针类型在32位操作系统下是4个字节，在64位操作系统下是8个字节。



#### 2. 空指针和野指针

**空指针**：指针变量指向内存中编号为0的空间

**用途：** 初始化指针变量

**注意：** 空指针指向的内存是不可以访问的

```cpp
int main() {

	//指针变量p指向内存地址编号为0的空间
	int * p = NULL;

	//访问空指针报错 
	//内存编号0 ~255为系统占用内存，不允许用户访问
	cout << *p << endl;

	system("pause");
	return 0;
}
```

**野指针**：指针变量指向非法的内存空间

```cpp
int main() {

	//指针变量p指向内存地址编号为0x1100的空间
	int * p = (int *)0x1100;

	//访问野指针报错 
	cout << *p << endl;

	system("pause");
	return 0;
}
```

> 总结：空指针和野指针都不是我们申请的空间，因此不要访问。



#### 3. const修饰指针

const修饰指针有三种情况

1. const修饰指针   --- 常量指针
2. const修饰常量   --- 指针常量
3. const即修饰指针，又修饰常量


```cpp
int main() {

	int a = 10;
	int b = 10;

	//const修饰的是指针，指针指向可以改，指针指向的值不可以更改
	const int * p1 = &a; 
	p1 = &b; //正确
	//*p1 = 100;  报错

	//const修饰的是常量，指针指向不可以改，指针指向的值可以更改
	int * const p2 = &a;
	//p2 = &b; //错误
	*p2 = 100; //正确

    //const既修饰指针又修饰常量
	const int * const p3 = &a;
	//p3 = &b; //错误
	//*p3 = 100; //错误

	system("pause");
	return 0;
}
```

> 技巧：看const右侧紧跟着的是指针还是常量, 是指针就是常量指针，是常量就是指针常量



#### 4. 指针和数组

**作用：** 利用指针访问数组中元素

```cpp
int main() {

	int arr[] = { 1,2,3,4,5,6,7,8,9,10 };
	int * p = arr;  //指向数组的指针

	cout << "第一个元素： " << arr[0] << endl;
	cout << "指针访问第一个元素： " << *p << endl;

	for (int i = 0; i < 10; i++) {
		//利用指针遍历数组
		cout << *p << endl;
		p++;
	}

	system("pause");
	return 0;
}
```



#### 5. 指针和函数

**作用：** 利用指针作函数参数，可以修改实参的值

```cpp
//值传递
void swap1(int a ,int b) {
    
	int temp = a;
	a = b; 
	b = temp;
}

//地址传递
void swap2(int * p1, int *p2) {
    
	int temp = *p1;
	*p1 = *p2;
	*p2 = temp;
}

int main() {

	int a = 10;
	int b = 20;
    
	swap1(a, b); // 值传递不会改变实参
	swap2(&a, &b); //地址传递会改变实参

	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

	system("pause");
	return 0;
}
```

> 总结：如果不想修改实参，就用值传递，如果想修改实参，就用地址传递



#### 6. 数组和函数

```cpp
// 传指针
void myFunction(int *param){}

// 传已定义大小的数组
void myFunction(int param[10]){}

// 传未知大小数组
void myFunction(int param[]){}
```

上面**三种方式完全一样**，调用函数传入数组名，即**传入了指针**。
#### 7. const修饰函数参数

| 函数                            | 实参无定义const | 实参有定义const | 形参在函数体中 | 实参结果值 |
| ------------------------------- | --------------- | --------------- | -------------- | ---------- |
| int demo(int i)                 | 可输入          | 可输入          | 可改变         | 不会改变   |
| int demo(**const** int i)       | 可输入          | 可输入          | 不可改变       | 不会改变   |
| int demop(int***** a)           | 可输入          | 不可输入        | 可改变         | 不会改变   |
| int demop(**const** int *****a) | 可输入          | 可输入          | 不可改变       | 不会改变   |
| int demop(int***** **const** a) | 可输入          | 不可输入        | 可改变         | 不会改变   |
| void demox(int **&**p)          | 可输入          | 可输入          | 可改变         | 可改变     |

### 3.结构体

结构体属于用户==自定义的数据类型==，允许用户存储不同的数据类型。

#### 1. 结构体定义和使用

**语法：**`struct 结构体名 { 结构体成员列表 }；`

通过结构体创建变量的方式有三种：

* struct 结构体名 变量名
* struct 结构体名 变量名 = { 成员1值 ， 成员2值...}
* 定义结构体时顺便创建变量

```cpp
//结构体定义
struct student {
	//成员列表
	string name;  //姓名
	int age;      //年龄
	int score;    //分数
}stu3; //结构体变量创建方式3 

int main() {

	//结构体变量创建方式1
	struct student stu1; //struct 关键字可以省略

	stu1.name = "张三";
	stu1.age = 18;
	stu1.score = 100;
	
	cout << "姓名：" << stu1.name << " 年龄：" << stu1.age << endl;

	//结构体变量创建方式2
	struct student stu2 = { "李四",19,60 };
    student stu4{"李四",19,60}; // 省略=
    
	cout << "姓名：" << stu2.name << " 年龄：" << stu2.age << endl;

	stu3.name = "王五";
	stu3.age = 18;
	stu3.score = 80;

	cout << "姓名：" << stu3.name << " 年龄：" << stu3.age<< endl;

	system("pause");
	return 0;
}
```

**定义结构体**时的关键字是struct，不可省略。

#### 2. 结构体数组

**作用：** 将自定义的结构体放入到数组中方便维护

**语法：**` struct 结构体名 数组名[元素个数] = {  {} , {} , ... {} }`

```cpp
//结构体定义
struct student
{
	//成员列表
	string name;  //姓名
	int age;      //年龄
	int score;    //分数
}

int main() {
	
	//结构体数组
	struct student arr[3]= {
		{"张三",18,80 },
		{"李四",19,60 },
		{"王五",20,70 }
	};

	for (int i = 0; i < 3; i++) {
		cout << "姓名：" << arr[i].name << " 年龄：" << arr[i].age << endl;
	}

	system("pause");
	return 0;
}
```



#### 3. 结构体指针

**作用：** 通过指针访问结构体中的成员

* 利用操作符 `-> `可以通过结构体指针访问结构体属性

```cpp
//结构体定义
struct student
{
	//成员列表
	string name;  //姓名
	int age;      //年龄
	int score;    //分数
};


int main() {
	
	struct student stu = { "张三",18,100, };
	
	struct student * p = &stu;
	
	p->score = 80; //指针通过 -> 操作符可以访问成员

	cout << "姓名：" << p->name << " 年龄：" << p->age << endl;
	
	system("pause");
	return 0;
}
```



#### 4. 结构体嵌套结构体

**作用：** 结构体中的成员可以是另一个结构体。

```cpp
//学生结构体定义
struct student
{
	//成员列表
	string name;  //姓名
	int age;      //年龄
	int score;    //分数
};

//教师结构体定义
struct teacher
{
    //成员列表
	int id; //职工编号
	string name;  //教师姓名
	int age;   //教师年龄
	struct student stu; //子结构体 学生
};


int main() {

	struct teacher t1;
	t1.id = 10000;
	t1.name = "老王";
	t1.age = 40;

	t1.stu.name = "张三";
	t1.stu.age = 18;
	t1.stu.score = 100;

	cout << "教师 职工编号： " << t1.id << " 姓名： " << t1.name << endl;
	cout << "辅导学员 姓名： " << t1.stu.name << " 年龄：" << t1.stu.age << endl;

	system("pause");
	return 0;
}
```



#### 5. 结构体做函数参数

**作用：** 将结构体作为参数向函数中传递

传递方式有两种：

* 值传递
* 地址传递

```cpp
//学生结构体定义
struct student
{
	//成员列表
	string name;  //姓名
	int age;      //年龄
	int score;    //分数
};

//值传递
void printStudent(student stu ){
    
	stu.age = 28;
	cout << "子函数中 姓名：" << stu.name << " 年龄： " << stu.age << endl;
}

//地址传递
void printStudent2(student *stu){
    
	stu->age = 28;
	cout << "子函数中 姓名：" << stu->name << " 年龄： " << stu->age << endl;
}

int main() {

	student stu = { "张三",18,100};
	//值传递
	printStudent(stu);
	cout << "主函数中 姓名：" << stu.name << " 年龄： " << stu.age << endl;
	cout << endl;

	//地址传递
	printStudent2(&stu);
	cout << "主函数中 姓名：" << stu.name << " 年龄： " << stu.age << endl;

	system("pause");
	return 0;
}
```

> 总结：如果不想修改主函数中的数据，用值传递，反之用地址传递



#### 6. 结构体中 const使用场景

**作用：** 用const来防止误操作

```cpp
//学生结构体定义
struct student
{
	//成员列表
	string name;  //姓名
	int age;      //年龄
	int score;    //分数
};

//const使用场景
void printStudent(const student *stu){ //加const防止函数体中的误操作
    
	//stu->age = 100; //操作失败，因为加了const修饰
	cout << "姓名：" << stu->name << " 年龄：" << stu->age << endl;
}

int main() {

	student stu = { "张三",18,100 };

	printStudent(&stu);

	system("pause");
	return 0;
}
```

#### 7. 共用体

共用体类似与结构体，但同一时间只能存储一个数据。

```cpp
union id{
    int id_int;
    double id_d1;
    double id_d2;
};

id num;
num.id_int = 7; // 创建数据
num.id_d1 = 67.89; // 覆盖id_int
num.id_d2 = 546.879; // 覆盖id_d1
```

```cpp
struct student
{
	string name;
	int age;
	union {   // 匿名共用体
        double double_score;
        char char_score;
    };
};

student st;
st.name = "张三";
st.age = 21;
st.char_score = 'A'; // 使用结构体调用。
```


### 4. typedef、auto、decltype
随着程序越来越复杂，程序中用到的类型也越来越复杂。
* 无法明确表示真实含义；
* 搞不清变量到底需要什么类型；

#### 1. typedef类型别名

```cpp
typedef double wages;
typedef wages base,*p;//base是double的同义词，p是double *的同义词
using Sl=Sales_item;//C++11，别名声明
wages hourly,weekly;
Sl item;//等价于Sales_item item
```

对于指针这种复合类型，类型别名的使用可能会产生意想不到的结果：

```cpp
typedef char *pstring;
const pstring cstr=0;//指向char的常量指针
const string *ps=0;//ps是指针变量，它的对象是指向char的常量指针
```
#### 2. auto
auto类型说明符：c++11,让编译器通过初始值来推断变量的类型。

```cpp
    auto item=val1+val2;
    
    auto i=0,*p=&i;//正确
    auto sz=0,pi=3.14;//错误，auto已经被推断为int类型，却需要被推断为double类型
```
#### 3. decltype
* 选择并返回操作数的数据类型；
* 只要数据类型，不要其值；

```cpp
    decltype (f()) sum=x;//sum的类型就是函数f返回的类型
    
    const int ci=0,&cj=ci;
    decltype(ci) x=0;//x的类型是const int
    decltype (cj) y=x;//y的类型是const &int
```
### 5.命名空间using的声明

头文件中尽量不要包含using声明，因为会被拷贝到引用该头文件中的文件中。

### 6.标准库类型string
1. 长度可变的字符序列，需要包含string头文件。

```cpp
#include <iostream>
#include <string>

using std::string;
int main()
{
    string s1;//空字符串
    string s2 = s1;//副本，等价于说(s1)
    string s3 = "Hello World!";//副本，等价于s3( "Hello World!")
    string s4(10, 'c');//可以理解为定义一个容器，大小为10，每一个元素的值为c，即s4=cccccccccc

    return 0;
}
```
2. C++将c语言标准库中的内容，命名为cname，不包含.h。
ctype头文件(ctype.h)中的函数：

|  |  |
|--|--|
|函数名称|返回值|
|isalnum(）|如果参数是字母数字,即字母或数字,该函数返回true|
|isalpha()|如果参数是字母,该函数返回true|
|iscntrl()|如果参数是控制字符,该函数返回true|
|isdigit0|如果参数是数字(0～9），该函数返回 true|
|isgraph()|如果参数是除空格之外的打印字符,该函数返回 true|
|islower()|如果参数是小写字母,该函数返回 true|
|isprint()	|如果参数是打印字符（包括空格）,该函数返回 true|
|ispunct()|如果参数是标点符号,该函数返回true|
|isspace(）|如果参数是标准空白字符,如空格、进纸、换行符、回车、水平制表符或者垂直制表符，该函数返回true|
|isupper()|如果参数是大写字母,该函数返回true|
|isxdigit()	|如果参数是十六进制数字，即0~9、a～f或A～F，该函数返回 true|
|tolower()|如果参数是大写字符,则返回其小写,否则返回该参数|
|toupper(）|如果参数是小写字符,则返回其大写,否则返回该参数|
|  |  |

### 7.include

1.  包含头文件的操作，通常有两种格式：

```cpp
#include <header-file> // *.h 文件
#include "header-file"12 // *.h 文件
```

2.  <>和”“表示编译器在搜索头文件时的顺序不同：

```cpp
<>表示从系统目录下开始搜索，然后再搜索PATH环境变量所列出的目录，不搜索当前目录

""是表示从当前目录开始搜索，然后是系统目录和PATH环境变量所列出的目录123
```

3. 所以，系统头文件一般用<>，用户自己定义的则可以使用”“，加快搜索速度。除此外，写代码多了就会发现，有些头文件之间的相互包含是有隐藏依赖关系的，一定要加以注意。
###  8.using namespace

NameSpace（名字空间），是为了解决命名冲突的问题而引入的概念。

1. 当**调用一个库中命名空间的标识符**时，有以下三种方式：

  * 直接指定标识符。例如std::ostream而不是ostream。完整语句如下： std::cout << std::hex << 3.4 << std::endl;

   * 使用using关键字。 using std::cout; using std::endl; using std::cin; 以上程序可以写成 cout << std::hex << 3.4 << endl;

  *  最方便的就是使用using namespace std; 这样命名空间std内定义的所有标识符都有效（曝光）。

```cpp
#include <iostream>

int main(){
    std::cout << 43 << std::endl; // 直接指定标识符

    return 0;
}
```

```cpp
#include <iostream>
using std::cout; // 使用using关键字
using std::endl; // 使用using关键字

int main(){
    cout << 43 << endl;

    return 0;
}
```

```cpp
#include <iostream>
using namespace std; // 使用using命名空间

int main(){
    cout << 43 << endl;

    return 0;
}
```

### 8.std

std是iostream库中定义的标识符所在的一个命名空间，`using namespace std;`是导入iostream库中的部分标识符。

一些其他常用库中也有std命名空间。
### 9. C++关键字
C++中关键字类型：

| 关键字     |              |                 |             |          |
| ---------- | ------------ | --------------- | ----------- | -------- |
| asm        | do           | if              | return      | typedef  |
| auto       | double       | inline          | short       | typeid   |
| bool       | dynamic_cast | int             | signed      | typename |
| break      | else         | long            | sizeof      | union    |
| case       | enum         | mutable         | static      | unsigned |
| catch      | explicit     | namespace       | static_cast | using    |
| char       | export       | new             | struct      | virtual  |
| class      | extern       | operator        | switch      | void     |
| const      | false        | private         | template    | volatile |
| const_cast | float        | protected       | this        | wchar_t  |
| continue   | for          | public          | throw       | while    |
| default    | friend       | register        | true        |          |
| delete     | goto         | reinterpet_cast | try         |          |

#### 1.auto

 auto 表示变量的自动类型推断。即在声明变量的时候，根据变量初始值的类型自动为此变量选择匹配的类型。

```cpp
auto x = 3; // x 为 int 类型
cout << typeid(x).name() << endl;
```

auto 变量必须在定义时初始化，这类似于const关键字。

#### 2.bool、true、false

bool 类型是C++ 中的基本数据结构。bool 类型只有两个取值，true 和 false。true 表示“真”，false 表示“假”。

bool 类型常用于条件判断、开关变量的值或函数返回值。

#### 3.char、wchar_t

char 类型表示单个字符。char 类型的数据需要用单引号括起来：

```cpp
char letter ='A';
```

wchar_t 是宽字符类型，每个 wchar_t 类型占2个字节，16位宽。汉字的表示就需要用到 wchar_t。

字符与整数密切相关，它们在内部其实是被存储为整数。每个可打印的字符以及许多不可打印的字符都被分配一个唯一的数字。用于编码字符的最常见方法是 ASCII（美国信息交换标准代码的首字母简写）。

#### 4.int、short、long

int 类型用于表示整数。

short 类型用于表示短整型整数，数值范围小于int。

long 类型用于表示长整型整数，数值范围大于int。

#### 5.float、double、long double

float 类型用于表示单精度浮点数。

double 类型用于表示双精度浮点数，double比float的范围大、有效数字多。long double 比 double 的精度更大。

当某个浮点值被分配给整型变量时，该值的小数部分（即小数点后的部分）将被丢弃。

```cpp
int num = 1.23; // num 值为1
```

#### 6.signed、unsigned

signed（有符号），表明该类型是有符号数，和 unsigned（无符号）相反。数字类型（整型和浮点型）默认就是 signed。

#### 7.enum

enum 表示枚举类型，可以给出一系列固定值，实质上是 int 类型。

```cpp
enum color {
    RED = 0,
    GREEN = 1,
    BLUE = 2 
};
```

#### 8.union

union 是联合体类型，通过共享内存，一个union可以有多个数据成员。但在任意时刻，联合中只能有一个数据成员可以有值。例如

```cpp
union price {
    char x
    int y;
    double z; 
}; 
```

#### 9.struct、class

class是一般的类类型，struct在C++中是特殊的类类型，声明中默认的访问权限与class不同，struct是public，class是private。

#### 10.sizeof

**作用：** sizeof 运算法用于获取数据类型占用的字节数即所占内存大小。

**语法：** sizeof(数据类型 / 变量)。

```cpp
int main() {

	cout << "short 类型所占内存空间为： " << sizeof(short) << endl;
	cout << "long long 类型所占内存空间为： " << sizeof(long long) << endl;

	system("pause");
	return 0;
}
```

```cpp
// 使用方式：
int a = 1;
int p = sizeof(a);
int q = sizeof a; // 对于变量可以不使用括号
```



#### 11.typeid

typeid运算符可以输出变量的类型。

#### 12.typedef

typedef 可以为现有数据类型创建一个别名，便于程序的阅读和编写。

#### 13.static

用于声明静态变量或类的静态函数。静态变量作用范围在一个文件内，程序开始时分配空间，结束时释放空间，默认初始化为 0，使用时可改变其值。

C++ 类的成员变量被声明为 static（称为静态成员变量），意味着它被该类的所有实例所共享，也就是说当某个类的实例修改了该静态成员变量，其修改值为该类的其它所有实例所见；而类的静态成员函数也只能访问静态成员（变量或函数）。

#### 14.public、protected、private

权限修饰符。

- public为公有的，访问不受限制；
- protected为保护的，只能在本类、派生类中访问；
- private为私有的，只能在本类中访问。

#### 15.virtual

用于声明虚基类、虚函数。虚函数=0时，则为纯虚函数，纯虚函数所在的类称为抽象类。

#### 16.override、final

- override 用于表示当前函数重写了基类的虚函数。
- final 用于禁止类继承、禁止重载虚函数。

```cpp
class Base {
public:
    virtual void g(); // 虚函数
    virtual void h() = 0; // 纯虚函数
};

class Derived : public Base {
    void g() override; // 表示派生类重写基类虚函数
    void h() final; // 表示不可再被派生类进一步重载
};
```

#### 17.operator

用于重载操作符。如下重载类Person的 == 运算法。

```cpp
#include <iostream>

class Person{
public:
    Person(int a): age(a){}
    bool operator == (const Person& p) {
        if (age == p.age){
            return true;
        }
        return false;
    }
    
private:
    int age = 0;
};

int main(){
    Person p1(3);
    Person p2(3);
    Person p3(5);

    std::cout << "p1 == p2: "<< (p1 == p2) << std::endl;
    std::cout << "p1 == p3: "<< (p1 == p3) << std::endl;

    return 0;
}

// p1 == p2: 1
// p1 == p3: 0
```

#### 18.const、constexpr

- const 表示所修饰的对象或变量不能被改变。
- constexpr 用于生成常量表达式，常量表达式主要是允许一些计算发生在编译时，而不是运行的时候。

#### 19.using

用于在当前文件引入命名空间，例如：using namespace std；

在子类中，使用 using 声明引入基类成员名称。

#### 20.namespace

C++标准程序库中的所有标识符都被定义于一个名为 std 的namespace中。

命名空间除了系统定义的名字空间之外，还可以自己定义，定义命名空间用关键字 namespace，使用命名空间时用符号 :: 指定。

#### 21.inline

声明为内联函数，即在编译时将所调用的函数代码直接嵌入到主调函数中。

#### 22.new、delete

new 用于向内存申请一段新的空间，delete 用于释放申请空间。

#### 23.this

每个类成员函数都隐含了一个this指针，用来指向类本身。

this指针一般可以省略，但在赋值运算符重载的时候要显示使用。静态成员函数没有this指针。

#### 24.nullptr

C++11新引入的，用来声明一个 空指针，代替NULL。

```cpp
int* p = nullptr;
```

#### 25.void

特殊的"空"类型，指定函数无返回值或无参数。

#### 26.friend

用于声明友元关系。

友元可以访问与其有 friend 关系的类中的 private/protected 成员，通过友元直接访问类中的 private/protected 成员的主要目的是提高效率。

友元包括友元函数和友元类。

#### 27.template

模板，C++中泛型机制的实现。模板就是实现代码重用机制的一种工具，它可以实现类型参数化，即把类型定义为参数， 从而实现了真正的代码可重用性。模版可以分为两类，一个是函数模版，另外一个是类模版。

#### 28.if、else

用于条件语句。

#### 29.for、while、do

用于循环语句。

#### 30.switch、case、default

用于分支语句。switch 表示分支语句的起始，根据 switch 条件跳转到 case 标记或 defalut 标记的分支上。

#### 31.break、continue、goto

- break用于跳出for、while循环或switch语句。
- continue用于跳到一个循环的起始位置。
- goto用于无条件跳转到函数内的标记处，一般情况不建议使用goto。

#### 32.and、or、xor、not、bitand、bitor

- and 表示逻辑与 &&；
- or 表示逻辑或 ||；
- xor 表示逻辑异或 ^；
- not 表示逻辑非 !；
- bitand 表示按位与 &；
- bitor 表示按位或 |。

#### 33.return

return表示从被调函数返回到主调函数继续执行，返回时可带一个返回值。

#### 34.try、catch、throw

用于异常处理。try 指定 try 块的起始，try 块后的 catch 可以捕获异常，异常由 throw 抛出。

#### 35.noexcept

C++11中，用于声明一个函数不可以抛出任何异常。

#### 36.static_cast、const_cast、dynamic_cast、reinterpret_cast

C++类型风格来性转换：

- static_cast用于静态转换；
- const_cast删除const变量的属性，方便赋值；
- dynamic_cast用于将一个父类对象的指针转换为子类对象的指针或引用；
- reinterpret_cast将一种类型转换为另一种不同的类型。

#### 37.register
提示编译器尽可能把变量存入到CPU内部寄存器中。
#### 38.explicit
explicit 的作用是禁止单参数构造函数被用于自动类型转换，比较典型的是容器类型。
#### 39.extern

当出现extern “C”时，表示 extern “C”之后的代码按照C语言的规则去编译；

当extern修饰变量或函数时，表示其具有外部链接属性，即其既可以在本模块中使用也可以在其他模块中使用。

```cpp
#ifdef __cplusplus
extern "C" {
#endif
 
    // 按照C语言的规则去编译
 
#ifdef __cplusplus
}
#endif
```
### 10. 转义字符

**作用：** 用于表示一些不能显示出来的ASCII字符。
| **转义字符** | **含义**                                | **ASCII**码值（十进制） |
| ------------ | --------------------------------------- | ----------------------- |
| \a           | 警报                                    | 007                     |
| \b           | 退格(BS) ，将当前位置移到前一列         | 008                     |
| \f           | 换页(FF)，将当前位置移到下页开头        | 012                     |
| **\n**       | **换行(LF) ，将当前位置移到下一行开头** | **010**                 |
| \r           | 回车(CR) ，将当前位置移到本行开头       | 013                     |
| **\t**       | **水平制表(HT)  （跳到下一个TAB位置）** | **009**                 |
| \v           | 垂直制表(VT)                            | 011                     |
| **\\\\**     | **代表一个反斜线字符"\"**               | **092**                 |
| \\'          | 代表一个单引号（撇号）字符              | 039                     |
| \\"          | 代表一个双引号字符                      | 034                     |
| \?           | 代表一个问号                            | 063                     |
| \0           | 数字0                                   | 000                     |
| \ddd         | 8进制转义字符，d范围0~7                 | 3位8进制                |
| \xhh         | 16进制转义字符，h范围0~9，a~f，A~F      | 3位16进制               |

```cpp
int main() {
	
	cout << "\\" << endl;
	cout << "\tHello" << endl;
	cout << "\n" << endl;

	system("pause");
	return 0;
}
```

### 11. 窗口暂停

```cpp
system("pause");// 等待按键
```
### 12.数据的输入

**作用：用于从键盘获取数据**

#### 1. cin

**语法：** `cin >> 变量 `

```C++
int main(){

	//整型输入
	int a = 0;
	cout << "请输入整型变量：" << endl;
	cin >> a;

	//浮点型输入
	double d = 0;
	cout << "请输入浮点型变量：" << endl;
	cin >> d;

	//字符串型输入
	string str;
	cout << "请输入字符串型变量：" << endl;
	cin >> str;

	system("pause");
	return 0;
}
```

#### 2. cin.getline( )

输入一行字符。

```cpp
char a[30];
cin.getline(a, sizeof a); // 输入一行，除了换行符，其他字符不中断输入。

char a[20];
char b[30];
cin.getline(a, sizeof a, ' ').getline(b, sizeof b); // 从一行读取两个字符串，空格隔开。
```

#### 3. cin.get( )

读取一行到换行符之前。

```cpp
// 连续两个get，后面的一个会读取前一行最后的换行符，而不是读取第二行。
char a[20];
char b[30];
	// 输入："一行字符\n"
cin.get(a, sizeof a); // "一行字符"
cin.get(b, sizeof b); // "\n"

char a[20];
cin.get(a, sizeof a).get(); // 输入一行，get()表示读取一个字符。
```

#### 4.混用

```cpp
// 同cin.get(),cin读取后末尾会留下一个换行符。如果第二行不使用cin就会读取。
char a[20];
char b[20];
cin>>a; // 输入一行
cin.getline(b, sizeof b); // "\n"

(cin>>a).get(); //过滤掉换行符
cin.getline(b, sizeof b);

// 结束while(cin)使用Ctrl+z
```

#### 5. getline(cin, string s)

```cpp
string e;
getline(cin, e); // 读取一行到string, 第一个参数是cin。
```

#### 6. getchar( )

- getchar( )读取并返回一个字符（char类型）。

```cpp
// 逐个读取字符并处理。
int c;
while((c = getchar()) != EOF); // 逐个读取并赋值给c，遇到换行符结束读取。
```

#### 7. scanf( )

- c 库标准输入函数。

```cpp
#include<stdio.h>

// 输入一个数字
int a;
scanf("%d", &a);

// 输入两个数字，用空格隔开
int a, b;
scanf("%d %d", &a,&b);

// 返回值
// 返回读取数字、字符串的数量。
```

#### 8. 自定义输入流

```cpp
#include<iostream>
#include<string>
#include<sstream>
using namespace std;
int main() {
    string line;

    while(getline(cin, line)) {
        int sum = 0, x;
        stringstream ss(line); // 将line转换为输入流

        while(ss >> x) // 输入并转换类型
            sum += x;
        cout << sum << endl;
    }
    system("pause");
    return 0;
}
```
### 13. 类型转换

- 定义变量时类型转换
- {}方式初始化
- 传递参数时类型转换
- 使用运算进行转换
- 强制类型转换

```cpp
// 定义变量时类型转换
int a = 9.78;

// {}方式初始化
char b{32}; // 不能把浮点转换为int
const int c = 4;
float d{c};// 只能使用常量名，不能使用变量名。

// 强制类型转换, 不会改变原值，只是创建了新值。
int(3.54);
(int)5.65;
static_cast<int>(9.87);

// 数字转字符串
std::to_string(43);

// 字符串转数字
string s = "54";
atoi(s.c_str());
```

| 字符函数库（\<cctype>） | 说明                       |
| ----------------------- | -------------------------- |
| int isalnum(int c)      | 检查字符是否是字母和数字   |
| int isalpha(int c)      | 检查字符是否是字母         |
| int iscntrl(int c)      | 检查字符是否是控制字符     |
| int isdigit(int c)      | 检查字符是否是十进制数字   |
| int isgraph(int c)      | 检查字符是否有图形表示法   |
| int islower(int c)      | 检查字符是否是小写字母     |
| int isprint(int c)      | 检查字符是否是可打印的     |
| int ispunct(int c)      | 检查字符是否是标点符号字符 |
| int isspace(int c)      | 检查字符是否是空白字符     |
| int isupper(int c)      | 检查字符是否是大写字母     |
| int isxdigit(int c)     | 检查字符是否是十六进制数字 |
| int tolower(int c)      | 将大写字母转换为小写字母   |
| int toupper(int c)      | 将小写字母转换为大写字母   |

### 14.枚举

枚举是一个范围常量。枚举的量只能是**整数**。

```cpp
enum num{zero, one, tree, n=34, m}; // 枚举默认赋值为0,1,2,3...

cout << zero << endl; // 0
cout << tree << endl; // 2
cout << n << endl; // 34，手动赋值。
cout << m << endl; // 35，自动递增。

num age; // 定义枚举num对象的新常量
age = num(30); // 为其赋值，赋值范围不应比num内最大整数多1个bit。
```
### 15. goto语句

**作用：** 可以无条件跳转语句

**语法：** goto 标记;

**解释：** 如果标记的名称存在，执行到goto语句时，会跳转到标记的位置。

```cpp
int main() {

	cout << "1" << endl;

	goto FLAG;

	cout << "2" << endl;
	cout << "3" << endl;
	cout << "4" << endl;

	FLAG:

	cout << "5" << endl;
	
	system("pause");
	return 0;
}
```

### 16.数组

所谓数组，就是一个集合，里面存放了相同类型的数据元素。

**特点1：** 数组中的每个==数据元素都是相同的数据类型==

**特点2：** 数组是由==连续的内存==位置组成的

#### 1.一维数组
##### 1. 一维数组定义方式

一维数组定义的多种方式：

1. ` 数据类型  数组名[ 数组长度 ]; `
2. `数据类型  数组名[ 数组长度 ] = { 值1，值2 ...};`
3. `数据类型  数组名[ ] = { 值1，值2 ...};`
4. `数据类型  数组名[数组长度]{ 值1，值2 ...};`
5. `数据类型  数组名[数组长度]{ };`

```cpp
int main() {

	//定义方式1
	//数据类型 数组名[元素个数];
	int score[10];

	//定义方式2
	//数据类型 数组名[元素个数] =  {值1，值2 ，值3 ...};
	//如果{}内不足10个数据，剩余数据用0补全
	int score2[10] = { 100, 90,80,70,60,50,40,30,20,10 };

	//定义方式3
	//数据类型 数组名[] =  {值1，值2 ，值3 ...};
	int score3[] = { 100,90,80,70,60,50,40,30,20,10 };

    //利用下标赋值
	score[0] = 100;
	score[1] = 99;

	//利用下标输出
	cout << score[0] << endl;
	cout << score[1] << endl;
    
    // 循环遍历输出
	for (int i = 0; i < 10; i++) {
		cout << score3[i] << endl;
	}

	system("pause");
	return 0;
}
```

- **数组初始化**：int，bool 类型数组要初始化，不初始化可能值为随机。

```cpp
bool b{0}; // 全部初始化为False，不支持初始化为True。
int n{0}; // 全部初始化为 0，不支持初始化为 其他值。
```



##### 2. 一维数组数组名

一维数组名称的**用途**：

1. 可以统计整个数组在内存中的长度
2. 可以获取数组在内存中的首地址

```cpp
int main() {
    
	//1、可以获取整个数组占用内存空间大小
	int arr[10] = { 1,2,3,4,5,6,7,8,9,10 };

	cout << "整个数组所占内存空间为： " << sizeof(arr) << endl;
	cout << "每个元素所占内存空间为： " << sizeof(arr[0]) << endl;
	cout << "数组的元素个数为： " << sizeof(arr) / sizeof(arr[0]) << endl;

	//2、可以通过数组名获取到数组首地址
	cout << "数组首地址为： " << (int)arr << endl;
	cout << "数组中第一个元素地址为： " << (int)&arr[0] << endl;
	cout << "数组中第二个元素地址为： " << (int)&arr[1] << endl;

	//arr = 100; 错误，数组名是常量，因此不可以赋值

	system("pause");
	return 0;
}
```



#### 2. 二维数组

二维数组就是在一维数组上，多加一个维度。

##### 1. 二维数组定义方式

二维数组定义的四种方式：

1. ` 数据类型  数组名[ 行数 ][ 列数 ]; `
2. `数据类型  数组名[ 行数 ][ 列数 ] = { {数据1，数据2 } ，{数据3，数据4 } };`
3. `数据类型  数组名[ 行数 ][ 列数 ] = { 数据1，数据2，数据3，数据4};`
4. ` 数据类型  数组名[  ][ 列数 ] = { 数据1，数据2，数据3，数据4};`

> 建议：以上4种定义方式，利用==第二种更加直观，提高代码的可读性==

```cpp
int main() {

	//方式1  
	//数组类型 数组名 [行数][列数]
	int arr[2][3];

	//方式2 
	//数据类型 数组名[行数][列数] = { {数据1，数据2 } ，{数据3，数据4 } };
	int arr2[2][3] = {
		{1,2,3},
		{4,5,6}
	};

	//方式3
	//数据类型 数组名[行数][列数] = { 数据1，数据2 ,数据3，数据4  };
	int arr3[2][3] = { 1,2,3,4,5,6 }; 

	//方式4 
	//数据类型 数组名[][列数] = { 数据1，数据2 ,数据3，数据4  };
	int arr4[][3] = { 1,2,3,4,5,6 };
	
	system("pause");
	return 0;
}
```



##### 2. 二维数组数组名

* 查看二维数组所占内存空间
* 获取二维数组首地址

```cpp
int main() {

	//二维数组数组名
	int arr[2][3] = {
		{1,2,3},
		{4,5,6}
	};

	cout << "二维数组大小： " << sizeof(arr) << endl;
	cout << "二维数组一行大小： " << sizeof(arr[0]) << endl;
	cout << "二维数组元素大小： " << sizeof(arr[0][0]) << endl;

	cout << "二维数组行数： " << sizeof(arr) / sizeof(arr[0]) << endl;
	cout << "二维数组列数： " << sizeof(arr[0]) / sizeof(arr[0][0]) << endl;

	//地址
	cout << "二维数组首地址：" << arr << endl;
	cout << "二维数组第一行地址：" << arr[0] << endl;
	cout << "二维数组第二行地址：" << arr[1] << endl;

	cout << "二维数组第一个元素地址：" << &arr[0][0] << endl;
	cout << "二维数组第二个元素地址：" << &arr[0][1] << endl;

	system("pause");
	return 0;
}
```

### 17.函数的分文件编写

**作用：** 让代码结构更加清晰

函数分文件编写一般有4个步骤

1. 创建后缀名为.h的头文件  
2. 创建后缀名为.cpp的源文件
3. 在头文件中写函数的声明
4. 在源文件中写函数的定义

```cpp
//swap.h文件
#include<iostream>
using namespace std;

//实现两个数字交换的函数声明
void swap(int a, int b);
```

```cpp
//swap.cpp文件
#include "swap.h"

void swap(int a, int b) {
    
	int temp = a;
	a = b;
	b = temp;

	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
}
```

```cpp
//main函数文件
#include "swap.h"
int main() {

	int a = 100;
	int b = 200;
	swap(a, b);

	system("pause");
	return 0;
}
```
### 18.内存分区模型

C++程序在执行时，将内存大方向划分为**4个区域**

- 代码区：存放函数体的二进制代码，由操作系统进行管理的
- 全局区：存放全局变量和静态变量以及常量
- 栈区：由编译器自动分配释放, 存放函数的参数值,局部变量等
- 堆区：由程序员分配和释放,若程序员不释放,程序结束时由操作系统回收

**内存四区意义：**

不同区域存放的数据，赋予不同的生命周期, 给我们更大的灵活编程。
#### 1. 程序运行前

​	在程序编译后，生成了exe可执行程序，**未执行该程序前**分为两个区域

​	**代码区：**

​		存放 CPU 执行的机器指令

​		代码区是**共享**的，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可

​		代码区是**只读**的，使其只读的原因是防止程序意外地修改了它的指令

​	**全局区：**

​		全局变量和静态变量存放在此.

​		全局区还包含了常量区, 字符串常量和其他常量也存放在此.

​		==该区域的数据在程序结束后由操作系统释放==.

```cpp
//全局变量
int g_a = 10;
int g_b = 10;

//全局常量
const int c_g_a = 10;
const int c_g_b = 10;

int main() {

	//局部变量
	int a = 10;
	int b = 10;

	//打印地址
	cout << "局部变量a地址为： " << (int)&a << endl;
	cout << "局部变量b地址为： " << (int)&b << endl;

	cout << "全局变量g_a地址为： " <<  (int)&g_a << endl;
	cout << "全局变量g_b地址为： " <<  (int)&g_b << endl;

	//静态变量
	static int s_a = 10;
	static int s_b = 10;

	cout << "静态变量s_a地址为： " << (int)&s_a << endl;
	cout << "静态变量s_b地址为： " << (int)&s_b << endl;

	cout << "字符串常量地址为： " << (int)&"hello world" << endl;
	cout << "字符串常量地址为： " << (int)&"hello world1" << endl;

	cout << "全局常量c_g_a地址为： " << (int)&c_g_a << endl;
	cout << "全局常量c_g_b地址为： " << (int)&c_g_b << endl;

	const int c_l_a = 10;
	const int c_l_b = 10;
	cout << "局部常量c_l_a地址为： " << (int)&c_l_a << endl;
	cout << "局部常量c_l_b地址为： " << (int)&c_l_b << endl;

	system("pause");
	return 0;
}
```

#### 2. 程序运行后

​	**栈区：**

​		由编译器自动分配释放, 存放函数的参数值,局部变量等

​		注意事项：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放

```cpp
int * func()
{
	int a = 10;
	return &a;
}

int main() {

	int *p = func();

	cout << *p << endl;
	cout << *p << endl;

	system("pause");
	return 0;
}
```

​	**堆区：**

​		由程序员分配释放,若程序员不释放,程序结束时由操作系统回收

​		在C++中主要利用new在堆区开辟内存

```cpp
int* func()
{
	int* a = new int(10);
	return a;
}

int main() {

	int *p = func();

	cout << *p << endl;
	cout << *p << endl;
    
	system("pause");
	return 0;
}
```



#### 3. new操作符

​	C++中利用==new==操作符在堆区开辟数据

​	堆区开辟的数据，由程序员手动开辟，手动释放，释放利用操作符 ==delete==

​	语法：` new 数据类型`

​	利用new创建的数据，会返回该数据对应的类型的指针

```cpp
int* func()
{
	int* a = new int(10);
	return a;
}

int main() {

	int *p = func();

	cout << *p << endl;
	cout << *p << endl;

	//利用delete释放堆区数据
	delete p;

	//cout << *p << endl; //报错，释放的空间不可访问

	system("pause");
	return 0;
}
```

```cpp
//堆区开辟数组
int main() {

	int* arr = new int[10];

	for (int i = 0; i < 10; i++)
	{
		arr[i] = i + 100;
	}

	for (int i = 0; i < 10; i++)
	{
		cout << arr[i] << endl;
	}
	//释放数组 delete 后加 []
	delete[] arr;

	system("pause");
	return 0;
}
```
### 19.引用
#### 1. 引用的基本使用

 **作用：** 给变量起别名

**语法：** `数据类型 &别名 = 原名`

```cpp
int main() {

	int a = 10;
	int &b = a;

	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

	b = 100;

	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

	system("pause");
	return 0;
}
```
#### 2. 引用注意事项

* 引用必须初始化
* 引用在初始化后，不可以改变

```cpp
int main() {

	int a = 10;
	int b = 20;
	//int &c; //错误，引用必须初始化
	int &c = a; //一旦初始化后，就不可以更改
	c = b; //这是赋值操作，不是更改引用

	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
	cout << "c = " << c << endl;

	system("pause");
	return 0;
}
```



#### 3. 引用做函数参数

**作用：** 函数传参时，可以利用引用的技术让形参修饰实参

**优点：** 可以简化指针修改实参

```cpp
//1. 值传递
void mySwap01(int a, int b) {
	int temp = a;
	a = b;
	b = temp;
}

//2. 地址传递
void mySwap02(int* a, int* b) {
	int temp = *a;
	*a = *b;
	*b = temp;
}

//3. 引用传递
void mySwap03(int& a, int& b) {
	int temp = a;
	a = b;
	b = temp;
}

int main() {

	int a = 10;
	int b = 20;

	mySwap01(a, b);
	cout << "a:" << a << " b:" << b << endl;

	mySwap02(&a, &b);
	cout << "a:" << a << " b:" << b << endl;

	mySwap03(a, b);
	cout << "a:" << a << " b:" << b << endl;

	system("pause");
	return 0;
}
```

> 总结：通过引用参数产生的效果同按地址传递是一样的。引用的语法更清楚简单



#### 4. 引用做函数返回值

作用：引用是可以作为函数的返回值存在的

注意：**不要返回局部变量引用**

用法：函数调用作为左值

```cpp
//返回局部变量引用
int& test01() {
	int a = 10; //局部变量
	return a;
}

//返回静态变量引用
int& test02() {
	static int a = 20;
	return a;
}

int main() {

	//不能返回局部变量的引用
	int& ref = test01();
	cout << "ref = " << ref << endl;
	cout << "ref = " << ref << endl;

	//如果函数做左值，那么必须返回引用
	int& ref2 = test02();
	cout << "ref2 = " << ref2 << endl;
	cout << "ref2 = " << ref2 << endl;

	test02() = 1000;

	cout << "ref2 = " << ref2 << endl;
	cout << "ref2 = " << ref2 << endl;

	system("pause");
	return 0;
}
```
#### 5. 引用的本质

本质：**引用的本质在c++内部实现是一个指针常量.**

```cpp
//发现是引用，转换为 int* const ref = &a;
void func(int& ref){
	ref = 100; // ref是引用，转换为*ref = 100
}
int main(){
	int a = 10;
    
    //自动转换为 int* const ref = &a; 指针常量是指针指向不可改，也说明为什么引用不可更改
	int& ref = a; 
	ref = 20; //内部发现ref是引用，自动帮我们转换为: *ref = 20;
    
	cout << "a:" << a << endl;
	cout << "ref:" << ref << endl;
    
	func(a);
	return 0;
}
```
#### 6. 常量引用

**作用：** 常量引用主要用来修饰形参，防止误操作

在函数形参列表中，可以加==const修饰形参==，防止形参改变实参

```cpp
//引用使用的场景，通常用来修饰形参
void showValue(const int& v) {
	//v += 10;
	cout << v << endl;
}

int main() {

	//int& ref = 10;  引用本身需要一个合法的内存空间，因此这行错误
	//加入const就可以了，编译器优化代码，int temp = 10; const int& ref = temp;
	const int& ref = 10;

	//ref = 100;  //加入const后不可以修改变量
	cout << ref << endl;

	//函数中利用常量引用防止误操作修改实参
	int a = 10;
	showValue(a);

	system("pause");
	return 0;
}
```

### 20. 函数默认参数

在C++中，函数的形参列表中的形参是可以有默认值的。

语法：` 返回值类型  函数名 （参数= 默认值）{}`

```cpp
int func(int a, int b = 10, int c = 10) {
	return a + b + c;
}

//1. 如果某个位置参数有默认值，那么从这个位置往后，从左向右，必须都要有默认值
//2. 如果函数声明有默认值，函数实现的时候就不能有默认参数
int func2(int a = 10, int b = 10);
int func2(int a, int b) {
	return a + b;
}
```
### 21. 函数占位参数

C++中函数的形参列表里可以有占位参数，用来做占位，调用函数时必须填补该位置

**语法：** `返回值类型 函数名 (数据类型){}`

```cpp
//函数占位参数 ，占位参数也可以有默认参数
void func(int a, int) {
	cout << "this is func" << endl;
}

int main() {

	func(10,10); //占位参数必须填补

	system("pause");
	return 0;
}
```
### 22. 函数重载

#### 1. 函数重载概述

**作用：** 函数名可以相同，提高复用性

**函数重载满足条件：**

* 同一个作用域下
* 函数名称相同
* 函数参数**类型不同**  或者 **个数不同** 或者 **顺序不同**

**注意:**  函数的返回值不可以作为函数重载的条件

```cpp
//函数重载需要函数都在同一个作用域下
void func(){
	cout << "func 的调用！" << endl;
}
void func(int a){
	cout << "func (int a) 的调用！" << endl;
}
void func(double a){
	cout << "func (double a)的调用！" << endl;
}
void func(int a ,double b){
	cout << "func (int a ,double b) 的调用！" << endl;
}
void func(double a ,int b){
	cout << "func (double a ,int b)的调用！" << endl;
}

/*
函数返回值不可以作为函数重载条件
int func(double a, int b) {
	cout << "func (double a ,int b)的调用！" << endl;
}
*/


int main() {

	func();
	func(10);
	func(3.14);
	func(10,3.14);
	func(3.14 , 10);
	
	system("pause");
	return 0;
}
```
#### 2. 函数重载注意事项

* 引用作为重载条件
* 函数重载碰到函数默认参数

**示例：**

```cpp
//函数重载注意事项
//1、引用作为重载条件

void func(int &a){
	cout << "func (int &a) 调用 " << endl;
}

void func(const int &a){
	cout << "func (const int &a) 调用 " << endl;
}


//2、函数重载碰到函数默认参数

void func2(int a, int b = 10){
	cout << "func2(int a, int b = 10) 调用" << endl;
}

void func2(int a){
	cout << "func2(int a) 调用" << endl;
}

int main() {
	
	int a = 10;
	func(a); //调用无const
	func(10);//调用有const

	//func2(10); //碰到默认参数产生歧义，需要避免

	system("pause");
	return 0;
}
```
### 23.封装
#### 1.  封装的意义

封装是C++面向对象三大特性之一

封装的意义：

* 将属性和行为作为一个整体，表现生活中的事物
* 将属性和行为加以权限控制

**封装意义一：**

​	在设计类的时候，属性和行为写在一起，表现事物

**语法：** `class 类名{访问权限： 属性/ 行为};`

```cpp
//圆周率
const double PI = 3.14;

//1、封装的意义
//将属性和行为作为一个整体，用来表现生活中的事物

//封装一个圆类，求圆的周长
//class代表设计一个类，后面跟着的是类名
class Circle
{
public:  //访问权限  公共的权限

	//属性
	int m_r;//半径

	//行为
	//获取到圆的周长
	double calculateZC(){
		//2 * pi  * r
		//获取圆的周长
		return  2 * PI * m_r;
	}
};

int main() {

	//通过圆类，创建圆的对象
	// c1就是一个具体的圆
	Circle c1;
	c1.m_r = 10; //给圆对象的半径 进行赋值操作

	//2 * pi * 10 = = 62.8
	cout << "圆的周长为： " << c1.calculateZC() << endl;

	system("pause");
	return 0;
}
```

**封装意义二：**

类在设计时，可以把属性和行为放在不同的权限下，加以控制

访问权限有三种：

1. public        公共权限  
2. protected 保护权限
3. private      私有权限

```cpp
//三种权限
//公共权限  public     类内可以访问  类外可以访问
//保护权限  protected  类内可以访问  类外不可以访问
//私有权限  private    类内可以访问  类外不可以访问

class Person{
	//姓名  公共权限
public:
	string m_Name;

	//汽车  保护权限
protected:
	string m_Car;

	//银行卡密码  私有权限
private:
	int m_Password;

public:
	void func(){
		m_Name = "张三";
		m_Car = "拖拉机";
		m_Password = 123456;
	}
};

int main() {

	Person p;
	p.m_Name = "李四";
	//p.m_Car = "奔驰";  //保护权限类外访问不到
	//p.m_Password = 123; //私有权限类外访问不到

	system("pause");
	return 0;
}
```
#### 2. struct和class区别

在C++中 struct和class唯一的**区别**就在于 **默认的访问权限不同**

区别：

* struct 默认权限为公共
* class   默认权限为私有
#### 3. 成员属性设置为私有

**优点1：** 将所有成员属性设置为私有，可以自己控制读写权限

**优点2：** 对于写权限，我们可以检测数据的有效性

```cpp
class Person {
public:

	//姓名设置可读可写
	void setName(string name) {
		m_Name = name;
	}
	string getName() {
		return m_Name;
	}

	//获取年龄 
	int getAge() {
		return m_Age;
	}
	//设置年龄
	void setAge(int age) {
		if (age < 0 || age > 150) {
			cout << "你个老妖精!" << endl;
			return;
		}
		m_Age = age;
	}

	//情人设置为只写
	void setLover(string lover) {
		m_Lover = lover;
	}

private:
	string m_Name; //可读可写  姓名
	int m_Age; //只读  年龄
	string m_Lover; //只写  情人
};


int main() {

	Person p;
	//姓名设置
	p.setName("张三");
	cout << "姓名： " << p.getName() << endl;

	//年龄设置
	p.setAge(50);
	cout << "年龄： " << p.getAge() << endl;

	//情人设置
	p.setLover("苍井");
	//cout << "情人： " << p.m_Lover << endl;  //只写属性，不可以读取

	system("pause");
	return 0;
}
```
### 24. 继承

**继承是面向对象三大特性之一**

考虑利用继承的技术，减少重复代码

#### 1. 继承的基本语法

`class A : public B;` 
A 类称为子类 或 派生类；B 类称为父类 或 基类

```cpp
//公共页面
class BasePage{
public:
	void header(){
		cout << "首页、公开课、登录、注册...（公共头部）" << endl;
	}
	void footer(){
		cout << "帮助中心、交流合作、站内地图...(公共底部)" << endl;
	}
	void left(){
		cout << "Java,Python,C++...(公共分类列表)" << endl;
	}
};

//Java页面
class Java : public BasePage{
public:
	void content(){
		cout << "JAVA学科视频" << endl;
	}
};
//Python页面
class Python : public BasePage{
public:
	void content(){
		cout << "Python学科视频" << endl;
	}
};

void test01(){
	//Java页面
	cout << "Java下载视频页面如下： " << endl;
	Java ja;
	ja.header();
	ja.footer();
	ja.left();
	ja.content();
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

**派生类中的成员，包含两大部分**：

一类是从基类继承过来的，一类是自己增加的成员。

从基类继承过过来的表现其共性，而新增的成员体现了其个性。

#### 2. 继承方式

继承的语法：`class 子类 : 继承方式  父类`

**继承方式一共有三种：**

* 公共继承
* 保护继承
* 私有继承

```cpp
class Base1{
public: 
	int m_A;
protected:
	int m_B;
private:
	int m_C;
};

//公共继承
class Son1 :public Base1{
public:
	void func(){
		m_A; //可访问 public权限
		m_B; //可访问 protected权限
		//m_C; //不可访问
	}
};

void myClass(){
	Son1 s1;
	s1.m_A; //其他类只能访问到公共权限
}


class Base2{
public:
	int m_A;
protected:
	int m_B;
private:
	int m_C;
};

//保护继承
class Son2:protected Base2{
public:
	void func(){
		m_A; //可访问 protected权限
		m_B; //可访问 protected权限
		//m_C; //不可访问
	}
};
void myClass2(){
	Son2 s;
	//s.m_A; //不可访问
}


class Base3
{
public:
	int m_A;
protected:
	int m_B;
private:
	int m_C;
};

//私有继承
class Son3:private Base3{
public:
	void func(){
		m_A; //可访问 private权限
		m_B; //可访问 private权限
		//m_C; //不可访问
	}
};
class GrandSon3 :public Son3{
public:
	void func(){
		//Son3是私有继承，所以继承Son3的属性在GrandSon3中都无法访问到
		//m_A;
		//m_B;
		//m_C;
	}
};
```



#### 3. 继承中的对象模型

**问题：** 从父类继承过来的成员，哪些属于子类对象中？

```cpp
class Base{
public:
	int m_A;
protected:
	int m_B;
private:
	int m_C; //私有成员只是被隐藏了，但是还是会继承下去
};

//公共继承
class Son :public Base{
public:
	int m_D;
};

void test01(){
	cout << "sizeof Son = " << sizeof(Son) << endl;
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

> 结论： 父类中私有成员也是被子类继承下去了，只是由编译器给隐藏后访问不到



#### 4. 继承中构造和析构顺序

子类继承父类后，当创建子类对象，也会调用父类的构造函数

问题：父类和子类的构造和析构顺序是谁先谁后？

```cpp
class Base {
public:
	Base(){
		cout << "Base构造函数!" << endl;
	}
	~Base(){
		cout << "Base析构函数!" << endl;
	}
};

class Son : public Base{
public:
	Son(){
		cout << "Son构造函数!" << endl;
	}
	~Son(){
		cout << "Son析构函数!" << endl;
	}
};


void test01(){
	//继承中 先调用父类构造函数，再调用子类构造函数，析构顺序与构造相反
	Son s;
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

> 总结：继承中 先调用父类构造函数，再调用子类构造函数，析构顺序与构造相反
#### 5. 继承同名成员处理方式

问题：当子类与父类出现同名的成员，如何通过子类对象，访问到子类或父类中同名的数据呢？

* 访问子类同名成员   直接访问即可
* 访问父类同名成员   需要加作用域

```cpp
class Base {
public:
	Base(){
		m_A = 100;
	}
	void func(){
		cout << "Base - func()调用" << endl;
	}
	void func(int a){
		cout << "Base - func(int a)调用" << endl;
	}

public:
	int m_A;
};


class Son : public Base {
public:
	Son(){
		m_A = 200;
	}

	//当子类与父类拥有同名的成员函数，子类会隐藏父类中所有版本的同名成员函数
	//如果想访问父类中被隐藏的同名成员函数，需要加父类的作用域
	void func(){
		cout << "Son - func()调用" << endl;
	}
public:
	int m_A;
};

void test01(){
	Son s;

	cout << "Son下的m_A = " << s.m_A << endl;
	cout << "Base下的m_A = " << s.Base::m_A << endl;

	s.func();
	s.Base::func();
	s.Base::func(10);

}
int main() {

	test01();
    
	system("pause");
	return EXIT_SUCCESS;
}
```

总结：

1. 子类对象可以直接访问到子类中同名成员
2. 子类对象加作用域可以访问到父类同名成员
3. 当子类与父类拥有同名的成员函数，子类会隐藏父类中同名成员函数，加作用域可以访问到父类中同名函数



#### 6. 继承同名静态成员处理方式

问题：继承中同名的静态成员在子类对象上如何进行访问？

静态成员和非静态成员出现同名，处理方式一致

- 访问子类同名成员   直接访问即可
- 访问父类同名成员   需要加作用域

```cpp
class Base {
public:
	static void func(){
		cout << "Base - static void func()" << endl;
	}
	static void func(int a){
		cout << "Base - static void func(int a)" << endl;
	}

	static int m_A;
};

int Base::m_A = 100;

class Son : public Base {
public:
	static void func(){
		cout << "Son - static void func()" << endl;
	}
	static int m_A;
};

int Son::m_A = 200;

//同名成员属性
void test01(){
	//通过对象访问
	cout << "通过对象访问： " << endl;
	Son s;
	cout << "Son  下 m_A = " << s.m_A << endl;
	cout << "Base 下 m_A = " << s.Base::m_A << endl;

	//通过类名访问
	cout << "通过类名访问： " << endl;
	cout << "Son  下 m_A = " << Son::m_A << endl;
	cout << "Base 下 m_A = " << Son::Base::m_A << endl;
}

//同名成员函数
void test02(){
	//通过对象访问
	cout << "通过对象访问： " << endl;
	Son s;
	s.func();
	s.Base::func();

	cout << "通过类名访问： " << endl;
	Son::func();
	Son::Base::func();
	//出现同名，子类会隐藏掉父类中所有同名成员函数，需要加作作用域访问
	Son::Base::func(100);
}
int main() {

	//test01();
	test02();

	system("pause");
	return 0;
}
```

> 总结：同名静态成员处理方式和非静态处理方式一样，只不过有两种访问的方式（通过对象 和 通过类名）



#### 7. 多继承语法

C++允许**一个类继承多个类**

语法：` class 子类 ：继承方式 父类1 ， 继承方式 父类2...`

多继承可能会引发父类中有同名成员出现，需要加作用域区分

**C++实际开发中不建议用多继承**

```cpp
class Base1 {
public:
	Base1(){
		m_A = 100;
	}
public:
	int m_A;
};

class Base2 {
public:
	Base2(){
		m_A = 200;  //开始是m_B 不会出问题，但是改为mA就会出现不明确
	}
public:
	int m_A;
};

//语法：class 子类：继承方式 父类1 ，继承方式 父类2 
class Son : public Base2, public Base1 {
public:
	Son(){
		m_C = 300;
		m_D = 400;
	}
public:
	int m_C;
	int m_D;
};


//多继承容易产生成员同名的情况
//通过使用类名作用域可以区分调用哪一个基类的成员
void test01(){
	Son s;
	cout << "sizeof Son = " << sizeof(s) << endl;
	cout << s.Base1::m_A << endl;
	cout << s.Base2::m_A << endl;
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

> 总结： 多继承中如果父类中出现了同名情况，子类使用时候要加作用域



#### 8. 菱形继承

**菱形继承概念：**

​	两个派生类继承同一个基类

​	又有某个类同时继承者两个派生类

​	这种继承被称为菱形继承，或者钻石继承

```cpp
class Animal{
public:
	int m_Age;
};

//继承前加virtual关键字后，变为虚继承
//此时公共的父类Animal称为虚基类
class Sheep : virtual public Animal {};
class Tuo   : virtual public Animal {};
class SheepTuo : public Sheep, public Tuo {};

void test01(){
	SheepTuo st;
	st.Sheep::m_Age = 100;
	st.Tuo::m_Age = 200;

	cout << "st.Sheep::m_Age = " << st.Sheep::m_Age << endl;
	cout << "st.Tuo::m_Age = " <<  st.Tuo::m_Age << endl;
	cout << "st.m_Age = " << st.m_Age << endl;
}


int main() {

	test01();

	system("pause");
	return 0;
}
```

总结：

* 菱形继承带来的主要问题是子类继承两份相同的数据，导致资源浪费以及毫无意义
* 利用虚继承可以解决菱形继承问题

### 25.多态
#### 1. 多态的基本概念

**多态是C++面向对象三大特性之一**

多态分为两类

* 静态多态: 函数重载 和 运算符重载属于静态多态，复用函数名
* 动态多态: 派生类和虚函数实现运行时多态

静态多态和动态多态区别：

* 静态多态的函数地址早绑定  -  编译阶段确定函数地址
* 动态多态的函数地址晚绑定  -  运行阶段确定函数地址

```cpp
class Animal{
public:
	//Speak函数就是虚函数
	//函数前面加上virtual关键字，变成虚函数，那么编译器在编译的时候就不能确定函数调用了。
	virtual void speak(){
		cout << "动物在说话" << endl;
	}
};

class Cat :public Animal{
public:
	void speak(){
		cout << "小猫在说话" << endl;
	}
};

class Dog :public Animal{
public:

	void speak(){
		cout << "小狗在说话" << endl;
	}

};
//我们希望传入什么对象，那么就调用什么对象的函数
//如果函数地址在编译阶段就能确定，那么静态联编
//如果函数地址在运行阶段才能确定，就是动态联编

void DoSpeak(Animal & animal) {
	animal.speak();
}
//
//多态满足条件： 
//1、有继承关系
//2、子类重写父类中的虚函数
//多态使用：
//父类指针或引用指向子类对象

void test01(){
	Cat cat;
    //Animal & animal = cat; 父类的引用接收子类的对象
    //C++ 允许父类和子类之间进行类型转换，此处是父类的引用指向子类的对象
	DoSpeak(cat);

	Dog dog;
	DoSpeak(dog);
}


int main() {

	test01();

	system("pause");
	return 0;
}
```

总结：

多态满足条件

* 有继承关系
* 子类重写父类中的虚函数

多态使用条件

* 父类指针或引用指向子类对象

重写：函数返回值类型  函数名 参数列表 完全一致称为重写

#### 2. 纯虚函数和抽象类

在多态中，通常父类中虚函数的实现是毫无意义的，主要都是调用子类重写的内容

因此可以将虚函数改为**纯虚函数**

纯虚函数语法：`virtual 返回值类型 函数名 （参数列表）= 0 ;`

当类中有了纯虚函数，这个类也称为==抽象类==

**抽象类特点**：

 * 无法实例化对象
 * 子类必须重写抽象类中的纯虚函数，否则也属于抽象类

```cpp
class Base{
public:
	//纯虚函数
	//类中只要有一个纯虚函数就称为抽象类
	//抽象类无法实例化对象
	//子类必须重写父类中的纯虚函数，否则也属于抽象类
	virtual void func() = 0;
};

class Son :public Base{
public:
	virtual void func() {
		cout << "func调用" << endl;
	};
};

void test01(){
	Base * base = NULL;
	//base = new Base; // 错误，抽象类无法实例化对象
	base = new Son;
	base->func();
	delete base;//记得销毁
}

int main() {

	test01();

	system("pause");
	return 0;
}
```
#### 3. 虚析构和纯虚析构

多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码

解决方式：将父类中的析构函数改为**虚析构**或者**纯虚析构**

虚析构和纯虚析构共性：

* 可以解决父类指针释放子类对象
* 都需要有具体的函数实现

虚析构和纯虚析构区别：

* 如果是纯虚析构，该类属于抽象类，无法实例化对象

虚析构语法：

`virtual ~类名(){}`

纯虚析构语法：

` virtual ~类名() = 0;`

`类名::~类名(){}`

```cpp
class Animal {
public:
	Animal(){
		cout << "Animal 构造函数调用！" << endl;
	}
	virtual void Speak() = 0;

	//析构函数加上virtual关键字，变成虚析构函数
	//virtual ~Animal()
	//{
	//	cout << "Animal虚析构函数调用！" << endl;
	//}

	virtual ~Animal() = 0;
};

Animal::~Animal(){
	cout << "Animal 纯虚析构函数调用！" << endl;
}

//和包含普通纯虚函数的类一样，包含了纯虚析构函数的类也是一个抽象类。不能够被实例化。

class Cat : public Animal {
public:
	Cat(string name){
		cout << "Cat构造函数调用！" << endl;
		m_Name = new string(name);
	}
	virtual void Speak(){
		cout << *m_Name <<  "小猫在说话!" << endl;
	}
	~Cat(){
		cout << "Cat析构函数调用!" << endl;
		if (this->m_Name != NULL) {
			delete m_Name;
			m_Name = NULL;
		}
	}

public:
	string *m_Name;
};

void test01(){
	Animal *animal = new Cat("Tom");
	animal->Speak();

	//通过父类指针去释放，会导致子类对象可能清理不干净，造成内存泄漏
	//怎么解决？给基类增加一个虚析构函数
	//虚析构函数就是用来解决通过父类指针释放子类对象
	delete animal;
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

总结：

​	1. 虚析构或纯虚析构就是用来解决通过父类指针释放子类对象

​	2. 如果子类中没有堆区数据，可以不写为虚析构或纯虚析构

​	3. 拥有纯虚析构函数的类也属于抽象类

### 26.对象的初始化和清理

*  生活中我们买的电子产品都基本会有出厂设置，在某一天我们不用时候也会删除一些自己信息数据保证安全
*  C++中的面向对象来源于生活，每个对象也都会有初始设置以及 对象销毁前的清理数据的设置。

#### 1. 构造函数和析构函数

对象的**初始化和清理**也是两个非常重要的安全问题

​	一个对象或者变量没有初始状态，对其使用后果是未知

​	同样的使用完一个对象或变量，没有及时清理，也会造成一定的安全问题

c++利用了**构造函数**和**析构函数**解决上述问题，这两个函数将会被编译器自动调用，完成对象初始化和清理工作。

对象的初始化和清理工作是编译器强制要我们做的事情，因此如果**我们不提供构造和析构，编译器会提供**

**编译器提供的构造函数和析构函数是空实现。**

* 构造函数：主要作用在于创建对象时为对象的成员属性赋值，构造函数由编译器自动调用，无须手动调用。
* 析构函数：主要作用在于对象**销毁前**系统自动调用，执行一些清理工作。

**构造函数语法：**`类名(){}`

1. 构造函数，没有返回值也不写void
2. 函数名称与类名相同
3. 构造函数可以有参数，因此可以发生重载
4. 程序在调用对象时候会自动调用构造，无须手动调用,而且只会调用一次

**析构函数语法：** `~类名(){}`

1. 析构函数，没有返回值也不写void
2. 函数名称与类名相同,在名称前加上符号  ~
3. 析构函数不可以有参数，因此不可以发生重载
4. 程序在对象销毁前会自动调用析构，无须手动调用,而且只会调用一次

```cpp
class Person {
    
public:
	//构造函数
	Person(){
		cout << "Person的构造函数调用" << endl;
	}
	//析构函数
	~Person(){
		cout << "Person的析构函数调用" << endl;
	}
};

void test01(){
	Person p;
}

int main() {
	
	test01();

	system("pause");
	return 0;
}
```
#### 2. 构造函数的分类及调用

两种分类方式：

​	按参数分为： 有参构造和无参构造

​	按类型分为： 普通构造和拷贝构造

三种调用方式：

​	括号法

​	显示法

​	隐式转换法

```cpp
//1、构造函数分类
// 按照参数分类分为 有参和无参构造   无参又称为默认构造函数
// 按照类型分类分为 普通构造和拷贝构造

class Person {
public:
	//无参（默认）构造函数
	Person() {
		cout << "无参构造函数!" << endl;
	}
	//有参构造函数
	Person(int a) {
		age = a;
		cout << "有参构造函数!" << endl;
	}
	//拷贝构造函数
	Person(const Person& p) {
		age = p.age;
		cout << "拷贝构造函数!" << endl;
	}
	//析构函数
	~Person() {
		cout << "析构函数!" << endl;
	}
public:
	int age;
};

//2、构造函数的调用
//调用无参构造函数
void test01() {
	Person p; //调用无参构造函数
}

//调用有参的构造函数
void test02() {

	//2.1  括号法，常用
	Person p1(10);
	//注意1：调用无参构造函数不能加括号，如果加了编译器认为这是一个函数声明
	//Person p2();

	//2.2 显式法
	Person p2 = Person(10); 
	Person p3 = Person(p2);
	//Person(10)单独写就是匿名对象  当前行结束之后，马上析构

	//2.3 隐式转换法
	Person p4 = 10; // Person p4 = Person(10); 
	Person p5 = p4; // Person p5 = Person(p4); 

	//注意2：不能利用 拷贝构造函数 初始化匿名对象 编译器认为是对象声明
	//Person p5(p4);
}

int main() {

	test01();
	//test02();

	system("pause");
	return 0;
}
```



#### 3. 拷贝构造函数调用时机

C++中拷贝构造函数调用时机通常有三种情况

* 使用一个已经创建完毕的对象来初始化一个新对象
* 值传递的方式给函数参数传值
* 以值方式返回局部对象

```cpp
class Person {
public:
	Person() {
		cout << "无参构造函数!" << endl;
		mAge = 0;
	}
	Person(int age) {
		cout << "有参构造函数!" << endl;
		mAge = age;
	}
	Person(const Person& p) {
		cout << "拷贝构造函数!" << endl;
		mAge = p.mAge;
	}
	//析构函数在释放内存之前调用
	~Person() {
		cout << "析构函数!" << endl;
	}
public:
	int mAge;
};

//1. 使用一个已经创建完毕的对象来初始化一个新对象
void test01() {

	Person man(100); //p对象已经创建完毕
	Person newman(man); //调用拷贝构造函数
	Person newman2 = man; //拷贝构造

	//Person newman3;
	//newman3 = man; //不是调用拷贝构造函数，赋值操作
}

//2. 值传递的方式给函数参数传值
//相当于Person p1 = p;
void doWork(Person p1) {}
void test02() {
	Person p; //无参构造函数
	doWork(p);
}

//3. 以值方式返回局部对象
Person doWork2() {
	Person p1;
	cout << (int *)&p1 << endl;
	return p1;
}

void test03() {
	Person p = doWork2();
	cout << (int *)&p << endl;
}

int main() {

	//test01();
	//test02();
	test03();

	system("pause");
	return 0;
}
```
#### 4. 构造函数调用规则

默认情况下，c++编译器至少给一个类添加3个函数

1．默认构造函数(无参，函数体为空)

2．默认析构函数(无参，函数体为空)

3．默认拷贝构造函数，对属性进行值拷贝

构造函数调用规则如下：

* 如果用户定义有参构造函数，c++不在提供默认无参构造，但是会提供默认拷贝构造


* 如果用户定义拷贝构造函数，c++不会再提供其他构造函数

```cpp
class Person {
public:
	//无参（默认）构造函数
	Person() {
		cout << "无参构造函数!" << endl;
	}
	//有参构造函数
	Person(int a) {
		age = a;
		cout << "有参构造函数!" << endl;
	}
	//拷贝构造函数
	Person(const Person& p) {
		age = p.age;
		cout << "拷贝构造函数!" << endl;
	}
	//析构函数
	~Person() {
		cout << "析构函数!" << endl;
	}
public:
	int age;
};

void test01()
{
	Person p1(18);
	//如果不写拷贝构造，编译器会自动添加拷贝构造，并且做浅拷贝操作
	Person p2(p1);

	cout << "p2的年龄为： " << p2.age << endl;
}

void test02()
{
	//如果用户提供有参构造，编译器不会提供默认构造，会提供拷贝构造
	Person p1; //此时如果用户自己没有提供默认构造，会出错
	Person p2(10); //用户提供的有参
	Person p3(p2); //此时如果用户没有提供拷贝构造，编译器会提供

	//如果用户提供拷贝构造，编译器不会提供其他构造函数
	Person p4; //此时如果用户自己没有提供默认构造，会出错
	Person p5(10); //此时如果用户自己没有提供有参，会出错
	Person p6(p5); //用户自己提供拷贝构造
}

int main() {

	test01();

	system("pause");
	return 0;
}
```
#### 5. 深拷贝与浅拷贝

深浅拷贝是面试经典问题，也是常见的一个坑

浅拷贝：简单的赋值拷贝操作

深拷贝：在堆区重新申请空间，进行拷贝操作

```cpp
class Person {
public:
	//无参（默认）构造函数
	Person() {
		cout << "无参构造函数!" << endl;
	}
	//有参构造函数
	Person(int age ,int height) {
		cout << "有参构造函数!" << endl;

		m_age = age;
		m_height = new int(height);
		
	}
	//拷贝构造函数  
	Person(const Person& p) {
		cout << "拷贝构造函数!" << endl;
		//如果不利用深拷贝在堆区创建新内存，会导致浅拷贝带来的重复释放堆区问题
		m_age = p.m_age;
		m_height = new int(*p.m_height);
	}

	//析构函数
	~Person() {
		cout << "析构函数!" << endl;
		if (m_height != NULL)
		{
			delete m_height;
		}
	}
public:
	int m_age;
	int* m_height;
};

void test01() {
	Person p1(18, 180);
	Person p2(p1);

	cout << "p1的年龄： " << p1.m_age << " 身高： " << *p1.m_height << endl;
	cout << "p2的年龄： " << p2.m_age << " 身高： " << *p2.m_height << endl;
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

> 总结：如果属性有在堆区开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来的问题
#### 6. 初始化列表

**作用：** C++提供了初始化列表语法，用来初始化属性

**语法：**`构造函数()：属性1(值1),属性2（值2）... {}`

```cpp
class Person {
public:
	//传统方式初始化
	/*Person(int a, int b, int c) {
		m_A = a;
		m_B = b;
		m_C = c;
	}*/

	//初始化列表方式初始化
	Person(int a, int b, int c) :m_A(a), m_B(b), m_C(c) {}
	void PrintPerson() {
		cout << "mA:" << m_A << endl;
		cout << "mB:" << m_B << endl;
		cout << "mC:" << m_C << endl;
	}
private:
	int m_A;
	int m_B;
	int m_C;
};

int main() {

	Person p(1, 2, 3);
	p.PrintPerson();

	system("pause");
	return 0;
}
```
#### 7. 类对象作为类成员

C++类中的成员可以是另一个类的对象，我们称该成员为 对象成员

```cpp
class A {}
class B {
    A a；
}
```

B类中有对象A作为成员，A为对象成员

那么当创建B对象时，A与B的构造和析构的顺序是谁先谁后？

```cpp
class Phone{
public:
	Phone(string name){
		m_PhoneName = name;
		cout << "Phone构造" << endl;
	}

	~Phone(){
		cout << "Phone析构" << endl;
	}

	string m_PhoneName;

};

class Person{
public:

	//初始化列表可以告诉编译器调用哪一个构造函数
	Person(string name, string pName) :m_Name(name), m_Phone(pName){
		cout << "Person构造" << endl;
	}

	~Person(){
		cout << "Person析构" << endl;
	}

	void playGame(){
		cout << m_Name << " 使用" << m_Phone.m_PhoneName << " 牌手机! " << endl;
	}

	string m_Name;
	Phone m_Phone;
};
void test01(){
	//当类中成员是其他类对象时，我们称该成员为 对象成员
	//构造的顺序是 ：先调用对象成员的构造，再调用本类构造
	//析构顺序与构造相反
	Person p("张三" , "苹果X");
	p.playGame();
}

int main() {

	test01();

	system("pause");
	return 0;
}
```
#### 8. 静态成员

静态成员就是在成员变量和成员函数前加上关键字static，称为静态成员

静态成员分为：

*  静态成员变量
   *  所有对象共享同一份数据
   *  在编译阶段分配内存
   *  类内声明，类外初始化
*  静态成员函数
   *  所有对象共享同一个函数
   *  静态成员函数只能访问静态成员变量

```cpp
class Person{
	
public:
	static int m_A; //静态成员变量

	//静态成员变量特点：
	//1 在编译阶段分配内存
	//2 类内声明，类外初始化
	//3 所有对象共享同一份数据

private:
	static int m_B; //静态成员变量也是有访问权限的
};
int Person::m_A = 10;
int Person::m_B = 10;

void test01(){
	//静态成员变量两种访问方式

	//1、通过对象
	Person p1;
	p1.m_A = 100;
	cout << "p1.m_A = " << p1.m_A << endl;

	Person p2;
	p2.m_A = 200;
	cout << "p1.m_A = " << p1.m_A << endl; //共享同一份数据
	cout << "p2.m_A = " << p2.m_A << endl;

	//2、通过类名
	cout << "m_A = " << Person::m_A << endl;

	//cout << "m_B = " << Person::m_B << endl; //私有权限访问不到
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

```cpp
class Person{

public:
	//静态成员函数特点：
	//1 程序共享一个函数
	//2 静态成员函数只能访问静态成员变量
	
	static void func()
	{
		cout << "func调用" << endl;
		m_A = 100;
		//m_B = 100; //错误，不可以访问非静态成员变量
	}

	static int m_A; //静态成员变量
	int m_B; // 
    
private:
	//静态成员函数也是有访问权限的
	static void func2(){
		cout << "func2调用" << endl;
	}
};
int Person::m_A = 10;

void test01() {
	//静态成员变量两种访问方式

	//1、通过对象
	Person p1;
	p1.func();

	//2、通过类名
	Person::func();

	//Person::func2(); //私有权限访问不到
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

### 27. C++对象模型和this指针
#### 1. 成员变量和成员函数分开存储

在C++中，类内的成员变量和成员函数分开存储

只有非静态成员变量才属于类的对象上

```cpp
class Person {
public:
	Person() {
		mA = 0;
	}
	//非静态成员变量占对象空间
	int mA;
	//静态成员变量不占对象空间
	static int mB; 
	//函数也不占对象空间，所有函数共享一个函数实例
	void func() {
		cout << "mA:" << this->mA << endl;
	}
	//静态成员函数也不占对象空间
	static void sfunc() {
	}
};

int main() {

	cout << sizeof(Person) << endl;

	system("pause");
	return 0;
}
```



#### 2. this指针概念

通过1我们知道在C++中成员变量和成员函数是分开存储的

每一个非静态成员函数只会诞生一份函数实例，也就是说多个同类型的对象会共用一块代码

那么问题是：这一块代码是如何区分那个对象调用自己的呢？

c++通过提供特殊的对象指针，this指针，解决上述问题。**this指针指向被调用的成员函数所属的对象**

this指针是隐含每一个非静态成员函数内的一种指针

this指针不需要定义，直接使用即可

this指针的用途：

*  当形参和成员变量同名时，可用this指针来区分
*  在类的非静态成员函数中返回对象本身，可使用return *this

```cpp
class Person{
public:
	Person(int age){
		//1、当形参和成员变量同名时，可用this指针来区分
		this->age = age;
	}

	Person& PersonAddPerson(Person p){
		this->age += p.age;
		//返回对象本身
		return *this;
	}

	int age;
};

void test01(){
	Person p1(10);
	cout << "p1.age = " << p1.age << endl;

	Person p2(10);
	p2.PersonAddPerson(p1).PersonAddPerson(p1).PersonAddPerson(p1);
	cout << "p2.age = " << p2.age << endl;
}

int main() {

	test01();

	system("pause");
	return 0;
}
```
#### 3. 空指针访问成员函数

C++中空指针也是可以调用成员函数的，但是也要注意有没有用到this指针

如果用到this指针，需要加以判断保证代码的健壮性

```cpp
//空指针访问成员函数
class Person {
public:
	void ShowClassName() {
		cout << "我是Person类!" << endl;
	}

	void ShowPerson() {
		if (this == NULL) {
			return;
		}
		cout << mAge << endl;
	}

public:
	int mAge;
};

void test01()
{
	Person * p = NULL;
	p->ShowClassName(); //空指针，可以调用成员函数
	p->ShowPerson();  //但是如果成员函数中用到了this指针，就不可以了
}

int main() {

	test01();

	system("pause");
	return 0;
}
```
#### 4. const修饰成员函数

**常函数：**

* 成员函数后加const后我们称为这个函数为**常函数**
* 常函数内不可以修改成员属性
* 成员属性声明时加关键字mutable后，在常函数中依然可以修改

**常对象：**

* 声明对象前加const称该对象为常对象
* 常对象只能调用常函数

```cpp
class Person {
public:
	Person() {
		m_A = 0;
		m_B = 0;
	}

	//this指针的本质是一个指针常量，指针的指向不可修改
	//如果想让指针指向的值也不可以修改，需要声明常函数
	void ShowPerson() const {
		//const Type* const pointer;
		//this = NULL; //不能修改指针的指向 Person* const this;
		//this->mA = 100; //但是this指针指向的对象的数据是可以修改的

		//const修饰成员函数，表示指针指向的内存空间的数据不能修改，除了mutable修饰的变量
		this->m_B = 100;
	}

	void MyFunc() const {
		//mA = 10000;
	}

public:
	int m_A;
	mutable int m_B; //可修改 可变的
};

//const修饰对象  常对象
void test01() {

	const Person person; //常量对象  
	cout << person.m_A << endl;
	//person.mA = 100; //常对象不能修改成员变量的值,但是可以访问
	person.m_B = 100; //但是常对象可以修改mutable修饰成员变量

	//常对象访问成员函数
	person.MyFunc(); //常对象不能调用const的函数

}

int main() {

	test01();

	system("pause");
	return 0;
}
```
### 28. 友元

生活中你的家有客厅(Public)，有你的卧室(Private)

客厅所有来的客人都可以进去，但是你的卧室是私有的，也就是说只有你能进去

但是呢，你也可以允许你的好闺蜜好基友进去。

在程序里，有些私有属性 也想让类外特殊的一些函数或者类进行访问，就需要用到友元的技术

友元的目的就是让一个函数或者类 访问另一个类中私有成员

友元的关键字为  ==friend==

友元的三种实现

* 全局函数做友元
* 类做友元
* 成员函数做友元

#### 1. 全局函数做友元

```cpp
class Building{
	//告诉编译器 goodGay全局函数 是 Building类的好朋友，可以访问类中的私有内容
	friend void goodGay(Building * building);

public:
	Building(){
		this->m_SittingRoom = "客厅";
		this->m_BedRoom = "卧室";
	}

public:
	string m_SittingRoom; //客厅

private:
	string m_BedRoom; //卧室
};


void goodGay(Building * building){
	cout << "好基友正在访问： " << building->m_SittingRoom << endl;
	cout << "好基友正在访问： " << building->m_BedRoom << endl;
}


void test01(){
	Building b;
	goodGay(&b);
}

int main(){

	test01();

	system("pause");
	return 0;
}
```



#### 2. 类做友元

```cpp
class Building;
class goodGay{
public:

	goodGay();
	void visit();

private:
	Building *building;
};


class Building{
	//告诉编译器 goodGay类是Building类的好朋友，可以访问到Building类中私有内容
	friend class goodGay;

public:
	Building();

public:
	string m_SittingRoom; //客厅
private:
	string m_BedRoom;//卧室
};

Building::Building(){
	this->m_SittingRoom = "客厅";
	this->m_BedRoom = "卧室";
}

goodGay::goodGay(){
	building = new Building;
}

void goodGay::visit(){
	cout << "好基友正在访问" << building->m_SittingRoom << endl;
	cout << "好基友正在访问" << building->m_BedRoom << endl;
}

void test01(){
	goodGay gg;
	gg.visit();

}

int main(){

	test01();

	system("pause");
	return 0;
}
```
#### 3. 成员函数做友元

```cpp
class Building;
class goodGay{
public:

	goodGay();
	void visit(); //只让visit函数作为Building的好朋友，可以发访问Building中私有内容
	void visit2(); 

private:
	Building *building;
};


class Building{
	//告诉编译器  goodGay类中的visit成员函数 是Building好朋友，可以访问私有内容
	friend void goodGay::visit();

public:
	Building();

public:
	string m_SittingRoom; //客厅
private:
	string m_BedRoom;//卧室
};

Building::Building()
{
	this->m_SittingRoom = "客厅";
	this->m_BedRoom = "卧室";
}

goodGay::goodGay(){
	building = new Building;
}

void goodGay::visit(){
	cout << "好基友正在访问" << building->m_SittingRoom << endl;
	cout << "好基友正在访问" << building->m_BedRoom << endl;
}

void goodGay::visit2(){
	cout << "好基友正在访问" << building->m_SittingRoom << endl;
	//cout << "好基友正在访问" << building->m_BedRoom << endl;
}

void test01(){
	goodGay  gg;
	gg.visit();

}

int main(){
    
	test01();

	system("pause");
	return 0;
}
```

### 29. 运算符重载

运算符重载概念：对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

#### 1. 加号运算符重载

- 作用：实现两个自定义数据类型相加的运算

```cpp
class Person {
public:
	Person() {};
	Person(int a, int b)
	{
		this->m_A = a;
		this->m_B = b;
	}
	//成员函数实现 + 号运算符重载
	Person operator+(const Person& p) {
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;
		return temp;
	}


public:
	int m_A;
	int m_B;
};

//全局函数实现 + 号运算符重载
/*Person operator+(const Person& p1, const Person& p2) {
	Person temp(0, 0);
	temp.m_A = p1.m_A + p2.m_A;
	temp.m_B = p1.m_B + p2.m_B;
	return temp;
}*/

//运算符重载 可以发生函数重载 
Person operator+(const Person& p2, int val)  {
	Person temp;
	temp.m_A = p2.m_A + val;
	temp.m_B = p2.m_B + val;
	return temp;
}

void test() {

	Person p1(10, 10);
	Person p2(20, 20);

	//成员函数方式
	Person p3 = p2 + p1;  //相当于 p2.operaor+(p1)
	cout << "mA:" << p3.m_A << " mB:" << p3.m_B << endl;


	Person p4 = p3 + 10; //相当于 operator+(p3,10)
	cout << "mA:" << p4.m_A << " mB:" << p4.m_B << endl;

}

int main() {

	test();

	system("pause");
	return 0;
}
```

> 总结1：对于内置的数据类型的表达式的的运算符是不可能改变的

> 总结2：不要滥用运算符重载



#### 2. 左移运算符重载

作用：可以输出自定义数据类型

```cpp
class Person {
	friend ostream& operator<<(ostream& out, Person& p);

public:
	Person(int a, int b){
		this->m_A = a;
		this->m_B = b;
	}

	//成员函数 实现不了  p << cout 不是我们想要的效果
	//void operator<<(Person& p){
	//}

private:
	int m_A;
	int m_B;
};

//全局函数实现左移重载
//ostream对象只能有一个
ostream& operator<<(ostream& out, Person& p) {
	out << "a:" << p.m_A << " b:" << p.m_B;
	return out;
}

void test() {
	Person p1(10, 20);
	cout << p1 << "hello world" << endl; //链式编程
}

int main() {

	test();

	system("pause");
	return 0;
}
```

> 总结：重载左移运算符配合友元可以实现输出自定义数据类型



#### 3. 递增运算符重载

作用： 通过重载递增运算符，实现自己的整型数据

```cpp
class MyInteger {

	friend ostream& operator<<(ostream& out, MyInteger myint);

public:
	MyInteger() {
		m_Num = 0;
	}
	//前置++
	MyInteger& operator++() {
		//先++
		m_Num++;
		//再返回
		return *this;
	}

	//后置++
	MyInteger operator++(int) {
		//先返回
		MyInteger temp = *this;
        //记录当前本身的值，然后让本身的值加1，但是返回的是以前的值，达到先返回后++；
		m_Num++;
		return temp;
	}

private:
	int m_Num;
};


ostream& operator<<(ostream& out, MyInteger myint) {
	out << myint.m_Num;
	return out;
}


//前置++ 先++ 再返回
void test01() {
	MyInteger myInt;
	cout << ++myInt << endl;
	cout << myInt << endl;
}

//后置++ 先返回 再++
void test02() {

	MyInteger myInt;
	cout << myInt++ << endl;
	cout << myInt << endl;
}

int main() {

	test01();
	//test02();

	system("pause");
	return 0;
}
```

> 总结： 前置递增返回引用，后置递增返回值
#### 4. 赋值运算符重载

c++编译器至少给一个类添加4个函数

1. 默认构造函数(无参，函数体为空)
2. 默认析构函数(无参，函数体为空)
3. 默认拷贝构造函数，对属性进行值拷贝
4. 赋值运算符 operator=, 对属性进行值拷贝

如果类中有属性指向堆区，做赋值操作时也会出现深浅拷贝问题

```cpp
class Person{
public:

	Person(int age){
		//将年龄数据开辟到堆区
		m_Age = new int(age);
	}

	//重载赋值运算符 
	Person& operator=(Person &p){
		if (m_Age != NULL){
			delete m_Age;
			m_Age = NULL;
		}
		//编译器提供的代码是浅拷贝
		//m_Age = p.m_Age;

		//提供深拷贝 解决浅拷贝的问题
		m_Age = new int(*p.m_Age);

		//返回自身
		return *this;
	}


	~Person(){
		if (m_Age != NULL){
			delete m_Age;
			m_Age = NULL;
		}
	}

	//年龄的指针
	int *m_Age;
};


void test01(){
	Person p1(18);
	Person p2(20);
	Person p3(30);

	p3 = p2 = p1; //赋值操作

	cout << "p1的年龄为：" << *p1.m_Age << endl;
	cout << "p2的年龄为：" << *p2.m_Age << endl;
	cout << "p3的年龄为：" << *p3.m_Age << endl;
}

int main() {

	test01();

	//int a = 10;
	//int b = 20;
	//int c = 30;

	//c = b = a;
	//cout << "a = " << a << endl;
	//cout << "b = " << b << endl;
	//cout << "c = " << c << endl;

	system("pause");
	return 0;
}
```
#### 5. 关系运算符重载

**作用：** 重载关系运算符，可以让两个自定义类型对象进行对比操作

```cpp
class Person{
public:
	Person(string name, int age){
		this->m_Name = name;
		this->m_Age = age;
	};

	bool operator==(Person & p){
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age){
			return true;
		}else{
			return false;
		}
	}

	bool operator!=(Person & p){
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age){
			return false;
		}else{
			return true;
		}
	}

	string m_Name;
	int m_Age;
};

void test01(){
	//int a = 0;
	//int b = 0;

	Person a("孙悟空", 18);
	Person b("孙悟空", 18);

	if (a == b){
		cout << "a和b相等" << endl;
	}else{
		cout << "a和b不相等" << endl;
	}

	if (a != b){
		cout << "a和b不相等" << endl;
	}else{
		cout << "a和b相等" << endl;
	}
}

int main() {

	test01();

	system("pause");
	return 0;
}
```
#### 6. 函数调用运算符重载

* 函数调用运算符 ()  也可以重载
* 由于重载后使用的方式非常像函数的调用，因此称为仿函数
* 仿函数没有固定写法，非常灵活

```cpp
class MyPrint{
public:
	void operator()(string text){
		cout << text << endl;
	}

};
void test01(){
	//重载的（）操作符 也称为仿函数
	MyPrint myFunc;
	myFunc("hello world");
}


class MyAdd{
public:
	int operator()(int v1, int v2){
		return v1 + v2;
	}
};

void test02(){
	MyAdd add;
	int ret = add(10, 10);
	cout << "ret = " << ret << endl;

	//匿名对象调用  
	cout << "MyAdd()(100,100) = " << MyAdd()(100, 100) << endl;
}

int main() {

	test01();
	test02();

	system("pause");
	return 0;
}
```
### 30.文件操作

C++中对文件操作需要包含头文件 ==&lt; fstream &gt;==

文件类型分为两种：

1. **文本文件**     -  文件以文本的**ASCII码**形式存储在计算机中
2. **二进制文件** -  文件以文本的**二进制**形式存储在计算机中，用户一般不能直接读懂它们

操作文件的三大类:

1. ofstream：写操作
2. ifstream： 读操作
3. fstream ： 读写操作
#### 1. 文本文件
##### 1. 写文件

   写文件步骤如下：

1. 包含头文件   

   \#include <fstream\>

2. 创建流对象  

   ofstream ofs;

3. 打开文件

   ofs.open("文件路径",打开方式);

4. 写数据

   ofs << "写入的数据";

5. 关闭文件

   ofs.close();


文件打开方式：

| 打开方式    | 解释                       |
| ----------- | -------------------------- |
| ios::in     | 为读文件而打开文件         |
| ios::out    | 为写文件而打开文件         |
| ios::ate    | 初始位置：文件尾           |
| ios::app    | 追加方式写文件             |
| ios::trunc  | 如果文件存在先删除，再创建 |
| ios::binary | 二进制方式                 |

**注意：** 文件打开方式可以配合使用，利用|操作符

**示例：**

```cpp
#include <fstream>

void test01(){
	ofstream ofs;
	ofs.open("test.txt", ios::out);

	ofs << "姓名：张三" << endl;
	ofs << "性别：男" << endl;
	ofs << "年龄：18" << endl;

	ofs.close();
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

总结：

* 文件操作必须包含头文件 fstream
* 读文件可以利用 ofstream  ，或者fstream类
* 打开文件时候需要指定操作文件的路径，以及打开方式
* 利用<<可以向文件中写数据
* 操作完毕，要关闭文件

##### 2.读文件

读文件与写文件步骤相似，但是读取方式相对于比较多

读文件步骤如下：

1. 包含头文件   

   \#include <fstream\>

2. 创建流对象  

   ifstream ifs;

3. 打开文件并判断文件是否打开成功

   ifs.open("文件路径",打开方式);

4. 读数据

   四种方式读取

5. 关闭文件

   ifs.close();

```cpp
#include <fstream>
#include <string>
void test01(){
	ifstream ifs;
	ifs.open("test.txt", ios::in);

	if (!ifs.is_open()){
		cout << "文件打开失败" << endl;
		return;
	}

	//第一种方式
	//char buf[1024] = { 0 };
	//while (ifs >> buf)
	//{
	//	cout << buf << endl;
	//}

	//第二种
	//char buf[1024] = { 0 };
	//while (ifs.getline(buf,sizeof(buf)))
	//{
	//	cout << buf << endl;
	//}

	//第三种
	//string buf;
	//while (getline(ifs, buf))
	//{
	//	cout << buf << endl;
	//}

	char c;
	while ((c = ifs.get()) != EOF){
		cout << c;
	}

	ifs.close();
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

总结：

- 读文件可以利用 ifstream  ，或者fstream类
- 利用is_open函数可以判断文件是否打开成功
- close 关闭文件 

#### 2. 二进制文件

以二进制的方式对文件进行读写操作

打开方式要指定为 ==ios::binary==

##### 1. 写文件

二进制方式写文件主要利用流对象调用成员函数write

函数原型 ：`ostream& write(const char * buffer,int len);`

参数解释：字符指针buffer指向内存中一段存储空间。len是读写的字节数

```C++
#include <fstream>
#include <string>

class Person{
public:
	char m_Name[64];
	int m_Age;
};

//二进制文件  写文件
void test01(){
	//1、包含头文件

	//2、创建输出流对象
	ofstream ofs("person.txt", ios::out | ios::binary);
	
	//3、打开文件
	//ofs.open("person.txt", ios::out | ios::binary);

	Person p = {"张三"  , 18};

	//4、写文件
	ofs.write((const char *)&p, sizeof(p));

	//5、关闭文件
	ofs.close();
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

总结：

* 文件输出流对象 可以通过write函数，以二进制方式写数据
##### 2. 读文件

二进制方式读文件主要利用流对象调用成员函数read

函数原型：`istream& read(char *buffer,int len);`

参数解释：字符指针buffer指向内存中一段存储空间。len是读写的字节数

```C++
#include <fstream>
#include <string>

class Person{
public:
	char m_Name[64];
	int m_Age;
};

void test01(){
	ifstream ifs("person.txt", ios::in | ios::binary);
	if (!ifs.is_open()){
		cout << "文件打开失败" << endl;
	}

	Person p;
	ifs.read((char *)&p, sizeof(p));

	cout << "姓名： " << p.m_Name << " 年龄： " << p.m_Age << endl;
}

int main() {

	test01();

	system("pause");
	return 0;
}
```

- 文件输入流对象 可以通过read函数，以二进制方式读数据

### 31. 指针与引用详解
#### 1. 变量

-  变量的类型，就是变量中数据的类型。
-  变量在定义(创建)时,必须先**指定它的类型**。
-  1个变量只有1个类型，而且不能改成其他类型。

#### 2. 变量的定义

- 变量的定义用于为变量**分配存储空间**，还可以为变量指定**初始值**。
- 提供了一个实体在程序中的唯一描述。
- 在一个程序中，变量有且仅有一个定义。

```cpp
// 同时定义多个变量。
int a = 1, b = 3;
```
#### 3. 变量的声明

- 将一个符号引入到一个作用域。
- 程序中变量可以声明多次。
- 在类中的成员函数和静态数据成员不能多次声明。

#### 4. 声明和定义的区别

- **定义即是声明**，因为定义变量时我们也向程序表明了它的类型和名字。
- **声明不一定是定义**，可以通过使用extern关键字声明变量而不是定义它。

```cpp
//定义：
int a；
extern int i=0；

// 声明不定义
//1.仅仅提供函数原型。
void func(int,int);
// 2.extern无赋值
extern int a; 
// 3.类名
class A; 
// 4.typedef声明
// 5.在类中定义的静态数据成员
class A{
    public:
    static int a;//声明。
};
```

#### 5. 变量的地址

- **&操作**：`&变量名`用于获取变量的地址。

#### 6、变量的指针

- `对象类型*`表示指针类型变量。对象类型和*之间可以有别的元素。

```cpp
// 同时定义变量和变量的指针
int a, *p = &a;
```

- 指针p存储的始终是a的地址，而不是*p存了a的地址。

- **\*操作**：`*指针变量名`用于获取指针的值。是&的相反操作。

```cpp
int a, b, *p = &a;
// 注意区别
*p = 200 // 改变 a（即*p）的值。
p = &b // 改变指针 p 的指向。
```

#### 7. 数组名

- 数组名类似于一个**指针常量**，其指向不能改变，只能一直指向同一块内存。
- 二维数组名类似与指针的指针。

```cpp
int a[10];
int* p = a;
// 等同于
int* q= &a[0];

p+1 == a+1; a+1 == q+1;
```

**注意**：

- `sizeof(a)`表示数组的大小（字节数）；

- `&a`表示指向整个数组存储区的指针，而不是&(&a[0])。

- 指针可以改变指向，但数组名不能改变指向。即可以p++，但不可以a++

- 数组名做函数参数会退化为指针。

#### 8. 指针变量类型

- `int*`类型的指针，单位为4Byte。
- `char*`类型的指针，单位为1Byte。
- `double*`类型的指针，单位为8Byte。

```cpp
// 验证int为4Byte
int a[10] = {1,2,3,4,5,6,7,8,9,10};
int n = 1;
int *p = a;
void *q = &n;
q = &a[0];
cout << *(p+1) << "," << *((int*)(q+4)) << endl;
// 2,2

// 验证int为short的2倍
int a, *p = &a;
short b, *q = &b;
cout << (short*)(p+1)-(short*)p << "," << q+1-q << endl;
// 2,1

// (int*)为强制类型转换
```

#### 9. 多级指针

- 指向指针的指针。用来操纵指针的指向。

```cpp
int a, *p = &a, **q = &p;
```

#### 10. 指针数组

- 指针组成是数组。

```cpp
int a, b, c;
int *p[3] = {&a, &b, &c};
```

#### 11. 数组指针

- 指向一个二维数组的指针。它是一个指针，但可以以数组的方式取出地址。

```cpp
int a[2][3] = {{1, 2, 3}, {4, 5, 6}};
int (*p)[3] = a;

a[1][2] == *(*(a+1)+2)；
```

#### 12. 指针和字符串

- 字符串本质是字符数组。
- 字符串名也和数组名有相同的类指针属性。
- 二维字符数组，同其他的二维数组一样。

```cpp
char* p = "uhbg";
char* q;
cin >> *q;
```

```cpp
char s[3][6] = {"abcde", "efghi", "jklmn"};
```

#### 13. const指针

const默认**修饰其左边**的内容，如果其左边没有内容或者左边的内容已经被修饰过了，则const修饰其右边的内容。

```cpp
const int a;// 修饰int, 等同于
int const a;
// a的类型为 常量int

int const * p;// 修饰int, 等同于
const int * p;
// 即p指向常量int，p指向的变量的值不能改变，但是p可以指向其他变量。

int a;
int * const p = &a; // 修饰*
// 即p指向常量地址，p的指向不能改变，只能指向a，但是指向的变量a的值可以改变。

int const * const p;// 修饰int，且修饰*， 等同于
const int * const p;// 等同于
const int const * p;
```

#### 14. 动态内存分配

- 不使用变量名，直接使用指针来开辟内存空间。
- 指针开辟的内存空间**可以回收**。
- 变量名定义的值存放在**栈**内，new创建的值存放在**堆**内或**自由存储区**。
- 程序运行时new的数组被称为动态数组。
- 动态分配的内存使用完必须删除，不然会一直占用。

```cpp
// 动态分配一个int大小的内存空间。
int *p = new int;
*p = 233;

// 动态分配10个int大小的内存空间。
int *q = new int[10];
q[0] = 0; // 可以使用数组名的方法取值和赋值。
q[1] = 1; // 等同于*(q+1) = 1。

// 动态分配3个int*大小的指针数组
int** r = new int*[3];
r[0] = p;

delete p;
delete[] q;
```

#### 15. 指针作为函数参数

- 指针作为函数参数，可以把变量的内存地址传递到函数内部，这样就可以对地址所在变量进行直接修改。

- 一维数组作为函数形参会退化为指针，二维数组作为形参则不会。

#### 16. 指针作为函数返回值

- 指针作为函数返回值，只是返回值类型是指针，其余和一个普通的函数没什么不同，。

```cpp
int* f(int x) {
    ......
}
```
#### 17. 函数指针

- 函数指针是指向函数的指针。
- 函数指针可以调用函数。
- 函数指针不能改变指向到形式（输入、输出类型）不同的函数。

```cpp
int (*p)(int, int);
int add(int a, int b) {
    return a+b;
}

p = add;

int c = (*p)(2,4);

auto m = p;// 新建指针。
```

#### 18. 引用

- 引用是变量的一个别名。两者所有操作都是等价的，且互相等价影响。

- `对象类型 &变量名 = 变量名`对引用初始化。引用定义时必须初始化。

#### 19. 引用作为函数形参

- 传递引用是传递原变量，不需要做变量拷贝，不需要开辟内存。
- **常量引用**可以避免无意中修改数据的变成错误；
- **常量引用**能够处理const和非const实参，否则只能使用非const数据。
- **常量引用**使函数能够正确生成并使用临时变量。

```cpp
void swap(int &r1, int &r2);

int num1 = 1, num2 = 2;
swap(num1, num2);
cout << num1 << "," << num2 << endl;
// 2,1

//按引用传参
void swap3(int &r1, int &r2) {
    int temp = r1;
    r1 = r2;
    r2 = temp;
}
```

#### 20. 引用作为函数函数返回值

- 函数返回一个引用时，实际上返回了一个指向返回值的隐式指针。
- 不能返回一个局部变量的引用。
- 返回值是一个左值。普通变量名即是左值，可以使用a=b对a赋值。

```cpp
int a[5]={0,1,2,3,4};

int &f(int x){
    return a[x];
}

// 用作左值
f(2) = 43;// 等同于：
a[2] = 43;

// 用作赋值
int n = f(2); // 复制
int& m = f(2); // 不复制
```

#### 21. 引用和指针的区别

1. 指针可以有空指针，但不存在空引用，引用必须连接到一块合法的内存；
2. 指针可以在任何时候指到另一个对象。一旦引用被初始化为一个对象，就不能被指向到另一个对象；
3. 引用必须在创建时被初始化。指针可以在任何时间被初始化；
4. 引用使用时无需解引用，指针需要解引用(即用 *p 访问指向的变量)；
5. 指针和引用的自增++运算意义不一样，引用自增被引用对象的值，指针自增其指向会移动一个单位长度；
6. sizeof 一个引用变量，得到的是所指变量的大小，sizeof 一个指针变量，得到指针的值(即所指变量的地址)的大小。

#### 22. 指针和结构体

```cpp
struct st{
    int num;
    string s;
};

st* p = new st; // 动态分配内存
p->num = 20; // 访问方式
(*p).s = "abc"; // 访问方式
delete p;
```

#### 23. 动态分配字符串内存

```cpp
char* getstr(){
    char temp[100]; // 局部变量使用完会自动消失。
    cout<< "请输入：";
    cin >> temp;
    char* p = new char[strlen(temp)+1]; // new的存储方式为堆。
    strcpy(p, temp);

    return p;
}
```

#### 24. 类型别名

- 简化类型名称。
- 简化函数指针。

```cpp
typedef double real; // 类型别名
real a = 3.14;

const double * f1(const double * ar, int n);
const double * f2(const double ar[], int n);
const double * f3(const double ar[], int n);
typedef const double*(*func)(const double*, int); // 函数类型别名
func p1 = f1; 
func ps[3] = {f1, f2, f3};
```

#### 25. 右值引用

- 左值引用只能引用左值，右值引用只能引用右值。
- 在赋值和函数参数传入时，右值引用可以免去复制的麻烦。
- 右值引用一定是const &，const &不一定是右值引用

```cpp
int a = 7;
string s = "bcdfe";

// 左值引用，只能引用有变量名的左值。
int& a1 = a;
// int& a2 = 7; // error
// int& a3 = a+a1;  // error
string& s1 = s;
// string& s2 = "bcdfe"; // error

// 左值引用，只能引用右值和语句返回的临时值。
// int&& a1 = a; // error
int&& a2 = 7;
int&& a3 = a+a1;  // 运算返回临时值
// string&& s1 = s; // error
string&& s2 = "bcdfe";
int&& m = min(3,5); // 函数返回。
```

#### 26. 转换为右值（移动赋值）

- std::move的作用是把一个左值或右值转换为右值。
- c++11在使用**等号赋值**时，会自动把右值进行移动而不是复制。
- 函数返回的右值（非引用），会自动转化为右值引用。即使用完会自动消失。

```cpp
vector<string> n1, n2;

auto n3 = n1; // 复制
auto n4 = std::move(n2); // 移动。
auto n5 = n1+n2; // 移动。
auto n5 = f(n1); // 如果f返回左值则采用复制，返回右值则采用移动。
```

- 类需要重载赋值运算符，并实现拷贝赋值和移动赋值（右值引用赋值）。

#### 27. 右值引用作为函数参数

- 使用结束会自动销毁。
- 可以避免对象被修改。

```cpp
string rand(const vector<string>& arr);// 左值引用
string rand(vector<string>&& arr); // 右值引用
```

#### 28. 类的构造函数

- 类的构造函数分为**拷贝构造函数**和**移动构造函数**。

```cpp
TheClass::TheClass(const TheClass& rhs); // 拷贝构造函数
TheClass::TheClass(TheClass&& rhs); // 移动构造函数

TheClass a = b;
TheClass a{b};
// 如果b是左值，则调用拷贝构造；如果b是右值，则调用移动构造。
```

#### 29. this指针

- 类的非静态的成员函数中，可以使用this指代类的对象。
- 非静态的成员函数中，使用this和不使用没有区别。当变量名冲突时，需要使用this。
- 全局函数、静态函数都不能使用this。
- 要返回本身或返回本身的引用时，使用this。

```cpp
class Test
{
    public:
        Test(){};
        Test(int val){
            data = val; // 或
            this.data = val;
            f1();// 或
            this.f1();
        }

        void f1();

    private:
        int data;
};

void Test::f1(){
    data = 9; // 或
    this.data = 9;
}

// 如果 成员变量是public的，其子类也可以直接或者使用this访问。
```

### 32. 泛型编程和STL
#### 1.模板

* C++另一种编程思想称为 ==泛型编程== ，主要利用的技术就是模板

* C++提供两种模板机制:**函数模板**和**类模板** 

##### 1. 函数模板
###### 1. 函数模板语法

函数模板作用：建立一个通用函数，其函数返回值类型和形参类型可以不具体制定，用一个**虚拟的类型**来代表。

**语法：** 

```cpp
template<typename T> // 或 template<class T>, T 可以用其他标识符
// 函数声明或定义，如：
T func(T a);
```

```cpp
//利用模板提供通用的交换函数
template<typename T>
void mySwap(T& a, T& b){
	T temp = a;
	a = b;
	b = temp;
}

//利用模板实现交换
//1、自动类型推导
mySwap(a, b);

//2、显示指定类型
mySwap<int>(a, b);
```



###### 2. 函数模板注意事项

注意事项：

* 自动类型推导，必须推导出一致的数据类型T,才可以使用


* 模板必须要确定出T的数据类型，才可以使用

```cpp
// 1、自动类型推导，必须推导出一致的数据类型T,才可以使用
template<class T>
void mySwap(T& a, T& b){
	T temp = a;
	a = b;
	b = temp;
}

int a = 10;
int b = 20;
char c = 'c';

mySwap(a, b); // 正确，可以推导出一致的T
//mySwap(a, c); // 错误，推导不出一致的T类型


// 2、模板必须要确定出T的数据类型，才可以使用
template<class T>
void func(){
	cout << "func 调用" << endl;
}

//func(); //错误，模板不能独立使用，必须确定出T的类型
func<int>(); //利用显示指定类型的方式，给T一个类型，才可以使用该模板
```



###### 3.  普通函数与函数模板的调用规则

调用规则如下：

1. 如果函数模板和普通函数都可以实现，优先调用普通函数
2. 可以通过空模板参数列表来强制调用函数模板
3. 函数模板也可以发生重载
4. 如果函数模板可以产生更好的匹配,优先调用函数模板

```cpp
void myPrint(int a, int b){
	cout << "调用的普通函数" << endl;
}

template<typename T>
void myPrint(T a, T b) { 
	cout << "调用的模板" << endl;
}

template<typename T>
void myPrint(T a, T b, T c) { 
	cout << "调用重载的模板" << endl; 
}

void test01(){
	//1、如果函数模板和普通函数都可以实现，优先调用普通函数
	// 注意 如果告诉编译器  普通函数是有的，但只是声明没有实现，或者不在当前文件内实现，就会报错找不到
	int a = 10;
	int b = 20;
	myPrint(a, b); //调用普通函数

	//2、可以通过空模板参数列表来强制调用函数模板
	myPrint<>(a, b); //调用函数模板

	//3、函数模板也可以发生重载
	int c = 30;
	myPrint(a, b, c); //调用重载的函数模板

	//4、 如果函数模板可以产生更好的匹配,优先调用函数模板
	char c1 = 'a';
	char c2 = 'b';
	myPrint(c1, c2); //调用函数模板
}
```

总结：既然提供了函数模板，最好就不要提供普通函数，否则容易出现二义性。

###### 4. 提供模板的重载

为**特定的类型**提供**具体化的模板**

```cpp
class Person{
public:
	Person(string name, int age){
		this->m_Name = name;
		this->m_Age = age;
	}
	string m_Name;
	int m_Age;
};

//普通函数模板
template<class T>
bool myCompare(T& a, T& b){
	if (a == b){
		return true;
	}else
    {
		return false;
	}
}

//具体化，显示具体化的原型和定意思以template<>开头，并通过名称来指出类型
//具体化优先于常规模板
template<> bool myCompare(Person &p1, Person &p2){
	if ( p1.m_Name  == p2.m_Name && p1.m_Age == p2.m_Age){
		return true;
	}else
	{
		return false;
	}
}

Person p1("Tom", 10);
Person p2("Tom", 10);
//自定义数据类型，不会调用普通的函数模板
//可以创建具体化的Person数据类型的模板，用于特殊处理这个类型
bool ret = myCompare(p1, p2);
```

##### 2. 类模板
###### 1. 类模板语法

**作用**：建立一个通用类，类中的成员 数据类型可以不具体制定，用一个**虚拟的类型**来代表。

**语法：** 

```cpp
template<typename T> // 或 template<class T>, T 可以用其他标识符
// 类声明，如：
class cls{};
```

```cpp
//类模板
template<class NameType, class AgeType> 
class Person{
public:
	Person(NameType name, AgeType age){
		this->mName = name;
		this->mAge = age;
	}
	void showPerson(){
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

// 指定NameType 为string类型，AgeType 为 int类型
Person<string, int>P1("孙悟空", 999);
P1.showPerson();
```
###### 2. 类模板与函数模板区别

1. 类模板没有自动类型推导的使用方式
2. 类模板在模板参数列表中可以有默认参数

```cpp
template<class NameType, class AgeType = int>  // 默认int
```

###### 3. 类模板中成员函数创建时机

* 普通类中的成员函数一开始就可以创建
* 类模板中的成员函数在调用时才创建

```cpp
class Person1{
    public:
        void showPerson1()
            {cout << "Person1 show" << endl;}
    };

class Person2{
    public:
        void showPerson2()
            {cout << "Person2 show" << endl;}
    };

template<class T>
class MyClass{
    public:
        T obj;
        void fun1()
            { obj.showPerson1(); } // 可以编译通过
        void fun2()
            { obj.showPerson2(); } // 可以编译通过
    };

MyClass<Person1> m;
m.fun1();
//m.fun2();// 不能调用，编译会出错，说明函数调用才会去创建成员函数
```



###### 4. 类模板对象做函数参数

一共有三种传入方式：

1. 指定传入的类型   --- 直接显示对象的数据类型
2. 参数模板化           --- 将对象中的参数变为模板进行传递
3. 整个类模板化       --- 将这个对象类型 模板化进行传递

```cpp
#include <string>
//类模板
template<class NameType, class AgeType = int> 
class Person{
public:
	Person(NameType name, AgeType age){
		this->mName = name;
		this->mAge = age;
	}
	void showPerson(){
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

//1、指定传入的类型
void printPerson1(Person<string, int> &p) {
	p.showPerson();
}

//2、参数模板化
template <class T1, class T2>
void printPerson2(Person<T1, T2>&p){
	p.showPerson();
}

//3、整个类模板化
template<class T>
void printPerson3(T & p){
	p.showPerson();
}

Person <string, int >p("唐僧", 30);
printPerson1(p);
printPerson2(p);
printPerson3(p);
```

* 使用比较广泛是第一种：指定传入的类型

###### 5. 类模板与继承

* 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
* 如果不指定，编译器无法给子类分配内存
* 如果想灵活指定出父类中T的类型，子类也需变为类模板



###### 6. 类模板成员函数类外实现

```cpp
//类模板中成员函数类外实现
template<class T1, class T2>
class Person {
public:
	//成员函数类内声明
	Person(T1 name, T2 age);
	void showPerson();

public:
	T1 m_Name;
	T2 m_Age;
};

//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}

//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() {
	cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}
```

总结：类模板中成员函数类外实现时，需要加上模板参数列表。

###### 7. 类模板分文件编写

问题：

* 类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到


解决：

* 解决方式1：直接包含.cpp源文件
* 解决方式2：将声明和实现写到同一个文件中，并更改后缀名为.hpp，hpp是约定的名称，并不是强制

person.hpp中代码：

```cpp
#pragma once
#include <iostream>
using namespace std;
#include <string>

template<class T1, class T2>
class Person {
public:
	Person(T1 name, T2 age);
	void showPerson();
public:
	T1 m_Name;
	T2 m_Age;
};

//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}

//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() {
	cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}
```

类模板分文件编写.cpp中代码

```cpp
#include<iostream>
using namespace std;

//#include "person.h"
#include "person.cpp" //解决方式1，包含cpp源文件

//解决方式2，将声明和实现写到一起，文件后缀名改为.hpp
#include "person.hpp"
void test01(){
	Person<string, int> p("Tom", 10);
	p.showPerson();
}

int main() {
	test01();
	system("pause");
	return 0;
}
```

总结：主流的解决方式是第二种，将类模板成员函数写到一起，并将后缀名改为.hpp

###### 8. 类模板与友元

全局函数类内实现 - 直接在类内声明友元即可

全局函数类外实现 - 需要提前让编译器知道全局函数的存在

```cpp
template<class T1, class T2>
class Person{
	//1、全局函数配合友元  类内实现
	friend void printPerson(Person<T1, T2> & p){
        // ...
	}
}

// 调用
Person<string, int >p("Tom", 20);
printPerson(p);
```

```cpp
// 全局函数配合友元  类外实现 4步：

//1、声明类模板。
template<class T1, class T2>
class Person;

//2、声明函数模板。 或直接写出函数体。
template<class T1, class T2>
void printPerson2(Person<T1, T2> & p); 

//3、类内friend声明。
template<class T1, class T2>
class Person{
	//3、全局函数配合友元  类外实现
	friend void printPerson(Person<T1, T2> & p);
}

//4、实现函数模板。
template<class T1, class T2>
void printPerson(Person<T1, T2> & p){
    // ...
}
```
#### 2. STL初识
##### 1. STL的诞生

* 长久以来，软件界一直希望建立一种可重复利用的东西

* C++的**面向对象**和**泛型编程**思想，目的就是**复用性的提升**

* 大多情况下，数据结构和算法都未能有一套标准,导致被迫从事大量重复工作

* 为了建立数据结构和算法的一套标准,诞生了**STL**

##### 2. STL基本概念

* STL(Standard Template Library,**标准模板库**)
* STL 从广义上分为: **容器(container) 算法(algorithm) 迭代器(iterator)**
* **容器**和**算法**之间通过**迭代器**进行无缝连接。
* STL 几乎所有的代码都采用了模板类或者模板函数

##### 3. STL六大组件

STL大体分为六大组件，分别是:**容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器**

1. 容器：各种数据结构，如vector、list、deque、set、map等,用来存放数据。
2. 算法：各种常用的算法，如sort、find、copy、for_each等
3. 迭代器：扮演了容器与算法之间的胶合剂。
4. 仿函数：行为类似函数，可作为算法的某种策略。
5. 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西。
6. 空间配置器：负责空间的配置与管理。
##### 4.  STL中容器、算法、迭代器

**容器：**置物之所也

STL**容器**就是将运用**最广泛的一些数据结构**实现出来

常用的数据结构：数组, 链表,树, 栈, 队列, 集合, 映射表 等

这些容器分为**序列式容器**和**关联式容器**两种:

​	**序列式容器**:强调值的排序，序列式容器中的每个元素均有固定的位置。
​	**关联式容器**:二叉树结构，各元素之间没有严格的物理上的顺序关系

**算法：**问题之解法也

有限的步骤，解决逻辑或数学上的问题，这一门学科我们叫做算法(Algorithms)

算法分为:**质变算法**和**非质变算法**。

质变算法：是指运算过程中会更改区间内的元素的内容。例如拷贝，替换，删除等等

非质变算法：是指运算过程中不会更改区间内的元素内容，例如查找、计数、遍历、寻找极值等等

**迭代器：**容器和算法之间粘合剂

提供一种方法，使之能够依序寻访某个容器所含的各个元素，而又无需暴露该容器的内部表示方式。

每个容器都有自己专属的迭代器

迭代器使用非常类似于指针，初学阶段我们可以先理解迭代器为指针

迭代器种类：

| 种类           | 功能                                                     | 支持运算                                |
| -------------- | -------------------------------------------------------- | --------------------------------------- |
| 输入迭代器     | 对数据的只读访问                                         | 只读，支持++、==、！=                   |
| 输出迭代器     | 对数据的只写访问                                         | 只写，支持++                            |
| 前向迭代器     | 读写操作，并能向前推进迭代器                             | 读写，支持++、==、！=                   |
| 双向迭代器     | 读写操作，并能向前和向后操作                             | 读写，支持++、--，                      |
| 随机访问迭代器 | 读写操作，可以以跳跃的方式访问任意数据，功能最强的迭代器 | 读写，支持++、--、[n]、-n、<、<=、>、>= |

常用的容器中迭代器种类为双向迭代器，和随机访问迭代器

#### 3.STL- 常用容器
##### 1. string容器
###### 1. string特点

* char * 是一个指针
* string是一个类，类内部封装了char\*，管理这个字符串，是一个char*型的容器。

- string 类内部封装了很多成员方法

- string管理char*所分配的内存，不用担心复制越界和取值越界等，由类内部进行负责。
###### 2. string构造函数

* `string();`          				//创建一个空的字符串 例如: string str;
  `string(const char* s);`	        //使用字符串s初始化
* `string(const string& str);`    //使用一个string对象初始化另一个string对象
* `string(int n, char c);`           //使用n个字符c初始化 

```cpp
#include <string>

string s1; //创建空字符串，调用无参构造函数
const char* str = "hello world";
string s2(str); //把c_string转换成了string
string s3(s2); //调用拷贝构造函数
string s4(10, 'a');
```
###### 3. string赋值操作

* `string& operator=(const char* s);`             //char*类型字符串 赋值给当前的字符串
* `string& operator=(const string &s);`         //把字符串s赋给当前的字符串
* `string& operator=(char c);`                          //字符赋值给当前的字符串
* `string& assign(const char *s);`                  //把字符串s赋给当前的字符串
* `string& assign(const char *s, int n);`     //把字符串s的前n个字符赋给当前的字符串
* `string& assign(const string &s);`              //把字符串s赋给当前字符串
* `string& assign(int n, char c);`                  //用n个字符c赋给当前字符串

```cpp
//赋值
string str1;
str1 = "hello world";

string str2;
str2 = str1;

string str3;
str3 = 'a';

string str4;
str4.assign("hello c++");

string str5;
str5.assign("hello c++",5);

string str6;
str6.assign(str5);

string str7;
str7.assign(5, 'x');
```



###### 4. string字符串拼接

* `string& operator+=(const char* str);`                   //重载+=操作符
* `string& operator+=(const char c);`                         //重载+=操作符
* `string& operator+=(const string& str);`                //重载+=操作符
* `string& append(const char *s); `                               //把字符串s连接到当前字符串结尾
* `string& append(const char *s, int n);`                 //把字符串s的前n个字符连接到当前字符串结尾
* `string& append(const string &s);`                           //同operator+=(const string& str)
* `string& append(const string &s, int pos, int n);`//字符串s中从pos开始的n个字符连接到字符串结尾


```cpp
string str3 = "I";
str3.append(" love ");
str3.append("game abcde", 4);
str3.append(str2, 4, 3); // 从下标4位置开始 ，截取3个字符，拼接到字符串末尾
```

总结：字符串拼接的重载版本很多，初学阶段记住几种即可
###### 5. string查找和替换

* `int find(const string& str, int pos = 0) const;`              //查找str第一次出现位置,从pos开始查找
* `int find(const char* s, int pos = 0) const; `                     //查找s第一次出现位置,从pos开始查找
* `int find(const char* s, int pos, int n) const; `               //从pos位置查找s的前n个字符第一次位置
* `int find(const char c, int pos = 0) const; `                       //查找字符c第一次出现位置
* `int rfind(const string& str, int pos = npos) const;`      //查找str最后一次位置,从pos开始查找
* `int rfind(const char* s, int pos = npos) const;`              //查找s最后一次出现位置,从pos开始查找
* `int rfind(const char* s, int pos, int n) const;`              //从pos查找s的前n个字符最后一次位置
* `int rfind(const char c, int pos = 0) const;  `                      //查找字符c最后一次出现位置
* `string& replace(int pos, int n, const string& str); `       //替换从pos开始n个字符为字符串str
* `string& replace(int pos, int n,const char* s); `                 //替换从pos开始的n个字符为字符串s

```cpp
string str1 = "abcdefgde";

int pos = str1.find("de");
pos = str1.rfind("de");
str1.replace(1, 3, "1111");
```

总结：

* find查找是从左往后，rfind从右往左
* find找到字符串后返回查找的第一个字符位置，找不到返回-1
* replace在替换时，要指定从哪个位置起，多少个字符，替换成什么样的字符串
######    6. string字符串比较

**比较方式：**

* 字符串比较是按字符的ASCII码进行对比

= 返回   0

\> 返回   1 

< 返回  -1

**函数原型：**

* `int compare(const string &s) const; `  //与字符串s比较
* `int compare(const char *s) const;`      //与字符串s比较



###### 7. string字符存取

* `char& operator[](int n); `     //通过[]方式取字符
* `char& at(int n);   `                    //通过at方法获取字符

```cpp
//字符修改
str[0] = 'x';
str.at(1) = 'x';
```
###### 8. string插入和删除

* `string& insert(int pos, const char* s);  `                //插入字符串
* `string& insert(int pos, const string& str); `        //插入字符串
* `string& insert(int pos, int n, char c);`                //在指定位置插入n个字符c
* `string& erase(int pos, int n = npos);`                    //删除从Pos开始的n个字符 

###### 9. string子串

* `string substr(int pos = 0, int n = npos) const;`   //返回由pos开始的n个字符组成的字符串

```cpp
string str = "abcdefg";
string subStr = str.substr(1, 3);
```
##### 2. vector容器

###### 1. vector基本概念

**功能：**vector数据结构和**数组非常相似**，也称为**单端数组**

**vector与普通数组区别：**数组是静态空间，而vector可以**动态扩展**

* vector容器的迭代器是支持随机访问的迭代器
###### 2. vector构造函数

* `vector<T> v; `               		     //采用模板实现类实现，默认构造函数
* `vector(v.begin(), v.end());   `       //将v[begin(), end())区间中的元素拷贝给本身。
* `vector(n, elem);`                            //构造函数将n个elem拷贝给本身。
* `vector(const vector &vec);`         //拷贝构造函数。
* `vector(n);`                                       // 初始化一个长度为n的vector。


```cpp
#include <vector>

vector<int> v1; //无参构造
vector<int> v{1,2,3,5};
vector<int> v2(v1.begin(), v1.end());
vector<int> v3(10, 100);
vector<int> v4(v3);
vector<vector<int>> v; // 嵌套

// 打印数组
for (int i:nums)
    cout << i << endl;
```

- 遍历

```cpp
vector<int>::iterator pBegin = v.begin();
vector<int>::iterator pEnd = v.end();

//第一种遍历方式：
while (pBegin != pEnd) {
    cout << *pBegin << endl;
    pBegin++;
}

//第二种遍历方式：
for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
    cout << *it << endl;
cout << endl;

//第三种遍历方式：
//使用STL提供标准遍历算法  头文件 algorithm
for_each(v.begin(), v.end(), void MyPrint(int val){cout << val << endl});

// 第四种遍历方式：
for (int i:v)
    cout << i << endl;

// 第五种遍历方式：
for (int i; i<v.size(); i++)
    cout << v[i] << endl;
```



###### 3. vector赋值操作

* `vector& operator=(const vector &vec);`//重载等号操作符


* `assign(beg, end);`       //将[beg, end)区间中的数据拷贝赋值给本身。
* `assign(n, elem);`        //将n个elem拷贝赋值给本身。

```cpp
vector<int> v1; //无参构造
for (int i = 0; i < 10; i++)
    v1.push_back(i);

vector<int>v2;
v2 = v1;

vector<int>v3;
v3.assign(v1.begin(), v1.end());

vector<int>v4;
v4.assign(10, 100);
```

###### 4.  vector容量和大小

* `empty(); `                            //判断容器是否为空

* `capacity();`                      //容器的容量

* `size();`                              //返回容器中元素的个数

* `resize(int num);`             //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。

  ​					      //如果容器变短，则末尾超出容器长度的元素被删除。

* `resize(int num, elem);`  //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。

  ​				              //如果容器变短，则末尾超出容器长度的元素被删除

###### 5. vector插入和删除

* `push_back(ele);`                                         //尾部插入元素ele
* `pop_back();`                                                //删除最后一个元素
* `insert(const_iterator pos, ele);`        //迭代器指向位置pos插入元素ele
* `insert(const_iterator pos, int count,ele);`//迭代器指向位置pos插入count个元素ele
* `erase(const_iterator pos);`                     //删除迭代器指向的元素
* `erase(const_iterator start, const_iterator end);`//删除迭代器从start到end之间的元素
* `clear();`                                                        //删除容器中所有元素

```cpp
vector<int> v1;
//尾插
v1.push_back(10);
v1.push_back(20);
//尾删
v1.pop_back();
//插入
v1.insert(v1.begin(), 100);
v1.insert(v1.begin(), 2, 1000);

//删除
v1.erase(v1.begin());

//清空
v1.erase(v1.begin(), v1.end());
v1.clear();
```



###### 6. vector数据存取

* `at(int idx); `     //返回索引 idx所指的数据
* `operator[]; `       //返回索引 idx所指的数据
* `front(); `            //返回容器中第一个数据元素
* `back();`              //返回容器中最后一个数据元素

###### 7. vector互换容器

* `swap(vec);`  // 将vec与本身的元素互换

总结：swap可以使两个容器互换，可以达到实用的收缩内存效果

###### 8. vector预留空间

* 减少vector在动态扩展容量时的扩展次数

* `reserve(int len);`//容器预留len个元素长度，预留位置不初始化，元素不可访问。

##### 3. deque容器

###### 1. deque容器基本概念

* 双端数组，可以对头端进行插入删除操作

**deque与vector区别：**

* vector对于头部的插入删除效率低，数据量越大，效率越低
* deque相对而言，对头部的插入删除速度回比vector快
* vector访问元素时的速度会比deque快,这和两者内部实现有关

deque内部**工作原理**:

deque内部有个**中控器**，维护每段缓冲区中的内容，缓冲区中存放真实数据

中控器维护的是每个缓冲区的地址，使得使用deque时像一片连续的内存空间

* deque容器的迭代器也是支持随机访问的
###### 2. deque构造函数

* `deque<T> deqT`;                      //默认构造形式
* `deque(beg, end);`                  //构造函数将[beg, end)区间中的元素拷贝给本身。
* `deque(n, elem);`                    //构造函数将n个elem拷贝给本身。
* `deque(const deque &deq);`   //拷贝构造函数

```cpp
#include <deque>

//deque构造
deque<int> d1; //无参构造函数
deque<int> d2(d1.begin(),d1.end());
deque<int>d3(10,100);
deque<int>d4 = d3;

// 遍历
void printDeque(const deque<int>& d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) 
		cout << *it << " ";
	cout << endl;
}
```

###### 3. deque赋值操作

* `deque& operator=(const deque &deq); `         //重载等号操作符


* `assign(beg, end);`                                           //将[beg, end)区间中的数据拷贝赋值给本身。
* `assign(n, elem);`                                             //将n个elem拷贝赋值给本身。
###### 4. deque大小操作

* `deque.empty();`                       //判断容器是否为空

* `deque.size();`                         //返回容器中元素的个数

* `deque.resize(num);`                //重新指定容器的长度为num,若容器变长，则以默认值填充新位置。

  ​			                             //如果容器变短，则末尾超出容器长度的元素被删除。

* `deque.resize(num, elem);`     //重新指定容器的长度为num,若容器变长，则以elem值填充新位置。

  ​                                                     //如果容器变短，则末尾超出容器长度的元素被删除。
###### 5. deque 插入和删除

两端插入操作：

- `push_back(elem);`          //在容器尾部添加一个数据
- `push_front(elem);`        //在容器头部插入一个数据
- `pop_back();`                   //删除容器最后一个数据
- `pop_front();`                 //删除容器第一个数据

指定位置操作：

* `insert(pos,elem);`         //在pos位置插入一个elem元素的拷贝，返回新数据的位置。

* `insert(pos,n,elem);`     //在pos位置插入n个elem数据，无返回值。

* `insert(pos,beg,end);`    //在pos位置插入[beg,end)区间的数据，无返回值。

* `clear();`                           //清空容器的所有数据

* `erase(beg,end);`             //删除[beg,end)区间的数据，返回下一个数据的位置。

* `erase(pos);`                    //删除pos位置的数据，返回下一个数据的位置。


```cpp
//两端操作
deque<int> d;
//尾插
d.push_back(10);
d.push_back(20);
//头插
d.push_front(100);
d.push_front(200);

//尾删
d.pop_back();
//头删
d.pop_front();

//插入
deque<int> d;
d.push_back(10);
d.push_back(20);
d.push_front(100);
d.push_front(200);

d.insert(d.begin(), 1000);

d.insert(d.begin(), 2,10000);

deque<int>d2;
d2.push_back(1);
d2.push_back(2);
d2.push_back(3);

d.insert(d.begin(), d2.begin(), d2.end());

//删除
deque<int> d;
d.push_back(10);
d.push_back(20);
d.push_front(100);
d.push_front(200);

d.erase(d.begin());

d.erase(d.begin(), d.end());
d.clear();
```

###### 6. deque 数据存取

- `at(int idx); `     //返回索引idx所指的数据
- `operator[]; `      //返回索引idx所指的数据
- `front(); `            //返回容器中第一个数据元素
- `back();`              //返回容器中最后一个数据元素
###### 7.  deque 排序

* `sort(iterator beg, iterator end)`  //对beg和end区间内元素进行排序

```cpp
#include <deque>
#include <algorithm>

deque<int> d{10,20,100,200};

sort(d.begin(), d.end()); // 原位排序
```
##### 4. stack容器
###### 1. stack 基本概念

**概念：**stack是一种**先进后出**(First In Last Out,FILO)的数据结构，它只有一个出口

栈中只有顶端的元素才可以被外界使用，因此栈不允许有遍历行为

栈中进入数据称为  --- **入栈**  `push`

栈中弹出数据称为  --- **出栈**  `pop`

###### 2. stack 常用接口

* `stack<T> stk;`                                 //stack采用模板类实现， stack对象的默认构造形式
* `stack(const stack &stk);`            //拷贝构造函数

赋值操作：

* `stack& operator=(const stack &stk);`           //重载等号操作符

数据存取：

* `push(elem);`      //向栈顶添加元素
* `pop();`                //从栈顶移除第一个元素
* `top(); `                //返回栈顶元素

大小操作：

* `empty();`            //判断堆栈是否为空
* `size(); `              //返回栈的大小



**示例：**

```cpp
#include <stack>

//创建栈容器 栈容器必须符合先进后出
stack<int> s;

//向栈中添加元素，叫做 压栈 入栈
s.push(10);
s.push(20);
s.push(30);

while (!s.empty()) {
    //输出栈顶元素
    cout << "栈顶元素为： " << s.top() << endl;
    //弹出栈顶元素
    s.pop();
}
cout << "栈的大小为：" << s.size() << endl;
```
##### 5. queue 容器
###### 1. queue 基本概念

**概念：**Queue是一种**先进先出**(First In First Out,FIFO)的数据结构，它有两个出口

队列容器允许从一端新增元素，从另一端移除元素

队列中只有队头和队尾才可以被外界使用，因此队列不允许有遍历行为

队列中进数据称为 --- **入队**    `push`

队列中出数据称为 --- **出队**    `pop`
###### 2. queue 常用接口

构造函数：

- `queue<T> que;`                                 //queue采用模板类实现，queue对象的默认构造形式
- `queue(const queue &que);`            //拷贝构造函数

赋值操作：

- `queue& operator=(const queue &que);`           //重载等号操作符

数据存取：

- `push(elem);`                             //往队尾添加元素
- `pop();`                                      //从队头移除第一个元素
- `back();`                                    //返回最后一个元素
- `front(); `                                  //返回第一个元素

大小操作：

- `empty();`            //判断堆栈是否为空
- `size(); `              //返回栈的大小

```cpp
#include <queue>
#include <string>

class Person{
public:
	Person(string name, int age);
};

//创建队列
queue<Person> q;

//准备数据
Person p1("唐僧", 30);
Person p2("孙悟空", 1000);
Person p3("猪八戒", 900);
Person p4("沙僧", 800);

//向队列中添加元素  入队操作
q.push(p1);
q.push(p2);
q.push(p3);
q.push(p4);

//队列不提供迭代器，更不支持随机访问	
while (!q.empty()) {
    //输出队头元素
    cout << "队头元素-- 姓名： " << q.front().m_Name 
        << " 年龄： "<< q.front().m_Age << endl;

    cout << "队尾元素-- 姓名： " << q.back().m_Name  
        << " 年龄： " << q.back().m_Age << endl;

    cout << endl;
    //弹出队头元素
    q.pop();
}
```
##### 6. list容器
###### 1. list基本概念

**功能：**将数据进行链式存储

**链表**（list）是一种物理存储单元上非连续的存储结构，数据元素的逻辑顺序是通过链表中的指针链接实现的

- 链表的组成：链表由一系列**结点**组成

- 结点的组成：一个是存储数据元素的**数据域**，另一个是存储下一个结点地址的**指针域**

- STL中的链表是一个双向循环链表

- 由于链表的存储方式并不是连续的内存空间，因此链表list中的迭代器只支持前移和后移，属于**双向迭代器**


list的优点：

* 采用动态存储分配，不会造成内存浪费和溢出
* 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素

list的缺点：

* 链表灵活，但是空间(指针域) 和 时间（遍历）额外耗费较大

List有一个重要的性质，插入操作和删除操作都不会造成原有list迭代器的失效，这在vector是不成立的。
###### 2.  list构造函数

* `list<T> lst;`                               //list采用采用模板类实现,对象的默认构造形式：
* `list(beg,end);`                           //构造函数将[beg, end)区间中的元素拷贝给本身。
* `list(n,elem);`                             //构造函数将n个elem拷贝给本身。
* `list(const list &lst);`            //拷贝构造函数。

```cpp
#include <list>

void printList(const list<int>& L) {
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++)
		cout << *it << " ";
	cout << endl;
}
```
###### 3. list 赋值和交换

* `assign(beg, end);`            //将[beg, end)区间中的数据拷贝赋值给本身。
* `assign(n, elem);`              //将n个elem拷贝赋值给本身。
* `list& operator=(const list &lst);`         //重载等号操作符
* `swap(lst);`                         //将lst与本身的元素互换。

###### 4. list 大小操作

* `size(); `                             //返回容器中元素的个数

* `empty(); `                           //判断容器是否为空

* `resize(num);`                   //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。

  ​					    //如果容器变短，则末尾超出容器长度的元素被删除。

* `resize(num, elem); `       //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。

   //如果容器变短，则末尾超出容器长度的元素被删除。

###### 5. list 插入和删除

* `push_back(elem);`//在容器尾部加入一个元素
* `pop_back();`//删除容器中最后一个元素
* `push_front(elem);`//在容器开头插入一个元素
* `pop_front();`//从容器开头移除第一个元素
* `insert(pos,elem);`//在pos位置插elem元素的拷贝，返回新数据的位置。
* `insert(pos,n,elem);`//在pos位置插入n个elem数据，无返回值。
* `insert(pos,beg,end);`//在pos位置插入[beg,end)区间的数据，无返回值。
* `clear();`//移除容器的所有数据
* `erase(beg,end);`//删除[beg,end)区间的数据，返回下一个数据的位置。
* `erase(pos);`//删除pos位置的数据，返回下一个数据的位置。
* `remove(elem);`//删除容器中所有与elem值匹配的元素。

```cpp
#include <list>

list<int> L;
//尾插
L.push_back(10);
L.push_back(20);
L.push_back(30);
//头插
L.push_front(100);
L.push_front(200);
L.push_front(300);

//尾删
L.pop_back();

//头删
L.pop_front();

//插入
list<int>::iterator it = L.begin();
L.insert(++it, 1000);

//删除
it = L.begin();
L.erase(++it);

//移除
L.push_back(10000);
L.push_back(10000);
L.push_back(10000);
L.remove(10000);

//清空
L.clear();
```
###### 6. list 数据存取

- `front();`        //返回第一个元素。

* `back();`         //返回最后一个元素。

```cpp
#include <list>

list<int> L1{10,20,30,40};

//cout << L1.at(0) << endl;//错误 不支持at访问数据
//cout << L1[0] << endl; //错误  不支持[]方式访问数据
cout << "第一个元素为： " << L1.front() << endl;
cout << "最后一个元素为： " << L1.back() << endl;

//list容器的迭代器是双向迭代器，不支持随机访问
list<int>::iterator it = L1.begin();
//it = it + 1;//错误，不可以跳跃访问，即使是+1
```
###### 7. list 反转和排序

* `reverse();`   //反转链表
* `sort();`        //链表排序

#### 7. set/ multiset 容器

###### 1. set基本概念

**简介：**所有元素都会在插入时自动被排序

**本质：**set/multiset属于**关联式容器**，底层结构是用**二叉树**实现。

**set和multiset区别**：

* set不允许容器中有重复的元素
* multiset允许容器中有重复的元素
* set插入数据的同时会返回插入结果，表示插入是否成功。插入失败即已有元素。
###### 2. set构造和赋值

构造：

* `set<T> st;`                        //默认构造函数：
* `set(const set &st);`       //拷贝构造函数

赋值：

* `set& operator=(const set &st);`    //重载等号操作符
###### 3. set大小和交换

* `size();`          //返回容器中元素的数目
* `empty();`        //判断容器是否为空
* `swap(st);`      //交换两个集合容器

###### 4. set插入和删除

* `insert(elem);`           //在容器中插入元素。
* `clear();`                    //清除所有元素
* `erase(pos);`              //删除pos迭代器所指的元素，返回下一个元素的迭代器。
* `erase(beg, end);`    //删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
* `erase(elem);`            //删除容器中值为elem的元素。

###### 5. set查找和统计

* `find(key);`                  //查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
* `count(key);`                //统计key的元素个数

###### 6. pair对组创建

**功能描述：**成对出现的数据，利用对组可以返回两个数据

* `pair<type, type> p ( value1, value2 );`
* `pair<type, type> p = make_pair( value1, value2 );`

```cpp
#include <string>

pair<string, int> p(string("Tom"), 20);
pair<string, int> p2 = make_pair("Jerry", 10);
```

###### 7. set容器排序

* 利用仿函数，可以改变排序规则

**示例一**   set存放内置数据类型

```cpp
#include <set>

class MyCompare {
public:
	bool operator()(int v1, int v2)
		return v1 > v2;
};

set<int> s1;
s1.insert(10);
s1.insert(40);
s1.insert(20);
s1.insert(30);
s1.insert(50);

//默认从小到大
for (set<int>::iterator it = s1.begin(); it != s1.end(); it++)
    cout << *it << " ";
cout << endl;

//指定排序规则
set<int,MyCompare> s2;
s2.insert(10);
s2.insert(40);
s2.insert(20);
s2.insert(30);
s2.insert(50);

for (set<int, MyCompare>::iterator it = s2.begin(); it != s2.end(); it++)
    cout << *it << " ";
cout << endl;
```

**示例二** set存放自定义数据类型

```cpp
#include <set>
#include <string>

class Person{
public:
	Person(string name, int age){
		this->m_Name = name;
		this->m_Age = age;
	}
	string m_Name;
	int m_Age;
};

class comparePerson{
public:
	bool operator()(const Person& p1, const Person &p2)
		//按照年龄进行排序  降序
		return p1.m_Age > p2.m_Age;
};

set<Person, comparePerson> s;

Person p1("刘备", 23);
Person p2("关羽", 27);
Person p3("张飞", 25);
Person p4("赵云", 21);

s.insert(p1);
s.insert(p2);
s.insert(p3);
s.insert(p4);

for (set<Person, comparePerson>::iterator it = s.begin(); it != s.end(); it++)
    cout << "姓名： " << it->m_Name << " 年龄： " << it->m_Age << endl;
```

对于自定义数据类型，set必须指定排序规则才可以插入数据
##### 8. map/ multimap容器

###### 1. map基本概念

* map中所有元素都是pair
* pair中第一个元素为key（键值），起到索引作用，第二个元素为value（实值）
* 所有元素都会根据元素的键值自动排序

**本质：**map/multimap属于**关联式容器**，底层结构是用二叉树实现。

**优点：**可以根据key值快速找到value值

map和multimap**区别**：

- map不允许容器中有重复key值元素
- multimap允许容器中有重复key值元素
###### 2.  map构造和赋值

* `map<T1, T2> mp;`                     //map默认构造函数: 
* `map(const map &mp);`             //拷贝构造函数

* `map& operator=(const map &mp);`    //重载等号操作符

```cpp
#include <map>

map<string,int> people{{"li", 21}, {"wang", 32}};

void printMap(map<int,int>&m){
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++)
		cout << "key = " << it->first << " value = " << it->second << endl;
	cout << endl;
}
```
###### 3. map大小和交换

- `size();`          //返回容器中元素的数目
- `empty();`        //判断容器是否为空
- `swap(st);`      //交换两个集合容器

###### 4. map插入和删除

- `insert(elem);`           //在容器中插入元素。
- `clear();`                    //清除所有元素
- `erase(pos);`              //删除pos迭代器所指的元素，返回下一个元素的迭代器。
- `erase(beg, end);`    //删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
- `erase(key);`            //删除容器中值为key的元素。

```cpp
#include <map>

//插入
map<int, int> m;
//第一种插入方式
m.insert(pair<int, int>(1, 10));
//第二种插入方式
m.insert(make_pair(2, 20));
//第三种插入方式
m.insert(map<int, int>::value_type(3, 30));
//第四种插入方式
m[4] = 40; 

//删除
m.erase(m.begin());

m.erase(3);

//清空
m.erase(m.begin(),m.end());
m.clear();
```

###### 5. map查找和统计

- `find(key);`                  //查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
- `count(key);`                //统计key的元素个数

###### 6. map容器排序

- 利用仿函数，可以改变排序规则

```cpp
#include <map>

class MyCompare {
public:
	bool operator()(int v1, int v2) 
		return v1 > v2;
};

//默认从小到大排序
//利用仿函数实现从大到小排序
map<int, int, MyCompare> m;

m.insert(make_pair(1, 10));
m.insert(make_pair(2, 20));
m.insert(make_pair(3, 30));
m.insert(make_pair(4, 40));
m.insert(make_pair(5, 50));
```
###### 7. unordered_map

- **unordered_map**内部为无序，查找速度达到o(1)。

### 33.STL- 函数对象
#### 1. 函数对象
##### 1. 函数对象概念

**概念：**

* 重载**函数调用操作符**的类，其对象常称为**函数对象**
* **函数对象**使用重载的()时，行为类似函数调用，也叫**仿函数**

**本质：**函数对象(仿函数)是一个**类**，不是一个函数

##### 2.  函数对象特点：

* 函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
* 函数对象超出普通函数的概念，函数对象可以有自己的状态
* 函数对象可以作为参数传递

```cpp
#include <string>

//1、函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
class MyAdd{
public :
	int operator()(int v1,int v2)
		return v1 + v2;
};

void test01(){
	MyAdd myAdd;
	cout << myAdd(10, 10) << endl;
}

//2、函数对象可以有自己的状态
class MyPrint{
public:
	MyPrint(){
		count = 0;
    }
	void operator()(string test){
		cout << test << endl;
		count++; //统计使用次数
	}

	int count; //内部自己的状态
};

void test02(){
	MyPrint myPrint;
	myPrint("hello world");
	myPrint("hello world");
	myPrint("hello world");
	cout << "myPrint调用次数为： " << myPrint.count << endl;
}

//3、函数对象可以作为参数传递
void doPrint(MyPrint &mp , string test){
	mp(test);
}

void test03(){
	MyPrint myPrint;
	doPrint(myPrint, "Hello C++");
}
```



#### 2.  谓词

* 返回bool类型的仿函数称为**谓词**
* 如果operator()接受一个参数，那么叫做一元谓词
* 如果operator()接受两个参数，那么叫做二元谓词

```cpp
#include <vector>
#include <algorithm>

//1.一元谓词
struct GreaterFive{
	bool operator()(int val) {
		return val > 5;
	}
};

void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++)
		v.push_back(i);

	vector<int>::iterator it = find_if(v.begin(), v.end(), GreaterFive());
	if (it == v.end())
		cout << "没找到!" << endl;
	else
		cout << "找到:" << *it << endl;
}
```

```cpp
#include <vector>
#include <algorithm>
//二元谓词
class MyCompare{
public:
	bool operator()(int num1, int num2)
		return num1 > num2;
};

void test01(){
	vector<int> v{10,40,20,30,50};

	//默认从小到大
	sort(v.begin(), v.end());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
		cout << *it << " ";
	cout << endl;
	cout << "----------------------------" << endl;

	//使用函数对象改变算法策略，排序从大到小
	sort(v.begin(), v.end(), MyCompare());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
		cout << *it << " ";
	cout << endl;
}
```
#### 3. 内建函数对象
##### 1. 内建函数对象分类

* 算术仿函数

* 关系仿函数

* 逻辑仿函数

**用法：**

* 这些仿函数所产生的对象，用法和一般函数完全相同
* 使用内建函数对象，需要引入头文件 `#include<functional>`

##### 2. 算术仿函数

**功能描述：**

* 实现四则运算
* 其中negate是一元运算，其他都是二元运算

**仿函数原型：**

* `template<class T> T plus<T>`                //加法仿函数
* `template<class T> T minus<T>`              //减法仿函数
* `template<class T> T multiplies<T>`    //乘法仿函数
* `template<class T> T divides<T>`         //除法仿函数
* `template<class T> T modulus<T>`         //取模仿函数
* `template<class T> T negate<T>`           //取反仿函数



**示例：**

```cpp
#include <functional>

//negate
	negate<int> n;
	cout << n(50) << endl;

//plus
	plus<int> p;
	cout << p(10, 20) << endl;
```
##### 3. 关系仿函数

* `template<class T> bool equal_to<T>`                    //等于
* `template<class T> bool not_equal_to<T>`            //不等于
* `template<class T> bool greater<T>`                      //大于
* `template<class T> bool greater_equal<T>`          //大于等于
* `template<class T> bool less<T>`                           //小于
* `template<class T> bool less_equal<T>`               //小于等于

##### 4. 逻辑仿函数

* `template<class T> bool logical_and<T>`              //逻辑与
* `template<class T> bool logical_or<T>`                //逻辑或
* `template<class T> bool logical_not<T>`              //逻辑非

### 34. STL- 常用算法

* 算法主要是由头文件`<algorithm>` `<functional>` `<numeric>`组成。

* `<algorithm>`是所有STL头文件中最大的一个，范围涉及到比较、 交换、查找、遍历操作、复制、修改等等
* `<numeric>`体积很小，只包括几个在序列上面进行简单数学运算的模板函数
* `<functional>`定义了一些模板类,用以声明函数对象。



#### 1. 常用遍历算法

* `for_each`     //遍历容器
* `transform`   //搬运容器到另一个容器中

##### 1. for_each

* `for_each(iterator beg, iterator end, _func);  `

  // 遍历算法 遍历容器元素

  // beg 开始迭代器

  // end 结束迭代器

  // _func 函数或者函数对象

```cpp
#include <algorithm>
#include <vector>

//普通函数
void print01(int val) {
	cout << val << " ";
}
//函数对象
class print02 {
 public:
	void operator()(int val) {
		cout << val << " ";
	}
};

//for_each算法基本用法
for_each(v.begin(), v.end(), print01);
cout << endl;

for_each(v.begin(), v.end(), print02());
cout << endl;
```

##### 2. transform

* `transform(iterator beg1, iterator end1, iterator beg2, _func);`

//beg1 源容器开始迭代器

//end1 源容器结束迭代器

//beg2 目标容器开始迭代器

//_func 函数或者函数对象

```cpp
#include<vector>
#include<algorithm>

//常用遍历算法  搬运 transform

class TransForm{
public:
	int operator()(int val){
		return val;
	}
};

vector<int>v;
vector<int>vTarget; //目标容器
vTarget.resize(v.size()); // 目标容器需要提前开辟空间
transform(v.begin(), v.end(), vTarget.begin(), TransForm());
```
#### 2. 常用查找算法

- `find`                     //查找元素
- `find_if`               //按条件查找元素
- `adjacent_find`    //查找相邻重复元素
- `binary_search`    //二分查找法
- `count`                   //统计元素个数
- `count_if`             //按条件统计元素个数

##### 1. find

- `find(iterator beg, iterator end, value);  `

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  // beg 开始迭代器

  // end 结束迭代器

  // value 查找的元素

##### 2. find_if

- `find_if(iterator beg, iterator end, _Pred);  `

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  // beg 开始迭代器

  // end 结束迭代器

  // _Pred 函数或者谓词（返回bool类型的仿函数）

##### 3. adjacent_find

- `adjacent_find(iterator beg, iterator end);  `

  // 查找相邻重复元素,返回相邻元素的第一个位置的迭代器

  // beg 开始迭代器

  // end 结束迭代器

##### 4. binary_search

- `bool binary_search(iterator beg, iterator end, value);  `

  // 查找指定的元素，查到 返回true  否则false

  // 注意: 在**无序序列中不可用**

  // beg 开始迭代器

  // end 结束迭代器

  // value 查找的元素
##### 5. count

- `count(iterator beg, iterator end, value);  `

  // 统计元素出现次数

  // beg 开始迭代器

  // end 结束迭代器

  // value 统计的元素
##### 6. count_if

- `count_if(iterator beg, iterator end, _Pred);  `

  // 按条件统计元素出现次数

  // beg 开始迭代器

  // end 结束迭代器

  // _Pred 谓词

#### 3. 常用排序算法

- `sort`             //对容器内元素进行排序
- `random_shuffle`   //洗牌   指定范围内的元素随机调整次序
- `merge `           // 容器元素合并，并存储到另一容器中
- `reverse`       // 反转指定范围的元素

##### 1. sort

- `sort(iterator beg, iterator end, _Pred);  `

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  //  beg    开始迭代器

  //  end    结束迭代器

  // _Pred  谓词

```cpp
#include <algorithm>
#include <vector>

vector<int> v{10,30,50,40};

//sort默认从小到大排序
sort(v.begin(), v.end());

//从大到小排序
sort(v.begin(), v.end(), greater<int>()); 
```

##### 2. random_shuffle

- `random_shuffle(iterator beg, iterator end);  `

  // 指定范围内的元素随机调整次序

  // beg 开始迭代器

  // end 结束迭代器


```cpp
#include <algorithm>
#include <vector>
#include <ctime>

srand((unsigned int)time(NULL));
vector<int> v;
for(int i = 0 ; i < 10;i++)
    v.push_back(i);

//打乱顺序
random_shuffle(v.begin(), v.end());
```

**总结：** random_shuffle洗牌算法比较实用，使用时记得加随机数种子

##### 3. merge

- `merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);  `

  // 容器元素合并，并存储到另一容器中

  // 注意: 两个容器必须是**有序的**

  // beg1   容器1开始迭代器
  // end1   容器1结束迭代器
  // beg2   容器2开始迭代器
  // end2   容器2结束迭代器
  // dest    目标容器开始迭代器
##### 4. reverse

- `reverse(iterator beg, iterator end);  `

  // 反转指定范围的元素

  // beg 开始迭代器

  // end 结束迭代器

#### 4 常用拷贝和替换算法

- `copy`                      // 容器内指定范围的元素拷贝到另一容器中
- `replace`                // 将容器内指定范围的旧元素修改为新元素
- `replace_if `          // 容器内指定范围满足条件的元素替换为新元素
- `swap`                     // 互换两个容器的元素

##### 1. copy

- `copy(iterator beg, iterator end, iterator dest);  `

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  // beg  开始迭代器

  // end  结束迭代器

  // dest 目标起始迭代器
##### 2. replace

- `replace(iterator beg, iterator end, oldvalue, newvalue);  `

  // 将区间内旧元素 替换成 新元素

  // beg 开始迭代器

  // end 结束迭代器

  // oldvalue 旧元素

  // newvalue 新元素
##### 3. replace_if

- `replace_if(iterator beg, iterator end, _pred, newvalue);  `

  // 按条件替换元素，满足条件的替换成指定元素

  // beg 开始迭代器

  // end 结束迭代器

  // _pred 谓词

  // newvalue 替换的新元素
##### 4. swap

- `swap(container c1, container c2);  `

  // 互换两个容器的元素

  // c1容器1

  // c2容器2
#### 5. 常用算术生成算法

* 算术生成算法属于小型算法，使用时包含的头文件为 `#include <numeric>`

**算法简介：**

- `accumulate`      // 计算容器元素累计总和

- `fill`                 // 向容器中添加元素

##### 1. accumulate

- `accumulate(iterator beg, iterator end, value);  `

  // 计算容器元素累计总和

  // beg 开始迭代器

  // end 结束迭代器

  // value 起始值
##### 2. fill

- `fill(iterator beg, iterator end, value);  `

  // 向容器中填充元素

  // beg 开始迭代器

  // end 结束迭代器

  // value 填充的值

#### 6. 常用集合算法

- `set_intersection`          // 求两个容器的交集

- `set_union`                       // 求两个容器的并集

- `set_difference `              // 求两个容器的差集

##### 1. set_intersection

- `set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);  `

  // 求两个集合的交集

  // **注意:两个集合必须是有序序列**

  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器

* 求交集的两个集合必须的有序序列
* 目标容器开辟空间需要从**两个容器中取小值**
* set_intersection返回值既是交集中最后一个元素的位置

##### 2. set_union

- `set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);  `

  // 求两个集合的并集

  // **注意:两个集合必须是有序序列**

  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器
##### 3. set_difference

- `set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);  `

  // 求两个集合的差集

  // **注意:两个集合必须是有序序列**

  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器





