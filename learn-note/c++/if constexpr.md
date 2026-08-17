```cpp
template<typename T>  
auto print_type_info(const T& t) {  
    if constexpr (std::is_integral<T>::value) {  
        return t + 1;  
    } else {  
        return t + 0.001;  
    }  
}
```