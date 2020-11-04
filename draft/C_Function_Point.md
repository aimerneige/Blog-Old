# 函数指针

函数指针在大型的 C 语言项目中十分重要，但是学校对它的介绍一带而过，国内一些介绍函数指针的文章十分粗浅甚至存在错误（这里就不点名了），因此博主写了这篇文章介绍函数指针，希望能够帮助一些 C 语言学习者。请不要将本文章转载到 CSDN 平台。

## 函数指针的概念

类似变量，函数也会被分配一段存储空间，这段存储空间的首地址即被称做这个函数的地址。而且函数名表示的就是这个地址。既然是地址，我们就可以定义一个指针变量来存放，这个指针变量就叫做函数指针变量，简称函数指针。

## 函数指针的定义

下面的语句定义了一个指向函数的指针变量 p。其中 `void *`表示返回值，`(*p)` 表示 p 是一个指针变量， `(int, float)` 是函数的参数列表。

```c
void *(*p) (int, float);
```

这样我们就得到了一个指针变量 p，它的类型为 `void * (*)(int, float)`

所以函数指针的定义方法为：

```
函数返回类型 (*指针变量名) (函数参数列表);
```

这里的函数参数列表类似函数声明，只需写出变量类型即可，并不需要定义变量。

即下面的俩种写法等价并且第二种写法中定义的变量并没有任何意义，建议使用第一种写法，不要使用第二种写法。

```c
void *(*p) (int, float);
```

```c
void *(*p) (int a, float b);
```

## 如何对函数指针赋值

```c
int Func(int x); // 函数的声明

int (*p) (int x); // 函数指针的定义

p = &Func; // 函数指针的赋值
p = Func; // 另一种可行的写法
```

注：对于俩种赋值写法的详细说明见[对函数赋值和调用的一些说明](#对函数赋值和调用的一些说明)

## 函数指针的调用

```c
#include <stdio.h>

int max(int a, int b); // 函数声明

int main(int argc, char const *argv[])
{
    int a = 12;
    int b = 32;

    int (*p)(int, int); // 函数指针定义
    p = &max; // 函数指针赋值

    int c = (*p)(a, b); // 函数指针的调用
    printf("%d\n", c); // 输出 `32`

    return 0;
}

// 函数定义
int max(int a, int b)
{
    return a > b ? a : b;
}
```

## 函数指针中要注意的一些内容

1. 与一般的指针不同，函数指针指向代码而不是数据。通常，一个函数指针存储了可执行代码的起始位置。
2. 与普通指针不同，我们使用函数指针时不需要分配和释放内存。
3. 函数名也可以用来获取函数的地址。（详见[对函数赋值和调用的一些说明](#对函数赋值和调用的一些说明)）
4. 类似普通的指针，我们也有函数指针数组。（详见[函数指针数组示例](#函数指针数组示例)）
5. 函数指针可被用于 switch case 结构中，例如[函数指针数组示例](#函数指针数组示例)中的示例程序中，用户可以通过输入 0 ～ 2 来选择不同的操作。
6. 就像普通数据的指针一样，一个函数指针同样可以被用作函数的参数和返回值，例如[函数指针作为参数的示例](#函数指针作为参数的示例)中的程序中 `wrapper()` 函数接受 `void (*fun)()` 作为参数并且执行这个函数。在[函数指针作为参数的应用](#函数指针作为参数的应用)中查看更多内容。
7. 

## 对函数赋值和调用的一些说明

通常，既然是指针赋值，我们会使用取地址操作来获取地址，然后使用 `*` 运算符调用，但是，对于函数来说，直接使用函数名也可以获得地址。即如下俩种写法等价：

```c
p = &Func;
(*p)(2, 3);
```

```c
p = Func;
p(2, 3);
```

编译执行以下程序：

```c
#include <stdio.h>
#include <stdlib.h>

int max(int a, int b);

int main(int argc, char const *argv[])
{
    printf("%p\n",  (max) );
    printf("%p\n", (&max) );
    return 0;
}

int max(int a, int b)
{
    return a > b ? a : b;
}
```

你会发现 `(max)` `(&max)` 拥有相同的值，即直接使用函数名也可以获得地址。

因此，我更推荐第二种没有&和*的写法，更加直观易用。

## 函数指针数组示例

```c
#include <stdio.h>

void add(int a, int b)
{
    printf("Addition is %d\n", a + b);
}

void subtract(int a, int b)
{
    printf("Subtraction is %d\n", a - b);
}

void multiply(int a, int b)
{
    printf("Multiplication is %d\n", a * b);
}

int main()
{
    // fun_ptr_arr is an array of function pointers
    void (*fun_ptr_arr[])(int, int) = {add, subtract, multiply};
    unsigned int ch, a = 15, b = 10;

    printf("Enter Choice: 0 for add, 1 for subtract and 2 for multiply\n");
    scanf("%d", &ch);

    if (ch > 2)
    {
        return 0;
    }

    (*fun_ptr_arr[ch])(a, b);

    return 0;
}
```

## 函数指针作为参数的示例

```c
// A simple C program to show function pointers as parameter
#include <stdio.h>

// Two simple functions
void fun1() { printf("Fun1\n"); }
void fun2() { printf("Fun2\n"); }

// A function that receives a simple function
// as parameter and calls the function
void wrapper(void (*fun)())
{
    fun();
}

int main()
{
    wrapper(fun1);
    wrapper(fun2);
    return 0;
}
```

## 函数指针作为参数的应用

```c
// An example for qsort and comparator
#include <stdio.h>
#include <stdlib.h>

// A sample comparator function that is used
// for sorting an integer array in ascending order.
// To sort any array for any other data type and/or
// criteria, all we need to do is write more compare
// functions. And we can use the same qsort()
int compare(const void *a, const void *b)
{
    return (*(int *)a - *(int *)b);
}

int main()
{
    int arr[] = {10, 5, 15, 12, 90, 80};
    int n = sizeof(arr) / sizeof(arr[0]);

    qsort(arr, n, sizeof(int), compare);

    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    return 0;
}
```

## 如何使用 typedef 定义函数指针类型

与一般的定义不同，函数指针在使用 typedef 重命名时，新的变量名不是在末尾而是类似函数指针变量定义的写法。

下面是一个简单的例子：

```c
typedef int (*type_name) (const void *, double);
```

其中 `int` 为函数返回值类型， `const void *, double` 是函数接受的参数列表，`(*type_name)` 表明定义的是一个函数指针，它的类型为 `int (*)(const void *, double)`，新的类型名为 `type_name`，你可以使用 `type_name` 来定义一个类型为 `int (*)(const void *, double)` 的函数指针。

也就是说，使用定义函数指针类型的格式如下：

```
typedef 函数返回值类型 (*类型名) (函数参数列表)
```

接下来看一下 `qsort()` 函数的源代码：

首先是函数 `qsort()` 的声明：

```c
extern void qsort (void *__base, size_t __nmemb, size_t __size,
		   __compar_fn_t __compar) __nonnull ((1, 4));
```

这里有一个新的数据类型 `__compar_fn_t`，我们知道它是一个函数指针，我们找一下它的定义：

```c
typedef int (*__compar_fn_t) (const void *, const void *);
```

---

建议阅读： <https://www.geeksforgeeks.org/function-pointer-in-c/>
