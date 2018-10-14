# :whale: 面向对象

## :fish: 对象

- :bee: 构造函数
  - 可重载
  - 可带默认参数
- :bee: 析构函数
  - 不可重载
  - 不带参数
- :bee: 拷贝构造函数
  - `public: 类名 (const 类名 &对象名)`

## :fish: 继承

构造函数调用顺序:

基类(多基类按申明的从左至右顺序) ==> 子对象 ==> 自己

## :fish: 多态

c++ 中实现多态的方法有: 虚函数, 抽象类, 覆盖, 模板 (重载与多态无关)

虚函数是实现动态多态的方法.

### :herb: 虚函数

:seedling: 析构函数最好用虚函数, 即加上 `virtual` 关键字. 可以保证用基类指针可以去销毁派生类对象. 而构造函数不能声明为虚函数.

```c++
class className {
  public:
    virtual void Func(int arg);
}
```

### :herb: 纯虚函数

```c++
class className {
  public:
    virtual int pureVirtualFunc(int arg)=0;
}
```

纯虚函数在基类中可以没有实现.

### :herb: 抽象类

有纯虚函数的类就是抽象类, 只能用于派生新类.

### :herb: 动态多态

基类的指针可以动态的调用基类和派生类以及派生类的派生类的虚函数成员函数.

派生类可以覆盖基类的虚函数, 即函数原型相同, 实现不同. 实现了覆盖的派生类调用的是派生类的实现, 而不是基类的实现.

## :fish: 模板

> 模板以变量的数据类型作为参数, 通过改变数据类型可以生成不同的函数和类. 通过改变函数模板的数据类型参数生成的函数叫做模板函数.

模板的数据类型需要在编译期间确定, 属于静态多态.

### :herb: 函数模板

:leaves: 声明:

```c++
template <typename typeA, typename typeB>
typeA func(typeB a, typeB b) {
  // do someting here.
}
```

:leaves: 使用:

```c++
func(doubleA, doubleB); // 模板函数可以隐式确定类型.
```

### :herb: 类模板

:leaves: 声明:

```c++
template <typename typeA, typename typeB>
class A {
  public:
    typeA funcA(typeB);
};

template <typename typeA, typename typeB>
typeA A<typeA, typeB>::funcA(typeB a) {
  // do someting here
}
```

:leaves: 使用

```c++
// 派生
class B:: public A<int, double> {
  // do someting here
}

// 实例化
A<int, double> *a = new A<int, double>;
A<int, double> b;
```
