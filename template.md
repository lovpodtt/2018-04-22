template
==
## 1 定义模板
### 1.1 函数模板
```c++
template <typename T>
int compare(const T &lhs, const T &rhs)
{
	if (lhs < rhs) return -1;
	if (rhs < lhs) return 1;
	return 0;
}
```
> 模板定义中，模板参数列表不能为空

### 1.1 实例化函数模板
> 当我们调用一个函数模板时，编译器（通常）用函数实参来为我们推断模板实参
>>编译器会推断出模板实参为int，并将int绑定到模板参数T上
>>然后编译器会实例化一个int版本的compare
```c++
std::cout << compare(1,0) << std::endl;	
```

