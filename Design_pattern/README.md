# 1. 单例模式
```C++

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