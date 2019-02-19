[When to use reinterpret_cast?](https://stackoverflow.com/questions/573294/when-to-use-reinterpret-cast)

## 问题

我对于 `reinterpret_cast vs static_cast` 的使用有点困惑。我已知的普遍规则是，当类型可以在编译时通过 `static` 关键字转换时，应该选择使用 `static_cast`。这是 C++ 内部隐式类型转换时所使用的。

`reinterpret_cast` 在两个场景下使用，将一个整数类型转换指针类型，或反过来；将一个指针类型转换为另一个指针类型。我知道的一般说法，这是无法移植并应该避免的。

我困惑的点在于，当我用 C 调用 C++ 时，需要保存 C++ 对象，一般保存成 `void*` 类型。那我应该用哪种转换来转换 `void*`  跟类类型呢？

`static_cast` 和 `reinterpret_cast` 的用法我都见过了吗？因为 `static` 转换发生在编译时，所以它更好吗？还是说应该用 `reinterpret_cast` 来讲一个指针类型转换成另一个指针类型。

## 回答

C++ 标准可以保证如下：

`static_cast` 在指针与 `void*` 互相转换时保留地址。也就是说，在如下代码中，a，b 和 c 三者都指向同样的地址：

``` c++
int* a = new int();
void* b = static_cast<void*>(a);
int* c = static_cast<int*>(b);
```

`reinterpret_cast` 只保证当你将一个指针转换成别的类型，再用 `reinterpret_cast`  转换回来，你能得到跟之前一样的值。所以在如何代码中：

``` c++
int* a = new int();
void* b = reinterpret_cast<void*>(a);
int* c = reinterpret_cast<int*>(b);
```
a 和 c 是一样的值，但是 b 的值是不确定的。（一般来说，b 跟 a，c 指向一样的地址，但是这在 C++ 标准中并没有明确指定，在有更复杂的内存系统的机器上，这并不一定为真。）

因此，对于和 `void*` 互相转换的场景，应该选择 `static_cast`。

## 词汇

* internally: adv.内部的；体内的
* applicable: adj.适用；合适
