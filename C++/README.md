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
int 范围不够时可尝试使用unsigned int，int数据与unsigned int数据进行比较时，c++会将int转换成unsigned int 进行比较，转换方式为二进制不变直接改变解读方式为int，这在int为负数时，会出错。在比较时应当用相同类型！！

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
1. c语言中static：持久存储和访问性。
    - static变量存在与**全局&静态区**中（和全局变量一样的位置），内置类型的static变量在main()函数执行前就被初始化，并存在于整个程序运行过程中，static变量定义的位置对应与于其可见的作用域，（eg.仅某个花括号内部可见，仅该文件内可见）。
    - static函数表示该函数仅在该文件内部可见。
2. c++中static：
    - static global variables (deprecated)
    - static free function (deprecated)  //类外函数
    - static local variables --Persistent storage
    - static member variables --Shared by all instances
    - static member function  --Shared by all instances, can only access static member variables and static functions
    - 与内置类型的static变量不同（即在main()函数前就已经初始化完成），本地static对象的初始化在程序第一次执行到初始化语句时才进行构造。全局static对象和内置类型一样在main()函数之前构造。
- static定义的函数与变量都属于类，不属于某个对象，所以static函数只能操作static变量，和static函数。普通成员函数与成员变量都不能访问。
- static变量由该类的所有对象共享。
- static型变量只被初始化一次，下次执行初始化语句会直接跳过。
- 普通函数包含一个指向对象的this指针，static函数没有。

## 5.1 extern
extern用于书写声明变量，表示该变量是外部文件所定义的。
```c
extern int a;//表示变量a是另一个文件定义的全局变量a
void func();//函数声明则不需要extern
extern "c"用于声明以下代码使用c编译器
    - 为了解决C语言编译器与c++编译器对名字的不同处理，而导致的链接问题。
```

# 6. 变量自动初始化
- 栈中的变量（函数体中的自动变量）和堆中的变量（动态内存）会保有不确定的值
- 全局变量和静态变量（包括局部静态变量）没有被显式初始化，或者显式初始化为0，它们就会被放在bss中，如果初始化不为0，就放在data中，bss，data都属于全局&静态区。bss的好处是，仅保存变量占用空间的大小，减小可执行文件大小。其中注意的是在c++中，static对象的始终放在bss中，即第一次执行到语句时才进行初始化。
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
3. 虚表
- 虚表中储存的是这个类的虚函数的函数地址。

# 8. 函数调用
1. 一般函数调用\
编译器判断是否合法。编译时进行绑定。
2. 虚函数调用\
使用指针调用，运行时才能够确定。取决于指针多绑定的对象的真实类型。


虚表存储的是基类的

# 9. 函数声明后+const
表示该函数不会修改成员数据。如果修改，会报错。const对象只能调用带有const的函数

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

```C++
n &= (n – 1)//可以清除二进制表示下的n的最后一个二进制位1，eg.8&7=1000&0111=0000,7&6=0111&0110=0110=6
```

# 13. 进程的地址空间：TEXT，DATA，BSS，HEAP，STACK
- 只读代码区：包括TEXT段，整个程序的代码，以及所有的常量。这部分内存是是固定大小的，只读的。
- 数据区：包括DATA段和BSS段，data：初始化为非零值的全局变量。data段（已手动初始化的数据）为数据分配空间，数据保存在目标文件中。BSS段：初始化为0或未初始化的全局变量和静态变量。bss段（未手动初始化的数据）并不给该段的数据分配空间，只是记录数据所需空间的大小。
- HEAP（堆空间）：动态内存区域，使用malloc或new申请的内存。
- 共享库内存映射区域
- STACK（栈空间）：局部变量、参数、返回值都存在这里，函数调用开始会参数入栈、局部变量入栈；调用结束依次出栈。

# 14. C++中的类型转换
- static_cast：编译期转换，用于内置类型，除了底层const以外，任何具有明确定义的类型转换。用于类层次结构中基类和子类之间指针或引用的转换。
- const_cast：只负责去掉const属性
- reinterpret_cast：低层次上的重新解释，较危险
- dynamic_cast：运行时转换，用于引用和指针，转换类类型，必须含有虚函数否则编译报错。在运行时dynamic_cast根据虚表检查对象的动态类型，若转换不合法则返回空指针。dynamic_cast主要用于类层次间的上行转换和下行转换，还可以用于类之间的交叉转换。如果 downcast 不安全，这个运算符会传回空指针（也就是说，基类指针或者引用没有指向一个派生类对象）。

# 15. C++运行时类型识别RTTI
typeid：返回表达式的类型，具体是返回一个常量对象的引用，该对象是标准库type_info或其public派生类型
dynamic_cast：将基类指针或引用安全的转换成派生类的指针或引用

# 16. static 成员变量
static成员变量需要在类定义体外进行初始化与定义，因为static数据成员独立该类的任意对象存在，它是与类关联的对象，不与类对象关联。例如：上述程序中的c变量的初始化。静态数据成员与类的大小无关，因为静态成员只是作用在类的范围而已。

