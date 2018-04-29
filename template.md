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

### 1.1.1 实例化函数模板
> 当我们调用一个函数模板时，编译器（通常）用函数实参来为我们推断模板实参
>
编译器会推断出模板实参为int，并将int绑定到模板参数T上  
然后编译器会实例化一个int版本的compare
```c++
std::cout << compare(1,0) << std::endl;	
```
### 1.1.2 模板类型参数（type parameter）
> 我们可以把类型参数看作类型说明符。和类型说明符一样，类型参数可以用来指定返回类型或函数的参数类型，以及在函数体内用于变量声明或类型转换  
>

>类型参数前必须使用关键字class或typename，这两个关键字的含义相同，可以互换使用，一个模板参数列表中可以同时使用这两个关键字
>
```c++
template <typename T, class U>
T example_template(const T &, const U &);
```
### 1.1.3 非类型模板参数（nontype parameter）
> 一个非类型模板参数表示一个值而非一个类型，我们通过一个特定的类型名而非关键字class或typename来指定非类型参数
```c++
template<typename T, unsigned NUM>
T example_template(const char s[NUM])
{
	std::cout << NUM << std::endl;
}
```
> 一个非类型参数可以是一个整形()，或者是一个指向对象或函数类型的指针或（左值）引用 
>
> 绑定到非类型整形参数的实参必须是一个常量表达式，绑定到指针或引用的非类型参数的实参必须具有静态生存期。我们不能用一个普通（非静态）局部变量
或动态对象作为指针或引用非类型模板参数的实参。指针参数也可以用nullptr或一个值为0的常量表达式来实例化
>
### 1.1.4 内联和constexpr的函数模板
```c++
template <typename T, class U>
inline T example_template(const T &, const U &);
```
### 1.1.5 编写与类型无关的代码
> 模板程序应该尽量减少对实参类型的要求。通过将函数参数设为const的引用，我们保证了函数可以用于不能拷贝的类型。在处理大对象，这种设计策略还能提高函数运行效率
```c++
template <typename T>
int compare(const T &lhs, const T &rhs)
{
	if (lhs < rhs) return -1;
	if (rhs < lhs) return 1;
	return 0;
}
```
### 1.1.6 模板编译
> 当编译器遇到一个模板定义时，编译器并不生成代码。只有当我们实例化出一个模板的特定版本（使用模板），编译器才会生成代码

>编译器只要知道函数的声明，就能调用函数（运行时会报错： 无法解析的外部符号 "return_type cdecl function_name(params)" ，该符号在函数 main 中被引用）  
编译器需要知道类的定义，才能使用一个类类型对象，但成员函数不必已经出现  
编译器需要掌握函数模板或类模板成员函数的定义，才能生成一个实例化版本。  
所以函数模板和类模板成员函数的定义通常放在头文件中  
### 1.1.7 编译器报错
> 1  编译模板本身，编译器检查语法错误  
2  编译器遇到模板使用时，编译器通常会检查模板实参数目是否正确，类型是否匹配
3  编译器在模板实例化时，才能发现类型相关的错误，大部分报错也是在这个阶段报告的

### 1.2 类模板（class template）
### 1.2.1 Template Class的声明
```c++
template <typename T> class vector;
}
```
### 1.2.2 Template Class的定义
```c++
template <typename T>
class vector
{
public:
	void push_back(T const&);
	void clear();				
	
private:
	T* elements;
};
```
### 1.2.3 Template Class的实例化
> 我们把通过类型绑定将模板类变成“普通的类”的过程，称之为模板实例化（Template Instantiate）
```c++
vector<int> intArray;
vector<float> floatArray;

intArray.push_back(5);
floatArray.push_back(3.0f);
```
> 变量定义的过程可以分成两步来看：第一步，vector<int>将int绑定到类模板vector上，获得了一个“普通的类vector<int>”;第二步通过“vector”定义了一个变量。 
```c++
vector unknownVector; // 错误示例
```
### 1.2.4 模板类的成员函数定义  

