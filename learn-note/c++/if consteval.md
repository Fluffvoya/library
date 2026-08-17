```cpp
int func(int val){
	if consteval{
		return val;//如果是编译期求值走这里
	}else{
		return val+1;//如果是运行时求值走这里
	}
}
```