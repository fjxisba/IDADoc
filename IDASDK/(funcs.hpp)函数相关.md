# 函数相关

首先要说明的就是，funcs.hpp所中围绕的主体func_t结构，指的是IDA主程序在汇编代码级别上提取的信息，这些信息是有限的，而IDA F5插件则是在这些信息基础之上更进一步的语义解析，二者有所不同。



遍历IDA中所有的函数，代码如下:

```c++
#include <ida.hpp>
#include <idp.hpp>
#include <loader.hpp>
#include <kernwin.hpp>
#include <funcs.hpp>

bool idaapi run(size_t)
{
	size_t funcCount= get_func_qty();
	for (unsigned int idx = 0; idx < funcCount; ++idx)
	{
		func_t* pFunc = getn_func(idx);

		qstring FuncName;
		get_func_name(&FuncName, pFunc->start_ea);

		//库函数
		if ((pFunc->flags & FUNC_LIB) != 0)
		{
			msg("LibFunc:%a--%s\n", pFunc->start_ea, FuncName.c_str());
			continue;
		}
		msg("Func:%a--%s\n", pFunc->start_ea, FuncName.c_str());
	}
	
	return true;
}
```

------

**get_func_cmt**/**set_func_cmt**函数用于获取和设置函数的注释。

------

