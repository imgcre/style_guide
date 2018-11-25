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

//TODO  
关于类的private的位置  
关于空格  
关于{的换行  
请使用强枚举类型  
请在局部变量中使用auto  