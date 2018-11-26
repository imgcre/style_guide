# C++风格指南

## 命名规范
### 概述
命名应使代码更易于理解  
不推荐使用缩写，除非是约定俗成的（比如`i`（迭代变量），`T`（模板参数））  

鼓励类似下面这些的变量名  
``` c++
int price_count_reader;
int num_errors;
int num_dns_connections;
```

不推荐类似下面这些的变量名  
``` c++
int n;                  //无意义
int nerr;               //含糊不清
int n_comp_conns;   
int pc_reader;
int wgc_connections;    //只有本项目开发者能理解
int cstmr_id;           //缩写严重
```

### 变量
对各类变量（局部变量、函数参数、成员变量等）使用此约定。  
使用**下划线命名法**，即字母全为小写，不同的单词间使用下划线`_`隔开，但类成员变量需要在最后在加一个下划线`_`，无论是否为静态变量。  
``` c++
//普通变量
string table_name;
string tableName;       //反例

//类成员变量
class TableInfo {
    //...
private:
    string table_name_;
    string Pool<TableInfo>* pool_;
};

//结构体变量
struct UrlTableProperties {
    string name;
    int num_entries;
    static Pool<UrlTableProperties>* Pool;
};
``` 

### 常量
对使用`constexpr`以及`const`声明的常量使用此约定。  
使用**小驼峰命名法**，即名称首字母为小写，单词的首字母大写，单词之间不使用下划线`_`隔开。  
常量名总是以`k`开头，比如：  
``` c++
const int kDaysInAWeek = 7;
```

