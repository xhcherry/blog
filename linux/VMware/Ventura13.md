# VMware安装macOS Ventura 13

iso镜像下载(百度网盘)：https://pan.baidu.com/s/1uf5gPOSXCh-frel1u_Sxkw?pwd=o2hg 
提取码：o2hg

下面的步骤都需要先安装VMware，如果你没有安装VMware就访问下方内容下载安装

> [VMware安装教程](VMware.md)

首先需要使用unlocker在VMware中解锁macOS，否则无法正常安装macOS

unlocker下载地址：https://github.com/DrDonk/unlocker/releases

这里只要下载最新的即可
![](https://pic.xhcheats.cn/assets/2023/12/24/004235.png)

下载完成后完全解压出来（禁压缩包内直接打开）

解压完成后进入下图中的目录文件并打开所指文件
（unlocker根目录中的windows目录）

然后等待黑框运行完成即可

![](https://pic.xhcheats.cn/assets/2023/12/24/004242.png)

接下来打开之前安装的VMware按下方所指操作

![](https://pic.xhcheats.cn/assets/2023/12/24/004248.png)

下面的教程中只要完成了图片中的内容后点击下一步就行

![](https://pic.xhcheats.cn/assets/2023/12/24/004254.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/004301.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/004306.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/004313.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/004332.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/004338.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/004345.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/004351.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/004358.png)

(下面在文本中的操作可搜索)

在文中找到board-id.reflectHost并且键值改为"FALSE"
board-id.reflectHost = "FALSE"

![](https://pic.xhcheats.cn/assets/2023/12/24/004409.png)

找到ethernet0.virtualDev并且键值改为"vmxnet3"

ethernet0.virtualDev = "vmxnet3"

![](https://pic.xhcheats.cn/assets/2023/12/24/004415.png)

在文件的最底部添加下面几行代码

board-id = "Mac-AA95B1DDAB278B95"

hw.model.reflectHost = "FALSE"

hw.model = "MacBookPro19,1"

serialNumber.reflectHost = "FALSE"

serialNumber = "C01234567890"

![](https://pic.xhcheats.cn/assets/2023/12/24/004421.png)

注意: 如果你是AMD的处理器, 就在文件的最底部再附加如下代码

smc.version = "0"

cpuid.0.eax = "0000:0000:0000:0000:0000:0000:0000:1011"

cpuid.0.ebx = "0111:0101:0110:1110:0110:0101:0100:0111"

cpuid.0.ecx = "0110:1100:0110:0101:0111:0100:0110:1110"

cpuid.0.edx = "0100:1001:0110:0101:0110:1110:0110:1001"

cpuid.1.eax = "0000:0000:0000:0001:0000:0110:0111:0001"

cpuid.1.ebx = "0000:0010:0000:0001:0000:1000:0000:0000"

cpuid.1.ecx = "1000:0010:1001:1000:0010:0010:0000:0011"

cpuid.1.edx = "0000:0111:1000:1011:1111:1011:1111:1111"

smbios.reflectHost = "TRUE"

最后保存并关闭

在VMware中选择打开此虚拟机

![](https://pic.xhcheats.cn/assets/2023/12/24/004444.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004458.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004505.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004511.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004517.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004523.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004530.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004536.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004546.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004553.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004606.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004611.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/004619.png)

这里会重启很多次，慢慢等着进就行了差不多30分钟左右，若三四十分钟还不行，就检查前面是不是有哪一步操作错误