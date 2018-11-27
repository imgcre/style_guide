## 其他c++特性
### auto
在C++11中，若变量被声明为`auto`，那么他的类型就会在编译时自动匹配为初始化表达式的类型。  
``` c++
vector<string> v;
...
auto s1 = v[0]  // 创建v[0]的拷贝
const auto& s2 = v[0]  // s2是v[0]的一个引用
```

C++类型名有时候又长又臭，特别是涉及模板或命名空间时：  
``` c++
sparse_hash_map<string, int>::iterator iter = m.find(val);
```

可以这样重构上面的代码：  
``` c++
auto iter = m.find(val);
```

如果没有`auto`，我们有时会在同一个表达式里重复书写同一类型名称(也可以用列表初始化来避免重复)：  
``` c++
diagnostics::ErrorStatus* status = new diagnostics::ErrorStatus("xyz");
```

如果使用`auto`的话：  
``` c++
auto status = new diagnostics::ErrorStatus("xyz");
```

但请只在局部变量中使用auto，而不要用在文件作用域、命名空间作用域和类数据成员变量上。  

### 列表初始化
在C++03中，只有聚合对象（数组和不自带构造函数的结构体）可以使用列表初始化。  
在C++11中，列表初始化被推广，任何对象类型都可以使用列表初始化：  
``` c++
//vector接收了一个初始化列表
vector<string> v{"foo", "bar"};

//也可以使用这种语法
vector<string> v = {"foo", "bar"};

//可以配合new一起使用
auto p = new vector<string>{"foo", "bar"};

//用于map和他的pair
map<int, string> m = {{1, "one"}, {2, "2"}};

//初始化列表可用于返回值的表达式（隐式转型）
vector<int> test_function() { return {1, 2, 3}; }

//迭代初始化列表
for (int i : {-1, -2, -3}) {}

//在函数调用中使用列表初始化
void TestFunctions(vector<int> v) {}_
TestFunctions2({1, 2, 3});
```

列表初始化适用于常规数据类型的构造，即使没有接收`std::initializer_list<T>`的构造函数。  
``` c++
double d{1.23};  // 列表初始化！

//类型直接带上常规构造函数
class MyType {
public:
    explicit MyType(string);
    MyType(int, string);
};

MyType m = {1, "b"};
//但如果构造函数是显式的，那么就不能使用列表初始化
MtType m{"b"};  // 错误
```

如果`auto`和列表初始化混用，要指明所初始化对象的类型，否则其将为`std::initializer_list`。  
``` c++
auto d = {1.23};  // d 为 std::initializer_list<double>
auto d = double{1.23};  // d 为 double
```

用户自定义类型可以定义接收`std::initializer_list<T>`的构造函数和赋值运算符，以自动列表初始化：  
``` c++
class MyType {
public:
    MyType(std::initializer_list<int> init_list) {
        for (int i : init_list) append(i);
    }

    MyType& operator=(std::initializer_list<int> init_list) {
        clear();
        for (int i : init_list) append(i);
    }
}

MyType m{2, 3, 5, 7};
```

### Lambda表达式
Lambda表达式是创建匿名函数对象的一种简易途径，常用于把函数当作参数传递：  
``` c++
std::sort(v.begin(), v.end(), [](int x, int y) {
    return Weight(x) < Weight(y);
});
```

Lambda，`std::functions`和`std::bind`可以搭配成通用回调机制。  
请不要过度使用Lambda，如果Lambda函数体超过5行，应该考虑改用函数。  

### 模板编程
利用模板编程，可以实现编译时类型判断等一系列编程技巧，有些工具如果脱离了模板时无法实现的。  
但是模板使用了很多技巧，这模板代码变得晦涩难懂，并且不易调试和维护。因模板导致的编译出错信息非常不友好、难以理解。  
所以模板编程最好指用在少量基础组件和基础数据结构上，如果一定要使用，需要尽可能最小化复杂度，不要对外暴露模板。  

### Boost库
Boost库是一个广受欢迎的，经同行鉴定的，免费开源的C++库集。  
Boost代码质量普遍较高，填补了C++标准库的很多空白。  
推荐使用Boost中被任何的库。  
