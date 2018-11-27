## 注释
注释虽然写起来痛苦，可是对保证代码可读性起至关重要的作用。  
但是注释虽然重要，最好的代码应该本身就是文档，有意义的类型名和变量名，要远远胜过含糊不清的文字。  
你写的注释是给读者看的，是下一个需要理解你代码的人，他有可能就是你自己。  

### 注释风格
使用`//`和`/**/`均可，但要保证风格统一。  

### 实现注释
需要对代码中巧妙的。晦涩的，有趣的，重要的地方加以注释。  

#### 代码前注释
巧妙或复杂的代码前要加以注释：  
``` c++
// Divide result by two, taking into account that x
// contains the carry from the add.
for (int i = 0; i < result->size(); i++) {
  x = (x << 8) + (*result)[i];
  (*result)[i] = x >> 1;
  x &= 1;
}
```

#### 行注释
比较隐晦的地方要加行注释，在行尾空两个再加注释：  
``` c++
// If we have enough memory, mmap the data portion too.
mmap_budget = max<int64>(0, mmap_budget - index_->length());
if (mmap_budget >= data_size_ && !MmapData(mmap_chunk_bytes, mlock))
  return;  // Error already logged.
```
这里两段注释分别描述的是代码的作用，和提示函数返回时记录以及被记入日志。  
如果需要连续多行注释，可以使之对齐以具有更好的可读性：  
``` c++
DoSomething();                  // Comment here so the comments line up.
DoSomethingElseThatIsLonger();  // Two spaces between the code and the comment.
{ // One space before comment when opening a new scope is allowed,
  // thus the comment lines up with the following comments and code.
  DoSomethingElse();  // Two spaces before line comments normally.
}
std::vector<string> list{
                    // Comments in braced lists describe the next element...
                    "First item",
                    // .. and should be aligned appropriately.
"Second item"};
DoSomething(); /* For trailing block comments, one space is fine. */
```

#### 函数参数注释
万不得已时，才考虑再调用处解释参数的意义。  
如果函数参数意义不明显，考虑这么重构：  
- 更改函数签名，例如将`bool`类型常量修改为`enum`类型。  
- 将某些参数使用类或结构体传递，这样可以表明其含义，并减少函数参数数量，使函数易读易写，并可减少对调用点代码的修改。  
- 使用具名变量来传递参数，而不是在调用参数时使用复杂的表达式。  
对比下面的代码：  
``` c++
//这些参数的意义是？
const DecimalNumber product = CalculateProduct(value, 7 ,false, nullptr);
```
这是使用类和结构体传递参数后的代码：  
``` c++
ProductOptions options;
options.set_precision_decimals(7);
options.set_use_cache(ProductOptions::kDontUseCache);
const DecimalNumber product = 
    CalculateProduct(values, options, /*completion_callback=*/nullptr);
```

#### 不允许的行为
不要用自然翻译代码作为注释，除非代码的行为真的非常不明显。  
你所提供的注释应该解释代码**为什么**这么做和代码的目的。  
请对比下面三段代码：  
``` c++
// Find the element in the vector.  <-- 差: 这太明显了!
auto iter = std::find(v.begin(), v.end(), element);
if (iter != v.end()) {
    Process(element);
}
```

``` c++
// Process "element" unless it was already processed.
auto iter = std::find(v.begin(), v.end(), element);
if (iter != v.end()) {
    Process(element);
}
```

自文档化的代码根本不需要注释，上面两例的代码对下面的代码而已是不必要的。  
``` c++
if (!IsAlreadyProcessed(element)) {
    Process(element);
}
```

### TODO注释
对那些零时、短期或者不完美的代码，使用`TODO`注释。  
`TODO`注释的主要目的是让添加注释的人可以根据规范的`TODO`格式进行查找。  
``` c++
// TODO(kl@gmail.com): Use a "*" here for concatenation operator.
// TODO(Zeke) change this to use relations.
// TODO(bug 12345): remove the "Last visitors" feature
```
可以在`TODO`中附上明确的时间或明确的事项。  

### 变量注释
通常变量名足以说明变量的用途，但是某些情况下也需要额外的注释：  

#### 类数据成员
如果变量类型和变量名称足以描述一个对象，就不需要加注释。但如果有非变量的参数（例如特殊值，数据成员之间的关系，生命周期等）不能够用类型和变量名表达，就应该加上注释。  
如果变量可以接受`NULL`或`-1`等值，应该加已说明：  
``` c++
private:
    // Used to bounds-check table accesses. -1 means
    // that we don't yet know how many entries the table has.
    int num_total_entries_;
```

#### 全局变量
和数据成员一样，全局变量也要加注释以说明含义和用途，以及作为全局变量的原因。  

### 函数注释
函数声明处的注释描述函数的功能，函数定义处的注释描述函数实现。  

#### 函数声明
只有在函数的功能简单而明显的时候才能省略这些注释（比如取值和赋值函数）。  
函数声明的注释内容：  
- 函数的输入输出。  
- 函数是否分配了必须由调用者释放的内存。  
- 参数是否可以为空指针。  
- 是否存在函数使用上的性能隐患。  
- 如果函数可重入，其同步前提是什么？  

举例如下：  
``` c++
// Returns an iterator for this table.  It is the client's
// responsibility to delete the iterator when it is done with it,
// and it must not use the iterator once the GargantuanTable object
// on which the iterator was created has been deleted.
//
// The iterator is initially positioned at the beginning of the table.
//
// This method is equivalent to:
//    Iterator* iter = table->NewIterator();
//    iter->Seek("");
//    return iter;
// If you are going to immediately seek to another place in the
// returned iterator, it will be faster to use NewIterator()
// and avoid the extra seek.
Iterator* GetIterator() const;
```

注释函数重载时，注释的重点是被重载的部分，而不是简单重复被重载函数的注释，多数情况下，重载函数不需要额外的文档和注释。  
注释构造/析构函数时，你应当注明的是构造参数对参数做了什么（例如是狗取得了指针所有权）以及析构函数清理了什么。如果没有，则注释可以省略。析构函数通常是没有注释的。  

### 类注释
每个类的定义都要附带一份注释，用来描述类的功能和用法，除非他的功能相当明显。  
``` c++
// Iterates over the contents of a GargantuanTable.
// Example:
//    GargantuanTableIterator* iter = table->NewIterator();
//    for (iter->Seek("foo"); !iter->done(); iter->Next()) {
//      process(iter->key(), iter->value());
//    }
//    delete iter;
class GargantuanTableIterator {
  ...
};
```

类注释因为读者理解如何和何时使用类提供足够的信息，同时应该提醒正确使用此类需要考虑的因素。如果类有任何同步前提，需要用文档说明。如果类的实例可以被多线程访问，特别主题文档说明多线程下相关的规则。  
可以在类注释里用一小段代码来演示这个类的基本用法和通常用法。  
如果类的声明和定义封开了，描述类用法的注释应该和接口定义放在一起，描述类的操作的注释应当和类的实现放在一起。  