# 17. static成员函数
- 因为**static成员函数没有this指针**，所以静态成员函数不可以访问非静态成员。
非静态成员函数可以访问静态成员。
- 出现在类体外的函数定义不能指定关键字static
- 由于没有this指针的额外开销，因此静态成员函数与类的全局函数相比速度上会有少许的增长；

# 18. 成员变量的内存顺序
C++中处在同一个访问标识符（指public、private、protected）下的声明的数据成员，在内存中必定保证以其声明顺序出现。而处于不同访问标识符声明下的成员则无此规定。

# 19. 智能指针auto_ptr与unique_ptr
unique_ptr是c++11的新特性，具有如下特点：
1. unique_ptr不可进行operator=复制操作，可以使用移动函数std::move()，返回该对象的右值引用，这样可以调用移动赋值运算符函数，完成移动操作。使用如下：
```c++
    unique_ptr<int> u_p(new int(3));
    unique_ptr<int> u2 = std::move(u_p);
    unique_ptr<int> u3(u2.release());//release() 返回指针
```
2. unique_ptr可以作为容器元素，auto_ptr不可以

# 20. c++ 11 thread库
```C++
#include <iostream> // std::cout
#include <thread>   // std::thread
#include<mutex>

volatile int num = 0;
std::mutex mtx;

void thread_task(int n) {
    for (int i = 0; i < n; i++) {
        std::unique_lock<std::mutex> lck(mtx);
        num++;
    }
}
void thread_task2(int n) {
    for (int i = 0; i < n; i++) {
        std::unique_lock<std::mutex> lck(mtx);
        num++;
    }
}

int main(){
    std::thread t(thread_task, 10000000);
    std::thread t2(thread_task2, 10000000);
    t.join();
    t2.join();
    std::cout << num;
    return 0;
}
```
std::unique_lock可以方便加锁，unique_lock对象被析构时自动unlock对应的mutex，但是比手动mutex.unlock()效率低，所需要的执行时间更长。

执行速度手动unlock() > lock_guard管理 > unique_lock管理

thead0.join()阻塞当前线程，直到线程thread0执行完毕，并且会被当前线程回收资源。然后返回。

thread1.detach()将线程thread0分离，即线程thread0执行完毕由操作系统自动进行资源回收。

# 21. shared_ptr weak_ptr
shared_ptr和weak_ptr共享一个ref_count对象（使用传统指针指向），ref_count里包含两个原子操作的计数变量：_uses, _weaks，分别对应shared_ptr的计数和weak_ptr的计数。当_uses==0时，指向的对象被delete，同时将_weaks引用计数减1(下面解释)，当_weaks==0时，ref_count被delete。shared_ptr和weak_ptr自己本身将在离开作用域后自动析构。更具体解释：由于ref_counter中的_uses和_weaks在初始化时初值均设为1。且在第一个shared_ptr构造完成时，ref_counter就已经构造完成，即_uses==1、_weaks==1。所以后面开始使用weak_ptr时，_weaks的计数数值始终比实际的weak_ptr数量多1，这样可以保证在没有使用weak_ptr的情况下，ref_count不被析构。而_uses因为是由shared_ptr建立的，其满足_uses等于实际shared_ptr数量。当最后一个shared_ptr析构时，_uses减1等于0，管理的对象被析构，同时会将_weaks的数值也减去1，这样_weaks就不再比实际的weak_ptr数量多1，而是与之相等。当最后一个weak_ptr析构时，就可以满足_weaks==0,从而可以析构ref_count。

# 22. C++单例模式
```c++
//懒汉版，延迟初始化
class Singleton{
public:
    // 注意返回的是引用。
    static Singleton& getInstance(){
        static Singleton m_instance;  //局部静态变量
        return m_instance;
    }
private:
    Singleton(); //私有构造函数，不允许使用者自己生成对象
    ~Singleton();
    Singleton(const Singleton& other);
    Singleton& operator=(const Singleton&);
};

//饿汉版本，程序运行一开始就实例化对象
class Singleton
{
public:
	static Singleton& getInstance() {
		return instance;
	}
private:
	static Singleton instance;//静态成员变量
	Singleton();
	~Singleton();
	Singleton(const Singleton&);
	Singleton& operator=(const Singleton&);
}
// initialize defaultly
Singleton Singleton::instance;//在类外初始化
```

