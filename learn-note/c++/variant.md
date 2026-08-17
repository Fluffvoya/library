```cpp
#include <print>
#include <variant>

class Foo1 {
public:
  Foo1() { std::println("Foo1 constructor"); }
  ~Foo1() { std::println("Foo1 destructor"); }
};

class Foo2 {
public:
  Foo2() { std::println("Foo2 constructor"); }
  ~Foo2() { std::println("Foo2 destructor"); }
};

struct Visitor {
  void operator()(int i) { std::println("int"); }
  void operator()(const Foo1 &) { std::println("Foo1"); }
  void operator()(const Foo2 &) { std::println("Foo2"); }
};

auto main() -> int {
  std::variant<int, Foo1, Foo2> v;
  v = 1;
  std::visit(Visitor(), v);
  v = Foo1();
  std::visit(Visitor(), v);
  v = Foo2();
  std::visit(Visitor(), v);
  return 0;
}
```