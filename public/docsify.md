# docsify配置教程

## docsify介绍

一个神奇的文档站点生成器。

Docsify即时生成您的文档网站。与 GitBook 不同，它不会生成静态 html 文件。相反，它会智能地加载和解析 Markdown 文件，并将它们显示为网站。

没有静态构建的 html 文件；简单轻便；智能全文搜索插件；各种各样的插件接口；支持表情符号；支持服务器端渲染

## windows环境配置

访问node的官网链接：https://nodejs.org/en/

点击下图红箭头所指下载lts版本，注意是lts版本的，不要下最新的（可能不稳定）

![](https://pic.xhcheats.cn/assets/2023/12/24/010331.png)

下载完成后打开安装，就一直next就行了，该同意的都同意，安装路径随你改不改都行，最后install

![](https://pic.xhcheats.cn/assets/2023/12/24/010337.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/010344.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/010350.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/010356.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/010403.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/010413.png)

安装完成后在命令框(win+r输入cmd)中输入node -v和npm -v

若如下图显示出版本号即为安装成功

![](https://pic.xhcheats.cn/assets/2023/12/24/010420.png)

这里先用npm装一个cnpm(这里用的淘宝镜像，可自选其他的)

在命令行中输入：npm install -g cnpm -registry=https://registry.npm.taobao.org

出现下图红线画的一行就成功了

![](https://pic.xhcheats.cn/assets/2023/12/24/010429.png)

在命令行中输入cnpm -v验证一下

![](https://pic.xhcheats.cn/assets/2023/12/24/010448.png)

## 安装docsify

输入命令：cnpm install docsify-cli -g
![](https://pic.xhcheats.cn/assets/2023/12/24/010454.png)

等待安装完成后如下图输入docsify -v验证

![](https://pic.xhcheats.cn/assets/2023/12/24/010502.png)

接下来新建一个文件夹，用于保存docsify源文件，新建的文件夹位置随便你
然后如下图打开文件夹是个空的

![](https://pic.xhcheats.cn/assets/2023/12/24/010510.png)

选中上方地址栏，删除原本的内容然后输入cmd后回车

![](https://pic.xhcheats.cn/assets/2023/12/24/010515.png)

就会进入这个文件夹目录下的终端

![](https://pic.xhcheats.cn/assets/2023/12/24/010525.png)

在这个终端中如下图输入docsify init进行初始化
出现(y/N)的这一行输入y回车
等待出现succeeded一行
然后输入docsify serve构建

![](https://pic.xhcheats.cn/assets/2023/12/24/010532.png)

在浏览器中输入http://localhost:3000访问即可

![](https://pic.xhcheats.cn/assets/2023/12/24/010541.png)

docsify的文件夹中会有下图的三个文件

![](https://pic.xhcheats.cn/assets/2023/12/24/010546.png)

至此，只要不关闭终端，就可以一直个人本地访问

如果需要让别人也能在别的地方通过浏览器访问就查看下面两种方法

## 托管和部署的区别

托管是将你的内容放在别的平台上，我这里是在github的平台，内容存放在github的服务器上，因为它在国外，访问有时候会进不去，另外GitHub平台需要对项目进行build，你新改的内容push上去不会立刻更新

部署你自己的国内服务器，访问绝对很快，而且什么都没有，更新也很快，但缺点也很明显，什么保护都没有，可能会被别人拿来做测试（极小概率），但这个服务器上传没有GitHub直接push舒服

## docsify托管到GitHub

这里使用的是GitHub desktop进行操作,相比于命令行要简单很多，需要去github创建一个账号并下载应用程序

github官网：https://github.com/

在软件中选择file->new repository（或按ctrl+n）新建

如下图添加一个新库

![](https://pic.xhcheats.cn/assets/2023/12/24/010813.png)

将之前初始化生成的三个文件拖进去

![](https://pic.xhcheats.cn/assets/2023/12/24/010839.png)

按下图在github desktop中操作

![](https://pic.xhcheats.cn/assets/2023/12/24/010846.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/010852.png)

然后登录你的网页版GitHub，打开刚刚上传的库，按红箭头所指操作

![](https://pic.xhcheats.cn/assets/2023/12/24/010858.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/010905.png)

### 有自己的域名

![](https://pic.xhcheats.cn/assets/2023/12/24/010913.png)

下图中红圈的内容是记录值

![](https://pic.xhcheats.cn/assets/2023/12/24/010939.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/010946.png)

在你的域名解析设置中添加一条记录，如下图所示，记录值就是之前红圈里的

![](https://pic.xhcheats.cn/assets/2023/12/24/010954.png)

### 无自用域名

![](https://pic.xhcheats.cn/assets/2023/12/24/011000.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/011006.png)

在填入上方内容后会报错，但会在pages页面最上面显示出如下图内容，所指链接就是公开访问链接

![](https://pic.xhcheats.cn/assets/2023/12/24/011012.png)

## docsify部署至linux服务器

### 服务器配置

这里用的是阿里云的服务器，打开购买的服务器，我这里使用的centos和宝塔，在阿里云控制台打开服务器

在左边栏选择应用详情（如下图）

![](https://pic.xhcheats.cn/assets/2023/12/24/011054.png)

先点开右边的远程连接（如下图）

将上图中的三个红箭头所指内容复制到下图打开的终端中

每复制一个就回车运行一次

![](https://pic.xhcheats.cn/assets/2023/12/24/011123.png)

复制上图中的外网面板地址到浏览器最上方的地址栏

进入后如下图

输入终端中的username和password登录

![](https://pic.xhcheats.cn/assets/2023/12/24/011130.png)

下图看完后同意，进入面板

![](https://pic.xhcheats.cn/assets/2023/12/24/011136.png)

进入后点lnmp的一键安装

![](https://pic.xhcheats.cn/assets/2023/12/24/011145.png)
![](https://pic.xhcheats.cn/assets/2023/12/24/011150.png)
如下图就是完成了
![](https://pic.xhcheats.cn/assets/2023/12/24/011157.png)

如下图输入你的宝塔账户，如果没有就注册一个

![](https://pic.xhcheats.cn/assets/2023/12/24/011204.png)

进入面板后点击左边的网站如下图

![](https://pic.xhcheats.cn/assets/2023/12/24/011211.png)

### 添加网站

添加你购买的域名，其他默认

![](https://pic.xhcheats.cn/assets/2023/12/24/011217.png)

在这个网站后面添加ssl证书（需要去阿里云控制台获取）

![](https://pic.xhcheats.cn/assets/2023/12/24/011226.png)

在其他证书中添加你的证书点击保存后再打开强制https保存后关闭

![](https://pic.xhcheats.cn/assets/2023/12/24/011232.png)

现在进入你的域名解析控制台，按下图所指操作

![](https://pic.xhcheats.cn/assets/2023/12/24/011240.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/011302.png)

完成后会在解析设置中看见如下图的两条记录

![](https://pic.xhcheats.cn/assets/2023/12/24/011309.png)

访问你的域名即可看见你的站点（解析要时间可能要等几分钟）

![](https://pic.xhcheats.cn/assets/2023/12/24/011315.png)

现在再返回你的宝塔面板，点开刚刚在网站中添加的站点（下图红箭头）

![](https://pic.xhcheats.cn/assets/2023/12/24/011321.png)

点开后如下图点击配置文件

![](https://pic.xhcheats.cn/assets/2023/12/24/011327.png)

在配置文件中找到图中红圈的内容并将其删除

![](https://pic.xhcheats.cn/assets/2023/12/24/011333.png)

删完后如下图

![](https://pic.xhcheats.cn/assets/2023/12/24/011340.png)

再在网站栏目里面点击网址根目录

![](https://pic.xhcheats.cn/assets/2023/12/24/011346.png)

点击上传（我这一面是之前上传过的，东西才这么多，不用理会）

![](https://pic.xhcheats.cn/assets/2023/12/24/011353.png)

如下图，将之前初始化的docsify拖进去（如果是第一次，就只有三个文件）

![](https://pic.xhcheats.cn/assets/2023/12/24/011400.png)

刷新你的域名网站就会如下图（这是修改过的）

![](https://pic.xhcheats.cn/assets/2023/12/24/011407.png)

如果你需要修改自己的内容，就在本地编写index.html和新建各种md文件（也可以通过远程软件在本地远程连接服务器，就不用手动上传了）

你需要在网上百度一些前端的知识和markdown的知识

或者直接百度关于docsify的相关内容

这是docsify官方文档：https://docsify.js.org/#/

注意：不需要去从头学习这些知识，只需要百度你需要的内容和代码即可