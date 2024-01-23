## Linux更新GCC版本

### Ubuntu

```
#查看当前使用的gcc版本命令:
gcc -v
#更新软件源指令：
sudo apt update
#更新软件指令：
sudo apt upgrade
```

```
添加相应的源
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
更新软件源
sudo apt update
安装最新gcc
sudo apt install gcc-12 g++-12

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
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 50
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-12 50
```

### Centos

```
yum -y install centos-release-scl
yum -y install devtoolset-11-gcc devtoolset-11-gcc-c++ devtoolset-11-binutils
临时修改
scl enable devtoolset-11 bash
配置变量
echo "source /opt/rh/devtoolset-11/enable" >>/etc/profile
```

## Ubuntu安装PowerVim

本文安装环境Ubuntu22.04

```
git clone https://github.com/youngyangyang04/PowerVim.git

cd PowerVim

sh install.sh
```
错误一：Syntax error: "(" unexpected

```sudo dpkg-reconfigure dash```

在选择项中选No，即可

错误二：Taglist: Exuberant ctags (http://ctags.sf.net) not found in PATH.
Plugin is not loaded.

不一样的Ubuntu版本用的命令有点区别

```sudo apt install exuberant-ctags```

错误三：Cannot set language to "zh_CN.gb2312"

打开配置文件(.vimrc) 注释43行语言设置