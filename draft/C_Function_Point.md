# 函数指针

## 函数指针的定义

类似变量，函数也会被分配一段存储空间，这段存储空间的首地址即被称做这个函数的地址。而且函数名表示的就是这个地址。既然是地址，我们就可以定义一个指针变量来存放，这个指针变量就叫做函数指针变量，简称函数指针。

## 函数指针的定义

下面的语句定义了一个指向函数的指针变量 p。其中 `void *`表示返回值，`(*p)` 表示 p 是一个指针变量， `int, float` 是函数的参数列表。

```c
void *(*p)(int, float);
```

这样我们就得到了一个指针变量 p，它的类型为 `void * (*)(int, float)`

所以函数指针的定义方法为：

```
函数返回类型 (*指针变量名) (函数参数列表);
```

## 如何对函数指针赋值

```c
int Func(int x); // 函数的声明

int (*p) (int x); // 函数指针的定义

p = &Func; // 函数指针的赋值
p = Func; // 另一种可行的写法
```

## 函数指针的调用

```c
#include <stdio.h>

int max(int a, int b); // 函数声明

int main(int argc, char const *argv[])
{
    int a = 12;
    int b = 32;

    int (*p)(int a, int b); // 函数指针定义
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
2. 与普通指针不同，我们不使用函数指针来分配和释放内存。
3. 函数名也可以用来获取函数的地址。（详见[对函数赋值和调用的一些说明](#对函数赋值和调用的一些说明)）
4. 类似普通的指针，我们也有函数指针数组。（详见[函数指针数组示例](#函数指针数组示例)）
5. 函数指针可被用于 switch case 结构中，例如[函数指针数组示例](#函数指针数组示例)中的示例程序中，用户可以通过输入 0 ～ 2 来选择不同的操作。
6. 就像普通数据的指针一样，一个函数指针同样可以被用作函数的参数和返回值，例如[函数指针作为参数的示例](#函数指针作为参数的示例)中的程序中 `wrapper()` 函数接受 `void (*fun)()` 作为参数并且执行这个函数。在[函数指针作为参数的应用](#函数指针作为参数的应用)中查看更多内容。
7. 

## 对函数赋值和调用的一些说明

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
int compare (const void * a, const void * b) 
{ 
return ( *(int*)a - *(int*)b ); 
} 

int main () 
{ 
int arr[] = {10, 5, 15, 12, 90, 80}; 
int n = sizeof(arr)/sizeof(arr[0]), i; 

qsort (arr, n, sizeof(int), compare); 

for (i=0; i<n; i++) 
	printf ("%d ", arr[i]); 
return 0; 
}
```


建议阅读： <https://www.geeksforgeeks.org/function-pointer-in-c/>
