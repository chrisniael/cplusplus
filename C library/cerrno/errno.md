宏

# errno

`int`

#### 最后一个错误号

这个宏被展开成为一个可改变的 _int_ 型的左值。因此，它可以被程序读取和改变。

程序开始的时候，_errno_ 被设置成为0，标准 C 库的任何函数可以把它改变成为任何非0的值，通常用来通知特定类型的错误（_errno_ 一旦改变，则没有库函数会把它设置为0）。 

程序同样可以改变 _errno_ 的值。事实上，如果它的值是打算用来检查库函数调用后的错误的话，那么它应该在函数被调用前被设置为0（因为之前调用的任何函数都可能会改变它的值）。

声明 _errno_ 的头文件 ([\<cerrno\>](README.md)) 中至少还定义了下面这些非0的常量宏：

宏     | 何时 _errno_ 会被设置
------ | ---------------------
EDOM   | __定义域错误__ ：某些数学函数仅仅能被用于某些特定的值，这些值被称为定义域，例如平方根函数仅仅能被用于非负的数字，因此当 [sqrt](../cmath/sqrt.md) 使用负数作为参数调用时，会设置 _errno_ 为 _EDOM_ 。
ERANGE | __值域错误__ ：用变量表示的值的范围是有限的。例如，像 [pow](../cmath/pow.md) 这样的数学函数很容易超出浮点变量能表示的范围，还有像 [strtod](../cstdlib/strtod.md) 这样的函数，可能会遇到数字序列长于值表示的范围。这些情况下，_errno_ 被设置为 _ERANGE_ 。
EILSEQ | __非法序列__ ：多字节字符序列可能会有一个有效序列的有限集。当一个多字节字符集被像 [mbrtowc](mbrtowc.md) 这样的函数转化时，如果遇到无效序列，则 _errno_ 会被设置为 _EILSEQ_ 。

标准库的函数可能会设置 [error](errno.md) 为任何值（不单单是上面列出的可移植的值）。某些特定的库实现可能会这个头文件中定义额外的名字。

C++ 11 通过包含很多可以在 POSIX 环境中获得的名字，扩展了必须定义在这个头文件中的基本的值集合，可移植的 [errno](errno.md) 值的数量增加到 78 个。详细列表，请查看 [errc](../../Other/system_error/errc.md)。

与 [errno](errno.md) 值相关的特定错误消息可以使用 [strerror](../cstring/strerror.md) 获得，或使用 [perror](../cstdio/perror.md) 直接打印。

在 C++ 中，[errno](errno.md) 总是被定义成一个宏，但是在 C 中可能会使用外部链接实现成一个 _int_ 对象。


## 数据竞争

支持多线程的库应该在每个线程基础上实现 _errno_ ：每个线程拥有它自己的局部 _errno_ 。依从 C11 和 C++ 11 标准，在库中这是一个必要条件。