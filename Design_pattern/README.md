# 1. 单例模式
```C++
//singleton
class Singleton
{
public:
    static Singleton& getinstance() {
        static Singleton instance;//c++11以后staic可以保证在多线程环境下，语句只会被执行一次。
        return instance;
    }

private:
    Singleton();
    Singleton(const Singleton&);
    Singleton& operator=(const Singleton&);
};
```
```c++
class Singletonlanhan {
public:
    static Singletonlanhan& getinstance() {
        return instance;
    }
private:
    static Singletonlanhan instance;//
    Singletonlanhan();
    Singletonlanhan(const Singletonlanhan&);
    Singletonlanhan& operator=(const Singletonlanhan&);
};
Singleton Singleton::instance;//在类外初始化
```

# 2. 工厂模式
```C++
//简单工厂模式
typedef enum ProductTypeTag{
    TypeA,
    TypeB,
    TypeC
}PRODUCTTYPE;

class Product//产品抽象基类{
public:
    virtual void Show() = 0;
};

class ProductA : public Product{
public:
    void Show() {
        cout<<"I'm ProductA"<<endl;
    }
};


class ProductB : public Product{
public:
    void Show() {
        cout<<"I'm ProductB"<<endl;
    }
};

class ProductC : public Product{
public:
    void Show() {
        cout<<"I'm ProductC"<<endl;
    }
};

class Factory{//工厂类
public:
    Product* CreateProduct(PRODUCTTYPE type) {
        switch (type) {
            case TypeA:
                return new ProductA();
    
            case TypeB:
                return new ProductB();
    
            case TypeC:
                return new ProductC();
    
            default:
                return NULL;
        }
    }
};
```