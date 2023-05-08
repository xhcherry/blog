# Ubuntu更新GCC版本

```
#查看当前使用的gcc版本命令:
gcc -v
#更新软件源指令：
sudo apt-get update
#更新软件指令：
sudo apt-get upgrade
```

```
添加相应的源
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
更新软件源
sudo apt-get update
安装最新gcc
sudo apt-get install gcc-12

```

GCC最新版本查看地址：http://ftp.gnu.org/gnu/gcc/

刷新db然后用locate查看我们已有哪些版本的GCC
```
sudo updatedb && sudo ldconfig
locate gcc | grep -E "/usr/bin/gcc-[0-13]"
```

切换到最新的gcc版本
```
# 命令最后的 20和50是优先级，如果使用auto选择模式，系统将默认使用优先级高的
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 20
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 50
```