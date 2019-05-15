https://harttle.land/tags.html#C++
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

# 7. 虚函数
1. 虚表：
    - 虚表对应类
    - 虚表存储本类可以调用的所有虚函数的地址，包括：继承而来的虚函数、本类定义的虚函数。虚函数在虚表中的排列顺序是先父类，再子类，父类按定义顺序排列、子类按定义顺序排列。其中如果继承而来的虚函数被重写，那么重写的函数放在被重写的父类函数原本的位置上。即：
2. 虚指针
    - 虚指针对应具体对象，存储在对象存储内存的首地址处，本身是一个指针，指向虚表的位置。

    eg分析:
    
        class father{};
        
        class son{};
        

# 8. 函数调用
1. 一般函数调用\
编译器判断是否合法。编译时进行绑定。
2. 虚函数调用\
使用指针调用，运行时才能够确定。取决于指针多绑定的对象的真实类型。


虚表存储的是基类的

# 9. 函数声明后+const
表示该函数不会修改成员数据。如果修改，会报错。

# 10. 数据对齐
- short(2bytes),地址必须是2的倍数，即2进制下的地址最后一位必须是0。其它占用空间更大的数据类型的地址必须是4的倍数，即二进制表示下的地址最后两位必须都是0。char一个字节,没有限制，结构体变量的起始地址能够被其最宽的成员大小整除。
- 在以上规则下:对于结构体的内存对齐，默认结构体内部按照最宽的变量长度对齐，使用pragma pack(n)可以对其对齐长度进行限制，pragma pack()默认为8，struct对齐长度不能超过pragma pack(n)设置的长度，不超过按照最宽变量对齐，超过则按照pragma pack设置的宽度进行对齐.

# 11. delete两次指针
delete不改变指针的值，第二次delete会发生未定义的情况（VS里运行时会抛出异常）。所以最好在一次delete之后给指针赋值为nullptr。

# 12. 位运算
异或^ :与全0异或原值不变，与全1异或，原值取反。

与& ：和全0与操作原值全变为0，和全1与操作，原值不变

或| ：和全0或操作原值不变，和全1或操作，原值全变为1

移位操作最好对unsigned int进行。因为对int操作会有符号位问题，有的编译器会报错，而VS编译器会对负数右移最左补1正数右移最左补0。unsigned char也可以

```
n &= (n – 1)//可以清除二进制表示下的n的最后一个二进制位1，eg.8&7=1000&0111=0000,7&6=0111&0110=0110=6
```
