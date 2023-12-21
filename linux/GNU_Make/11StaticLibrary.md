# Makefile 静态库编译

## 程序
>add.hpp
```c++
#ifndef ADD_HPP
#define ADD_HPP
int add(int a, int b);

#endif // ADD_HPP
```
>add.cpp
```c++
int add(int a, int b)
{
    return a+b;
}
```
>minus.hpp
```c++
#ifndef MINUS_HPP
#define MINUS_HPP
int minus(int a, int b);

#endif // MINUS_HPP
```
>minus.cpp
```c++
int minus(int a, int b)
{
    return a-b;
}
```
>main.cpp
```c++
#include <stdio.h>
#include "add.hpp"
#include "minus.hpp"

int main()
{
    int a=10; int b=5;
    int res = add(a, b);
    printf("a + b = %d\n", res);
    res = minus(a, b);
    printf("a - b = %d\n", res);

    return 0;
}
```


## 编译过程  
- 源文件[.c/cpp] -> Object文件[.o]
    ```
    g++ -c [.c/cpp][.c/cpp]... -o [.o][.o]... -I[.h/hpp] -g
    ```
- Object文件[.o] -> 静态库文件[lib库名.a]
    ```
    ar -r [lib库名.a] [.o][.o]...
    ```
- main 文件[.c/cpp] -> Object 文件[.o]
    ```
    g++ -c [main.c/cpp] -o [.o] -I[.h/hpp] 
    ```
- 链接 main 的 Object 文件与静态库文件 [lib库名.a]
    ```
    g++ [main.o] -o [可执行文件] -l[库名] -L[库路径]
    ```
## Makefile

```makefile
cpp_srcs := $(filter-out src/main.cpp,$(shell find src -name *.cpp))
cpp_objs := $(patsubst src/%.cpp,objs/%.o,$(cpp_srcs))

include_paths := ./include \

library_paths := ./lib \

linking_libs := xxx \

I_options := $(include_paths:%=-I%)
l_options := $(library_paths:%=-l%)
L_options += $(linking_libs:%=-L%)

compile_flags := -g -O3 -std=c++11 $(I_options)
linking_flags := $(l_options) $(L_options)


# 编译静态库
objs/%.o : src/%.cpp
	@mkdir -p $(dir $@)
	@g++ -c $< -o $@ $(compile_flags)

lib/libxxx.a : $(lib_objs)
	@mkdir -p $(dir $@)
	@ar -r $@ $^

static_lib : lib/libxxx.a

# 链接静态库
objs/main.o : src/main.cpp
	@mkdir -p $(dir $@)
	@g++ -c $< -o $@ $(compile_flags)

workspace/exec : objs/main.o
	@mkdir -p $(dir $@)
	@g++ $^ -o $@ $(linking_flags)


run : workspace/exec
	@./$<

debug:
	@echo $(compile_options)

clean:
	@rm -rf objs workspace/exec

.PHONY: debug objects run
```