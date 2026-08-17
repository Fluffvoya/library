用于在普通引用无法使用时创建引用对象
## std::bind
```cpp
auto f=std::bind([](int ,int & ,const int &){},x,std::ref(y),std::cref(z));
```
## std::thread
同理

> cref 创建行为类似const T&的对象，无法改变值