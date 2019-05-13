# 1. c++ 函数返回值为引用
函数可以当作左值使用。例子：
```c++
double vals[] = {10.1, 12.6, 33.1, 24.1, 50.0};
double& setValues( int i ){
  return vals[i];   // 返回第 i 个元素的引用
}

setValues(1) = 20.23; // 改变第 2 个元素
setValues(3) = 70.8;  // 改变第 4 个元素
```

返回指针也可以作为左值：
```c++
int *f(int p[]){
    return p+1;
}

int main(){
    int *p;
    int a[]={1,2,3};
    *f(a)=5;
    cout<<a[0]<<' '<<a[1]<<' '<<a[2];//输出1 5 3
    return 0;
}
```

# 2. unsigned int 范围是 0~4294967295
int 范围不够时可尝试使用unsigned int

# 3. 继承中的类作用域
- 首先在编译时：根据静态类型进行进行名字查找。
派生类找不到找基类，在继承体系中一直向上找。找到之后，判断是否是虚函数，如果是，那么将在运行时根据动态类型的虚表判断调用版本。
- 派生类中的函数只要名字和基类中的一样，就会覆盖基类函数，即使形参不一致。
- 由于上一点的原因，如果基类有重载函数的情况，当派生类如果定义了一个同名的函数，那么对应函数所有的重载版本都会被派生类的这个同名函数隐藏。为了解决这个问题，可以在派生类中使用using关键字。然后派生类定义特有的函数就可以了。
```C++ 
using base::fun;//在派生类的函数定义处书写
```

# 4. const顶层底层
```c++
const int *p1;//底层const，p1是一个指向const int或者int类型的指针。通过p1不可改变指向变量的值。

int *const p2;//顶层const，p2是一个指向int类型的指针，但p2本身不可改变。

const int t;
const int *p=&t;//当一个变量是const类型时，只可以使用底层const指针指向它。
```

# 5. static函数与static变量
- static定义的函数与变量都属于类，不属于某个对象，所以static函数只能操作static变量，和static函数。普通成员函数与成员变量都不能访问。
- static变量由该类的所有对象共享。
- static型变量只被初始化一次，下次执行初始化语句会直接跳过。
- 普通函数包含一个指向对象的指针，static函数没有。

# 6. 变量自动初始化
- 栈中的变量（函数体中的自动变量）和堆中的变量（动态内存）会保有不确定的值
- 全局变量和静态变量（包括局部静态变量）会初始化为零
```C++
//在函数体中定义
int i;                    // 不确定值
int i = int();            // 0
int *p = new int;         // 不确定值
int *p = new int();       // 0
```
