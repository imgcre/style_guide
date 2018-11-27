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


