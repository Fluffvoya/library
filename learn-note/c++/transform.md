```cpp
auto main() -> int {
  std::vector<int> v = {1, 2, 3, 4};
  // 使用back_inserter
  std::vector<int> result;
  std::transform(v.begin(), v.end(), std::back_inserter(result),
                 [](int i) { return i * i; });
  std::println("result:{}", result);

  // 预先扩容
  std::vector<int> result2;
  result2.resize(v.size());

  std::transform(v.begin(), v.end(), result2.begin(),
                 [](int i) { return i * i; });

  std::println("result2:{}", result2);
}
```