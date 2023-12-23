# msys2安装与更新

## 下载MSYS2

进入官网下载[MSYS2](https://www.msys2.org/)

安装后开始菜单进入msys2 msys

更新包数据库和基础包pacman -Syu

更新其余基本软件包pacman -Su

## 安装GCC/G++

pacman -S --needed base-devel mingw-w64-x86_64-toolchain

gcc路径：msys64\mingw64\bin