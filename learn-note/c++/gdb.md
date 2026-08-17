## 启动
```bash
gcc -g mian.c
gdb ./a.out
```
## 断点
函数
```bash
b main
```
行数
```bash
b 3
```
## 源码信息
```bash
list
```
## 下一步
```bash
n
```
## 运行
```bash
r
```
## 查看断点
```bash
info b
```
## 查看
```bash
p var
```
## 步入
```bash
step
```
## Watchpoint追踪
```bash
watch 地址
```
后续地址指向的值改变会打印
