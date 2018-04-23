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

### 1.2 实例化函数模板
> 当我们调用一个函数模板时，编译器（通常）用函数实参来为我们推断模板实参
>
编译器会推断出模板实参为int，并将int绑定到模板参数T上  
然后编译器会实例化一个int版本的compare
```c++
std::cout << compare(1,0) << std::endl;	
```
### 1.3 模板类型参数（type parameter）
> 我们可以把类型参数看作类型说明符。和类型说明符一样，类型参数可以用来指定返回类型或函数的参数类型，以及在函数体内用于变量声明或类型转换  


