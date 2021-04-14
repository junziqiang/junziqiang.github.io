---
layout:     post
title:      "习惯C++"
date:       2020-12-23 15:00:00
author:     "Junziqiang"
tags:
  - effective c++
  - c++
---


> 1. 视C++为一个语言联邦包括c，面向对象，泛型编程（模板），STL
> 2. 尽量以const，enum，inline代替#define,类似`#define ASPECT_RATIO 1.653`这种，#define不被视为语言的一部分，记号名称`ASPECT_RATIO`可能不会被编译器看见。
```c++
class GamePlayer{
public:
static const int num = 5; //常量声明式，叫做class专属常量
int scores[num];
};
const int GamePlayer::num;//没有的话会报GamePlayer.cc:(.text+0x9)：对‘GamePlayer::num’未定义的引用错误
int main(){
    GamePlayer hello;
    cout<<&(hello.num)<<endl;
}
```
> #define不能创建一个class专属常量，若不支持类内初值，那么可采用enum hack的做法在上述第三行换为`enum {num = 5}`（一个属于枚举类型的数值可权充ints被使用）
>
> 3. STL与const
```c++
    const std::vector<int>::iterator iter = vec.begin();//iter的作用为T* const
    std::vector<int>::const_iterator cIter = vec.begin();//cIter的作用像const T*-------;p
```
>在C++中，mutable也是为了突破const的限制而设置的。被mutable修饰的变量，将永远处于可变的状态，即使在一个const函数中。
>当const和non-const成员函数有着等价的实现时，可以领non-const调用const版本来避免代码重复，使用static_cast和const_cast。
>
>4. 确定对象被使用前已被初始化。1)为内置型对象手工初始化，构造函数使用初始化列。在针对static对象初始化的时候使用local-static对象初始化。
```c++
//c++对定义于不同编译单元内的non-local-static对象的初始化并无明确的定义（次序）
//在函数内，class内以及在file作用域内的static被称为local-static，改成如下形式：
FileSystem& tfs(){
    static FileSystem fs;
    return fs;
}
```
