# Ubuntu安装PowerVim

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