### 函数
函数命名分两种情况  
- 如果是常规函数，则使用[帕斯卡命名法](https://baike.baidu.com/item/帕斯卡命名法/9464494?fr=aladdin)，即单词首字母均大写，不能出现下划线。  
- 如果是对变量取值和赋值的函数，其命名规则于变量一致，并且此类函数的名称与其操作的变量名称相对应，比如`int count()`和`void set_count(int count)`。  

对于按首字母缩写的单词，应该将其视为普通单词进行命名。例如命名为`StartRpc()`而非`StartRPC()`。  
``` c++
AddTableEntry();
DeleteUrl();
OpenFileOrDie();
```

### 类
对类、结构体、类型定义`typedef`、枚举类型、类型模板参数使用此约定。  
使用[帕斯卡命名法](https://baike.baidu.com/item/帕斯卡命名法/9464494?fr=aladdin)，即单词首字母均大写，不能出现下划线。  
``` c++
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...

typedef hash_map<UrlTableProperties *, string> PropertiesMap;
using PropertiesMap = hash_map<UrlTableProperties *, string>;

enum UrlTableErrors { ...
```

### 文件
文件名使用**下划线命名法**  
C++文件以`.cc`结尾，头文件以`.h`结尾。  
应该尽量让文件名更加明确，比如`http_server_logs.h`优于`logs.h`，在定义类时文件名要成对出现，比如`foo_bar.h`和`foo_bar.cc`对应于类`FooBar`。  
内联函数请放在`.h`文件中。  

### 命名空间
命令空间使用**下划线命名法**。  
顶级命名空间的名称应该是项目名或该命名空间下代码所属团队的名称，命名空间中的代码应该放在和命名空间的名字匹配的文件夹中。  
应特别注意命名空间可能的名称冲突问题，命名空间名称冲突可能导致编译失败。例如使用`websearch::index`或者`websearch::index_util`，而非`websearch::util`。  
对于内部的命名空间，可以使用文件名避免名称冲突，比如对于文件`forbber.h`，使用`websearch::index::frobber_internal`。  

### 枚举
对枚举类型使用类命名的规则，对枚举值使用常量命名的规则。  
``` c++
enum UrlTableErrors {
    kOK = 0,
    kErrorOutOfMemory,
    kErrorMalformedInput,
};
```

## 格式

### 空行
尽量不要使用空行。  
两个函数定义间的空行不能超过2行，函数体首尾不留空行。

### 空格

#### 通用
``` c++
void f(bool b) {  // 左大括号前总是有空格.
    ...
int i = 0;  // 分号前不加空格.
// 列表初始化中大括号内的空格是可选的.
// 如果加了空格, 那么两边都要加上.
int x[] = { 0 };
int x[] = {0};

// 继承与初始化列表中的冒号前后总有空格.
class Foo : public Bar {
 public:
    // 对于单行函数的实现, 在大括号内加上空格
    // 然后是函数实现
    Foo(int b) : Bar(), baz_(b) {}  // 大括号里面是空的话, 不加空格。
    void Reset() { baz_ = 0; }  // 用括号把大括号与实现分开。
    ...
```

#### 操作符
``` c++
//赋值运算符前后均有空格
x = 0;

//其他二元操作符可有空格，为强调优先级关系和省略相应操作符的空格
v = w * x + y /z;
v = w*x + y/z;
v = w * (x + z);  // 圆括号内侧不加空格

//操作数于其一元操作符之间不加空格
x = -5;
++x;
if (x && !y)
    ...
```

#### 模板和转换
``` c++
// 尖括号'<'和'>'内侧不加空格，'<'前面不加空格，'>'和'('之间不加空格
vector<string> x;
y = static_cast<char*>(x);
```

#### 循环和条件语句
``` c++
if (b) {          // if 条件语句和循环语句关键字后均有空格。
    ...
} else {          // else 前后有空格。
    ...
}
while (test) {}   // 圆括号内测没有空格。
switch (i) {
    case 1:         // switch case 的冒号前无空格。
        ...
    case 2: break;  // 如果冒号后有代码, 加个空格。
for (int i = 0; i < 5; ++i) {  // 对于循环，在 ; 后添加个空格
```

### 变量以及数组初始化
可以使用`=`，`()`和`{}`初始化。  
``` c++
int x = 3;
int x(3);
int x{3};

string name("Some Name");
string name = "Some Name";
string name{"Some Name"};
```

请注意，非空列表初始化`{...}`会优先调用`std::initializer_list`（当请放心食用空列表初始化`{}`，他会调用默认构造函数）。如果不想使用列表初始化，请使用`()`。  
``` c++
vector<int> v(100, 1);  // 内容为 100 个 1 的向量
vector<int> v{100, 1};  // 内容为 100 和 1 的向量
```

此外，列表初始化不允许整数类型的四舍五入，可以利用这点来避免一些编程失误。  
``` c++
int pi(3.14);  // pi == 3
int pi{3.14};  // 编译错误
```


### 指针和引用
句点或箭头前后不要有空格。指针/地址操作符 (`*`, `&`) 之后不能有空格。  
在声明指针变量或参数时，星号与类型或变量名紧挨。  
``` c++
//空格前置
char *c;
const string &str;

//空格后置
char* c;
const string& str;
```

下面是**反例**：
``` c++
int x, *y;  // 声明多个变量，不建议使用 & 或 *
char * c;  // * 两边都有空格
const string & str;  // & 两边都有空格
```

### 函数声明与定义
返回类型和函数名在同一行，参数尽量放在同一行，如果放不下就对形参分行。  
``` c++
ReturnType ClassName::FunctionName(Type par_name1, Type par_name2) {
    DoSomething();
    ...
}
```

如果一行放不下所有参数：  
``` c++
ReturnType ClassName::ReallyLongFunctionName(Type par_name1, Type par_name2,
                                             Type par_name3) {
    DoSomething();
    ...
}
```

或者这么写：  
``` c++
ReturnType LongClassName::ReallyReallyReallyLongFunctionName(
        Type par_name1,
        Type par_name2,
        Type par_name3) {
    DoSomething();
    ...
}
```

注意：  
- 函数名和左圆括号间没有空格，且与左圆括号总在同一行  
- 圆括号与参数间没有空格  
- 左大括号和右圆括号间有一个空格，且与左大括号在同一行  

未被使用的参数，或者根据上下文容易看出用途的，可以省略参数名  
``` c++
class Foo {
public:
    Foo(Foo&&);
    Foo(const Foo&);
    Foo& operator=(Foo&&);
    Foo& operator=(const Foo&);
};
```

未被使用的参数，如果通途不明显，则在函数定义讲参数名注释起来：  
``` c++
class Shape {
public:
    virtual void Rotate(double radians) = 0;
};

class Circle : public Shape {
public:
    void Rotate(double radians) override;
};

void Circle::Rotate(double /*radians*/) {}
```

### Lambda表达式
函数的声明和定义的格式规则也适用于Lambda表达式，捕获列表同理。  
使用引用捕获时，变量名和`&`之间没有空格。  
``` c++
int x = 0;
auto add_to_x = [&x](int n) { x += n; };

std::set<int> blacklist = {7, 8, 9};
std::vector<int> digits = {3, 9, 1, 8, 4, 7, 1};
digits.erase(std::remove_if(digits.begin(), digits.end(), [&blacklist](int i) {
                return blacklist.find(i) != blacklist.end();
             }),
             digits.end());
```

### 函数返回值
不要再`return`表达式中加入非必要的`()`。  
``` c++
return result;
//用圆括号括住复杂表达式以改善可读性
return (some_long_condition &&
        another_condition);
```

### 函数调用
一行写完函数调用，或者对实参进行分行。尽可能适当把多个参数放在同一行中。  
参数的格式处理应该首先考虑可读性。  
``` c++
bool retval = DoSomething(argument1, argument2, argument3);
bool retval = DoSomething(averyveryveryverylongargument1,
                          argument2, argument3);
DoSomething(
        argument1, argument2,  // 两个tab
        argument3, argument4);
```

如果一些参数是复杂的表达式，可以创建临时变量来描述表达式，并将其返回值传递给函数。  
``` c++
int my_heuristic = scores[x] * y + bases[x];
bool retval = DoSomething(my_heuristic, x, y, z);
```

或者在对应实参后添加注释：  
``` c++
bool retval = DoSomething(scores[x] * y + bases[x],  // Score heuristic.
                          x, y, z);
);
```

如果参数本身就有一定的结构，可以酌情按其结构觉得参数格式。  
``` c++
// 通过 3x3 矩阵转换 widget.
my_widget.Transform(x1, x2, x3,
                    y1, y2, y3,
                    z1, z2, z3);
```

### 列表初始化
函数调用的格式规则也适用于列表初始化。  
如果列表初始化伴随名字，比如类型和恶变量名，则将其视作函数名，`{}`视作函数调用的括号，如果没有名字，则视名字长度为0。  
``` c++
// 一行列表初始化示范.
return {foo, bar};
functioncall({foo, bar});
pair<int, int> p{foo, bar};

// 当不得不断行时.
SomeFunction(
    {"assume a zero-length name before {"},  // 假设在 { 前有长度为零的名字.
    some_other_function_parameter);
SomeType variable{
    some, other, values,
    {"assume a zero-length name before {"},  // 假设在 { 前有长度为零的名字.
    SomeOtherType{
        "Very long string requiring the surrounding breaks.",  // 非常长的字符串, 前后都需要断行.
        some, other values},
    SomeOtherType{"Slightly shorter string",  // 稍短的字符串.
                  some, other, values}};
SomeType variable{
    "This is too long to fit all in one line"};  // 字符串过长, 因此无法放在同一行.
MyType m = {  // 注意了, 您可以在 { 前断行.
    superlongvariablename1,
    superlongvariablename2,
    {short, interior, list},
    {interiorwrappinglist,
     interiorwrappinglist2}};
```

### 类格式
访问声明块的声明次序依次是：`public:`、`protected:`、`private:`。
类声明的基本格式如下：  
``` c++
class MyClass : public OtherClass {
public:
    MyClass();
    explicit MyClass(int var);
    ~MyClass() {}

    void SomeFunction();
    void SomeFunctionThatDoesNothing() {
    }

    void set_some_var(int var) { some_var_ = var; }
    int some_var() const { return some_var_; }

private:
    bool SomeInternalFunction();

    int some_var_;
    int some_other_var_;
};
```

注意：  
- 基类名尽量与子类名放在同一行  
- 除了第一个关键字（一般是`public`），其他关键字前要空一行，当后面不要空行。  

### 构造函数初始值列表
构造函数的初始化列表应该在同一行中，或者并排多行。  
示例如下：  
``` c++
// 如果所有变量能放在同一行:
MyClass::MyClass(int var) : some_var_(var) {
  DoSomething();
}

// 如果不能放在同一行,
MyClass::MyClass(int var)
        : some_var_(var), some_other_var_(var + 1) {
    DoSomething();
}

// 如果初始化列表需要置于多行, 将每一个成员放在单独的一行
// 并逐行对齐
MyClass::MyClass(int var)
        : some_var_(var),             // 4 space indent
          some_other_var_(var + 1) {  // lined up
    DoSomething();
}

// 右大括号 } 可以和左大括号 { 放在同一行
// 如果这样做合适的话
MyClass::MyClass(int var)
    : some_var_(var) {}
```

### 命名空间
命名空间不要增加额外的缩进层次，例如：
``` c++
namespace {

void foo() {
  ...
}

}  // namespace
```

**不要**像下面这样，在命名空间内缩进：  
``` c++
namespace {

  void foo() {
    ...
  }

}  // namespace
```

声明嵌套命名空间时，每个命名空间独立成行：  
``` c++
namespace foo {
namespace bar {
```



//TODO  
关于类的private的位置  
关于空格  
关于{的换行  
请使用强枚举类型  
请在局部变量中使用auto  