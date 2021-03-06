## 1 C# struct
c#的结构是值类型，可以用new进行构造函数的初始化，但是内存并不分配在堆上，而是在栈上，如果是作为其它类的一部分，则称为存储为内联。struct的生存期的限制与简单的数据类型一致。

## 2 abstruct virtual
包含抽象函数的类必须是抽象类，所以抽象函数所在的类不能被实例化。而虚函数所在的类可以。抽象函数不能直接实现，必须在非抽象的派生类中重写。虚函数可以直接实现，也可以在派生类中重写。

## 3 高性能C#Tip
1. 避免装箱与拆箱，所以尽量使用Generic(泛型)命名空间中的容器，比如：System.Collections.Generic.List<T>，少用非泛型，如System.Collections.ArrayList.
2. 字符串连接，在一个字符串上需要进行很多或未知个数的修改操作时，使用StringBuilder。stringBuilder的capacity容量根据不同的实现会初始化为一个默认值，当Length和capacity相等时，capacity会扩展为之前的2倍，并伴有新的内存分配。当扩展到maxcapacity定义的内存大小，并且length增长到最大还要add的话，就会引发异常。
3. 不要使用空的析构函数。空的析构函数会在终结器里面生成一个入口，垃圾收集器在处理finalize queue时会使用这个入口去执行析构函数，降低性能。