# 23. c++ string实现
```C++
#include <utility>
#include <string.h>

class String{
public:
    String(): data_(new char[1]){
        *data_ = '\0';
    }

    String(const char *str): data_(new char[strlen(str) + 1]) {
        strcpy(data_, str);
    }

    String(const String &rhs): data_(new char[rhs.size() + 1]) {
        strcpy(data_, rhs.c_str());
    }
    /* Delegate constructor in C++11
    String(const String& rhs): String(rhs.data_) {
    }
  */

    ~String(){
        delete[] data_;
    }

    /* Traditional:
  String& operator=(const String& rhs)
  {
    String tmp(rhs);
    swap(tmp);
    return *this;
  }
  */
    String &operator=(String rhs){// yes, pass-by-value
        swap(rhs);
        return *this;
    }

    // C++ 11
    String(String &&rhs): data_(rhs.data_) {
        rhs.data_ = nullptr;
    }

    String &operator=(String &&rhs) {
        swap(rhs);
        return *this;
    }

    // Accessors
    size_t size() const {
        return strlen(data_);
    }

    const char *c_str() const {
        return data_;
    }

    void swap(String &rhs){
        std::swap(data_, rhs.data_);
    }

private:
    char *data_;
};
```

# 24. c++ 0x move semantics
解决问题：两个。
1. 函数返回临时对象的拷贝问题
2. 其它只想移动的地方，结合move()函数使用，其返回对象的右值引用方便调用 move constructor & move assignment operator。
    - move()返回一个对象的右值引用

# C语言文件编译到可执行文件的过程
.c -> 预处理 -> .i -> 编译器 -> .s -> 汇编器 -> .o -> 链接器 -> 可执行文件
- 预处理(Preprocessing)：include 宏（macro）
- 编译(Compilation)：编译成汇编语言
- 汇编(Assemble)：转为机器码
- 链接(Linking)：库文件链接成为可执行文件

# struct 和 class区别
class和struct做类型定义时只有两点区别：
1. 默认继承权限。如果不明确指定，子类是class的继承按照private继承处理，子类是struct的继承按照public继承处理；
2. 成员的默认访问权限。class的成员默认是private权限，struct默认是public权限。

# inline函数
1. 在调用处展开函数，节省函数调用入栈出栈的开销。
1. 代码膨胀，消耗硬盘与内存空间
2. inline函数无法随着函数库升级而升级
3. inline和普通函数一样会做安全检查和自动类型转换
4. 在类中声明并完成定义的成员函数，默认是inline函数。类内声明类外定义的则默认不是inline。
5. 虚函数也可以定义为inline函数，（在类中定义也是默认具有inline属性的），但是只有在编译期可以确定调用的函数版本时才可能会inline。？
   
# c++四种分配内存的方法整理
1. calloc 函数： void *calloc(unsigned int num, unsigned int size)
按照所给的数据个数和数据类型所占字节数，分配一个 num * size 连续的空间。
calloc申请内存空间后，会**自动初始化内存空间为 0**。

2. malloc 函数： void *malloc(unsigned int size)
在内存的动态分配区域中分配一个长度为size的连续空间，如果分配成功，则返回所分配内存空间的首地址，否则返回NULL，**申请的内存不会进行初始化，是一堆随机值**。

3. realloc 函数： void *realloc(void *ptr, unsigned int size)
更改以前分配的内存长度（增加或减少）。当增加长度时，可能需将以前分配区的内容移到另一个足够大的区域，而新增区域内的初始值则不确定。

4. new是动态分配内存的运算符，自动计算需要分配的空间，在分配类类型的内存空间时，同时调用类的构造函数，对内存空间进行初始化，即完成类的初始化工作。动态分配内置类型是否自动初始化取决于变量定义的位置，在函数体外定义的变量都初始化为0，在函数体内定义的内置类型变量都不进行初始化。

# explicit
- 指定构造函数或转换函数 (C++11 起)或推导指引 (C++17 起)为显式，即它不能用于隐式转换和复制初始化。
- 隐式转换指的是，直接用 ”=值“ 或者 “={}” 的方式初始化。

# 纯虚函数
```c++
virtual int A() = 0;
```
- 要求子类必须实现该函数，（子类可以继续不实现，但继续继承下的后面一定要有一个实现。）
- 含有纯虚函数的类叫抽象类。

# RTTI运行时类型信息
- typeid 有虚指针返回动态类型，没有则返回静态类型。
- type_id 返回一个 type_info 对象的引用

# assert()
断言，是宏，而非函数。assert 宏的原型定义在 <assert.h>（C）、\<cassert>（C++）中，其作用是如果它的条件返回错误，则终止程序执行。可以通过定义 NDEBUG 来关闭 assert，但是需要在源代码的开头，include <assert.h> 之前。
```c++
#define NDEBUG          // 加上这行，则 assert 不可用
#include <assert.h>

assert( p != NULL );    // assert 不可用
```

# std::cout
在执行cout语句时，先把插入的数据顺序存放在输出缓冲区中，直到输出缓冲区满或遇到cout语句中的endl(或'\n',ends,flush)为止，此时将缓冲区中已有的数据一起输出，并清空缓冲区。
> 再使用cout调试的时候，记得加上endl，不然可能会出现程序运行到了出错点并退出，但是前面的cout还没有输出到屏幕上。
