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
>

>类型参数前必须使用关键字class或typename，这两个关键字的含义相同，可以互换使用，一个模板参数列表中可以同时使用这两个关键字
>
```c++
template <typename T, class U>
T example_template(const T &, const U &);
```
### 1.4 非类型模板参数（nontype parameter）
> 一个非类型模板参数表示一个值而非一个类型，我们通过一个特定的类型名而非关键字class或typename来指定非类型参数
```c++
template<typename T, unsigned NUM>
T example_template(const char s[NUM])
{
	std::cout << NUM << std::endl;
}
```
> 一个非类型参数可以是一个整形，或者是一个指向对象或函数类型的指针或（左值）引用  
> 绑定到非类型整形参数的实参必须是一个常量表达式，绑定到指针或引用的非类型参数的实参必须具有静态生存期。我们不能用一个普通（非静态）局部变量
或动态对象作为指针或引用非类型模板参数的实参。指针参数也可以用nullptr或一个值为0的常量表达式来实例化
>
