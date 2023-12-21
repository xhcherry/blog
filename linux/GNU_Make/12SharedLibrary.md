# 动态库（共享库）

## 编库
>编译 .c 文件
> 源文件[.c/cpp] -> Object文件[.o]
```makefile
g++ -c [.c/cpp][.c/cpp]... -o [.o][.o]... -I[.h/hpp] -g -fpic
```
> Object文件[.o] -> 动态库文件[lib库名.so]
```
g++ -shared [.o][.o]... -o [lib库名.so] 
```
> main文件[.c/cpp] -> Object文件[.o]
```
g++ -c [main.c/cpp] -o [.o] -I[.h/hpp] -g
```


## 链接
> 链接 main 的 Object 文件与动态库文件[lib库名.so]
```
g++ [main.o] -o [可执行文件] -l[库名] -L[库路径] -Wl,-rpath=[库路径]
```

## Makefile

```makefile
cpp_srcs := $(shell find src -name *.cpp)
cpp_objs := $(patsubst src/%.cpp,objs/%.o,$(cpp_srcs))

so_objs := $(filter-out objs/main.o,$(cpp_objs))

include_dirs := ./include \

library_dirs := ./lib \

linking_libs := ddd \

I_options := $(include_dirs:%=-I%)
l_options := $(linking_libs:%=-l%)
L_options += $(library_dirs:%=-L%)
r_options := $(library_dirs:%=-Wl,-rpath=%)

compile_options := -g -O3 -w -FPIC $(I_options)
linking_options := $(l_options) $(L_options) $(r_options)

objs/%.o : src/%.cpp
	@mkdir -p $(dir $@)
	@g++ -c $^ -o $@ $(compile_options)

compile : $(cpp_objs)

lib/libddd.so : $(so_objs)
	@mkdir -p $(dir $@)
	@g++ -shared $^ -o $@

dynamic : lib/libddd.so

workspace/exec : objs/main.o compile dynamic
	@mkdir -p $(dir $@)
	@g++ $< -o $@ $(linking_options)

clean :
	@rm -rf objs lib workspace

run : workspace/exec
	@./$<

debug:
	@echo $(so_objs)

.PHONY: debug run dynamic compile
```