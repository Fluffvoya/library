```cpp
int func(int a, int b) { return a + b; }

auto main() -> int {
  using FuncType = std::function<int(int, int)>;
  FuncType f1 = func;
  FuncType f2 = [](int a, int b) { return a + b; };
  f1(1, 2);
  f2(1, 2);
  auto bindFunc = std::bind(f1, std::placeholders::_1, 2);
  bindFunc(1);
  return 0;
}
```