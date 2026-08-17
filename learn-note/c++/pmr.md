```cpp
#include <array>  
#include <cstddef>  
#include <memory_resource>  
#include <vector>  
  
std::array<std::byte, 1024> buffer;  
std::pmr::monotonic_buffer_resource pool{buffer.data(), buffer.size()};  
  
std::pmr::vector<int> v{&pool}; // 从栈上的 buffer 分配，而非堆  
for (int i = 0; i < 5; ++i) v.push_back(i);
```