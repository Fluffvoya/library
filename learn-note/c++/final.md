```cpp
struct Base {  
    virtual void foo() final;  
};  
struct SubClass1 final: Base {  
}; // 合法  
  
struct SubClass2 : SubClass1 {  
}; // 非法, SubClass1 已 final  
  
struct SubClass3: Base {  
    void foo(); // 非法, foo 已 final  
};
```