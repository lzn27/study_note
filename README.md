# c++ 函数返回值为引用
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