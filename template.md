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