> 类模板的成员函数和函数模板一样，被第一次调用时才会进行实例化
```c++
template <typename T>
void vector<T>::clear()		// 类外定义
{
	// Function body
}
```
> 当我们使用一个类模板时必须提供模板实参，但有一个例外，在类模板自己的作用域中，我们可以直接使用模板名而不提供实参
```c++
template <typename T>
class vector
{
public:
	vector& operator+(const vector &rhs); //类模板作用域中我们可以直接使用模板名vector而不是vector<T>
	void push_back(T const&);
	void clear();				
	
private:
	T* elements;
};

//类外定义
//返回类型不在类模板作用域内所以要用vector<T>
//形参rhs在类模板作用域内所以只用vector就行
template<typename T>
vector<T>& vector<T>::operator+(const vector &rhs) 
{
	// Functuon body
}
```
### 1.2.5 类模板和友元
#### 一对多关系
> 如果一个类模板包含一个非模板友元，则友元可以访问所以模板实例
```c++
template <typename T>
class vector
{
public:
	friend get_vecotr_elems_pointer(const vector&);
	vector& operator+(const vector &rhs); //类模板作用域中我们可以直接使用模板名vector而不是vector<T>
	void push_back(T const&);
	void clear();				
	
private:
	T* elements;
};
```
## 2 模板实参推断（template argument deduction）
### 2.1 类型转换与模板类型参数
> 对于模板参数类型，除了以下情况，编译器通常不会对实参进行类型转换，而是直接生成一个新的模板实例 ，比如：算数转换、派生类向基类转换，用户自定义转换
> 顶层const无论在形参中还是在实参中都被忽略  
> 可以将一个非const对象的引用（或指针）转换成const对象的引用（或指针）  
> 数组或函数指针转换
> 注意，如果函数参数不是模板参数，则对实参进行正常的类型转换
```c++
template <typename T> T example_obj(T, T)
template <typename T> T example_ref(const T&, const T&)
template <typename T> T example_com(T, int)
int var = 1;
const int cvar = 2;
example_obj(var, cvar);	//调用example_obj(int, int),顶层const被忽略
example_ref(var, cvar);  //调用example_ref(const int&, const int&),非const对象的引用转换成const对象的引用

int mat_lhs[10];
int mat_rhs[22];
//形参为对象，数组大小无关，都将转换成int*指针
example_obj(mat_rhs, mat_lhs);	//调用example_obj(int*, int*),数组向指针转换
//形参为引用，数组不会转换成指针，所以数组大小也算进类型???????待补充，数组大小相同也不正确
example_ref(mat_rhs, mat_lhs);	//错误：数组类型不匹配example_ref(int[10], int[22])
争取
long lvar = 3;
std::string svar = " ";
example_obj(var, lvar);	//错误：模板参数转换无法进行算数转换long->int,无法实例化example_obj(int, long),两个实参类型要相同（或是转换后要相同）
example_com(svar, lvar);	//正确，调用example_com(string, int),普通参数可以进行算术转换long->int, T为string
```
### 2.2 函数模板的显式实参
> 某些情况下，编译器无法推断出模板实参类型，我们希望允许用户控制模板实例化  
> 当函数返回类型与参数列表中任何模板参数类型都不同时，编译器无法推断出返回类型,此时必须要为返回类型提供一个显示模板参数
```c++
template <typename T1, typename T2, typename T3>
T1 example(T2, T3);

auto result = example(1, 2);	//错误：无法推断出返回类型T1的类型
auto result = example<int>(1, 2);	//正确：调用example<int, const int, const int>
```
>对于模板类型参数已经显式指定了的函数参数，可以进行正常的类型转换
```c++
template <typename T> T example_obj(T, T)

long lvar = 1;
int var = 2;

example_obj(var, lvar);	//错误：模板参数转换无法进行算数转换long->int
example_obj<int>(var, lvar);	//正确：模板类型参数已经显式指定了的函数参数可以进行算数转换long->int，example_obj(int, int)
example_obj<long>(var, lvar);	//example_obj(long, long)
```
### 2.3 函数指针与实参推断
> 当我们用函数模板给函数指针初始化或赋值时，编译器将用函数指针类型来推断模板实参
```c++
template <typename T> int example_ref(const T&, const T&);
//使用example_ref<int, int>初始化pfun
int (*pfun)(const int&, const int&) = example_ref;
```
> 当参数是一个函数模板实例的地址时，程序上下文必须满足：对每个模板参数能唯一确定其类型或值
```c++
//func的重载版本
void func(int(*)(const int&, const int&))
void func(int(*)(const std::string&, const std::string&))
//错误：对函数指针类型的形参初始化时不能确定唯一实例化版本，编译失败
func(example_ref)	//example_ref<std::string>还是example_ref<int>?
```
> 我们可以通过使用显式模板实参来消除func调用的歧义
```c++
//正确：传递example_ref<std::string>
func(example_ref<std::string>)
```
### 2.4 引用与模板实参推断
### 2.4.1 从左值引用函数参数推断类型
> 当一个函数参数时模板类型参数的一个左值引用时(T&),我们只能传给它一个左值，实参可以是const类型，也可以不是。如果是，T会被推断为const类型
```c++
template <typename T>
void example_ref_left(T&);

example_ref_left(var)	//T:int
example_ref_left(cvar)	//T:const int
example_ref_left(5)	//错误：传递给T&的实参必须时左值
```
> 如果一个函数参数的类型为const T&,那么我们可以传递给它任何类型的实参——对象（const或非const），临时对象或字面值常量  
> 当传入实参本时const时，T的类型不会时一个const类型，因为const已经是函数参数类型的一部分了，所以它不会也是模板参数类型的一部分  
> 函数参数类型为const &T,模板参数类型为T
```c++
template <typename T>
void example_ref_Cleft(const T&);
//每个函数参数都为const int&
example_ref_Cleft(var)	//T:int
example_ref_Cleft(cvar)	//T:int
example_ref_Cleft(5)	//T:int
```
### 2.4.1 从右值引用函数参数推断类型
> 当一个函数参数是一个右值引用（T&&），我们可以传递给它一个右值，推断过程类似于左值
```c++
template <typename T>
void example_ref_right(T&&);

example_ref_right(5)	//T:int
```
### 2.5 模板特例化
> 当我们不希望使用模板版本时，可以定义类或函数模板的一个特例化版本
```c++
//定义模板
template <typename T>
void example_ref_Cleft(const T&)
{
	//do something
}
//特例化int
template <>
void example_ref_Cleft(const int&)
{
	//do something else
};
```






