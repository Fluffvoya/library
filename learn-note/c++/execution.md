在头文件`<execution>`中，有以下几种执行策略：
- `std::execution::seq`：**顺序执行**，与传统的单线程版本行为一致。
- `std::execution::par`：**并行执行**，算法会在多个线程中并行运行，以利用多核CPU。
- `std::execution::par_unseq`：**并行+向量化执行**，不仅多线程并行，还可能在每个线程内使用SIMD指令进行向量化，以获得极致性能。
```cpp
  std::vector<int> data = {3, 4, 6, 1, 2, 5};
  std::sort(std::execution::par, data.begin(), data.end());
  std::println("{}", data);
```
> 如需并行编译时链接tbb
