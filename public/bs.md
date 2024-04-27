# 笔试

## 京东测开

### 20道单选题

第1和9丢失

2.下列关于Tomcat的说法中，错误的是()\
A.Tomcat是一款小型的WEB应用服务器，适合于中小型WEB应用\
B.Tomcat的默认端口是8080\
C.Tomcat的项目目录放在/usr/local/tomcat/webapps下面,库文件放到/usr/local/tomcat/lib下面\
D.Tomcat的性能取决于jvm内存的大小，可以通过catalina.sh配置文件中的Xms和Xmx分别设置最大内存和最小内存

3.下列代码运行后会出什么错()
```java
class Access{
    public int x;
    private int y;
    void cal(int a,int b){
        x=a+1;
        y=b;
    }
}
public class Test{
    public static void main(String[] args){
        Access obj=new Access();
        obj.cal(2,3);
        System.out.println(obj.x+" "+obj.y);
    }
}
```
A.compile error\
B.runtime error\
C.2 3\
D.3 3

4.下列接口测试的常见接口的描述，有错误的一项是 ()\
A.webService接口: 走soap协议通过http传输，请求报文和返回报文都是xml格式的。\
B.http api接口:走http协议，通过路径来区分调用的方法，请求报文都是key-value的，返回报文一般都是json串，有get和post等方法，这也是最常用的两种请求方式。\
C.socket接口: 走UDP协议，通过sever和client建立连接来发送数据.\
D.dubbo接口: 使用的是 TCP/IP是传输层协议

5.哪个是自动化测试模型()

A.线性测试、模块化与类库、数据驱动测试、关键字驱动测试\
B.V模型、W模型、X模型、H模型\
C.概念模型、层次模型、网状模型、关系模型、面向对象模型\
D.瀑布模型、敏捷模型

6.postman中，Pre-request Script中设置环境变量的方法是 ()\
A.pm.environment.set("variable key","variable value");\
B.pm.environment.set("variable key":"variable value");\
C.pm.environment.set("variable key";"variable value");\
D.pm.environment.set("variable key"="variable value");

7.下列关于哈希查找说法错误的是 ()\
A.哈希表最适合的求解问题是查找与给定值相等的记录\
B.哈希查找不适合同样的关键字对应多条记录的情况\
C.哈希查找不适合范围查找\
D.无论冲突是否发生，哈希表的查找不需要关键字比较

8.一个类创建10个对象时，会创建多少个静态变量和属性变量()\
A.1, 10\
B.10, 10\
C.10, 1\
D.11

10.某程序的段表如下，则逻辑地址 (1,262) 对应的物理地址为 ()

|段号|段首址|段长度|
|:---:|:---:|:---:|
|0|400K|80K|
|1|128K|40K|
|2|780K|20K|
|3|1000K|30K|

A.780K+2\
B.390K\
C.128K+262\
D.130K

11.现用广度优先搜索算法(BFS) 来遍历一个无向图G，则在最坏情况下，BFS算法实现的空间复杂度为()\
注:存储图所需的空间不计入算法实现的空间复杂度计算,|V|表示顶点个数,|E|表示边数。\
A.O(1)\
B.O(|V|)\
C.O(|E|)\
D.O(|E+V|)

12.下列关于Java引用关系描述错误的是 ()\
A.有强引用指向一个对象，就能表明对象还”活着”，垃圾收集器不会回收这个对象\
B.有软引用指向一个对象，只有当JVM认为内存不足时，才会去试图回收软引用指向的对象\
C.被弱引用指向的对象，在下次垃圾回收时一定会被回收\
D.不能通过虚引用 (幻象引用) 访问到对象，使用时必须结合引用队列。

13.某主机的 IP 地址为 212.212.77.55，子网掩码为 255.255.252.0.若该主机向其所在子网发送广播分组，则目的地址可以是()\
A.212.212.76.255\
B.212.212.77.255\
C.212.212.78.255\
D.212.212.80.255\
E.212.212.79.255

14.对于MySQL的触发器，以下说法不正确的是()\
A.可以触发触发器的操作是: insert、 delete、 select、 update\
B.触发器可以用在所在数据库以外的对象上\
C.一个表可以定义多个触发器\
D.触发器是在check约束之前执行的

15.使用分时系统，当就绪进程数达到20时，并保证响应时间不超过1s(忽略进程切换时间)，此时时间片最大应为()\
A.100ms\
B.50ms\
C.1s\
D.20ms

16.在下面的代码片段中，变量'b'和变量'd'是什么类型?\
int a [],b;\
int []c,d;\
A.'b'和'd'都是int\
B.'b'和'd'都是arrays\
C.'b'是int;'d'是array\
D.'d'是int;'b'是array

17.在高度为10(只有根节点的高度为0)的堆中，元素个数最少和最多分别是()\
A.1024 2047\
B.1023 1024\
C.512 513\
D.512 1024

18.下面关于InnoDB 存储引警和 MyISAM 存储引擎正确的是()\
A.InnoDB 支持行级锁和表级锁，而 MyISAM 支持表级锁\
B.InnoDB 支持全文索引，而 MyISAM 不支持全文索引\
C.InnoDB 不支持事务，而 MyISAM 支持事务\
D.InnoDB 不支持外键，而 MYISAM 支持外键

19.假设有n个条件，每个条件的取值有两个(0,1)，全组合有几种规则?\
A.2n\
B.2^n\
C.n^2\
D.n

20.一棵含有6个节点完全二叉树的中序遍历为[n,y,m,x,p,z]，那么这棵树的前序遍历结果()\
A.[x,y,m,n,z,p]\
B.[x,y,n,m,z,p]\
C.[n,m,y,p,z,x]\
D.[n,m,p,y,z,x]

### ACM输入示例

```py
# ACM模式多行输入示例
# 给出n阶方阵里所有数,求方阵里所有数的和
"""
输入
3
1 2 3
2 1 3
3 2 1
输出
18
"""
import sys
if __name__ == "__mian__":
    # 读取第一行的n
    n = int(sys.stdin.readline().strip())
    ans = 0
    for i in range(n):
        # 读取每一行
        line = sys.stdin.readline().strip()
        #把每一行的数字分隔后转化成int列表
        values = list(map(int, line.split()))
        for v in values:
        ans += V
    print(ans)

# ACM模式单行输入示例
# 求和，输入1 1，输出2
import sys
for line in sys.stdin:
    a = line.split()
    print(int(a[0]) + int(a[1]))
```

### 算法1

给出一个长度和Nowcoder等长的字符串s，请你计算要更改s中的几个字符可以
使得字符串s和Nowcoder相等\
输入描述：在一行中输入一个长度和Nowcoder等长的字符串s\
保证字符串中只包含大小写字母和数字\
输出描述:在一行中输出一个数字代表要更改的次数\
输入nowcoder输出1\
输入nOWcoDeR输出5

```py

```

### 算法2
小红有一个nxm大小的矩阵，阵第i行第j列的元素为ixj，小红想知道矩阵中第k小的元素是多少\
输入描述:第行三个整数n,m,k;1<=n,m < 10^5,1<=k<=n x m\
输出描述:输出一个整数表示答案\
输入3 3 4,输出3\
矩阵为\
1 2 3\
2 4 6\
3 6 9

```py

```

### 算法3
小Z在数星星，一片天空可用一个大小为nxm的矩阵表示，其中0表示空白，1表示有光源，定义一颗星星为如下3x3矩阵\
010\
111\
101\
注意，上面矩阵中的空自也不能有光源(即"0"不能为"1")，光源处也不能有空白(即"1"不能为"0")，否则不构成一颗星星。帮他数出天空中的星星颗数(即完全相同的矩阵个数)\
输入描述：\
第一行两个整数，表示n,m。\
后接n行，每行m列，表示天空\
数据保证1<=n,m<=500。\
输出描述:输出一行表示星星颗数\
输入\
4 4\
0101\
1110\
1011\
1101\
输出1

```py

```


## 车300自动化测试

### 功能测试⽤例设计
1. 在最近的项⽬测试过程中，对哪些功能实现了接⼝⾃动化

在实习的时候，有如下功能的接口自动化：网页端下发设备远程升级(OTA)，网页端对设备进行远程操控，对设备进行can报文模拟发送，对继电器的控制自动控制

2. 简单介绍⼀下其中⼀个接⼝及其⾃动化⽤例的设计思路

OTA升级是通过无线网络对车载系统进行远程升级的机制。我们需要通过公司云平台相关接口(升级信息，下载升级包，校验升级包，执行升级)与远程服务器进行通信并获取软件包信息。\
用例设计思路：使用python调用云平台登录接口自动登录，然后调用版本信息校验、下载升级包、执行升级等接口实现通过平台自动下发升级任务并执行升级计划。

### 性能测试⽅案设计
有这样⼀个接⼝，⽣产环境95%响应时间在100ms内，最⼤响应时间不超过200ms，业务⾼峰期每秒钟请求数量为1000次左右，现需要在机器资源约为⽣产环境1/5的测试环境进⾏压测，请设计对应的性能测试⽅案并简述设计思路

```
性能测试方案:
测试目标:验证在测试环境下，系统能否满足生产环境的性能需求，即95%的响应时间在100ms内，最大响应时间不超过200ms。
测试环境:机器资源约为生产环境的1/5。
测试工具:使用性能测试工具，如Apache JMeter、Locust等。
测试场景设计:模拟高峰期的负载，设置每秒钟请求数量为1000次左右。
性能指标:关注关键性能指标，包括响应时间（平均、95%分位、最大）、吞吐量、并发用户数等。

测试步骤:
a. 预热阶段: 在测试开始前进行一段时间的预热，确保系统已经处于稳定状态。
b. 逐步增加负载: 从低到高逐步增加负载，观察系统的性能表现。
c. 高峰期负载: 将负载设置为高峰期的预期负载，持续一段时间。
d. 恢复测试: 在高峰期后逐步减少负载，观察系统的性能是否能够迅速恢复。

压测脚本设计:设计模拟真实用户行为的压测脚本，确保模拟的场景与实际使用场景相符。
监控与分析:实时监控系统的性能指标，包括服务器资源利用率、响应时间等。
在测试结束后，进行性能分析，查找性能瓶颈和潜在问题。
测试报告:汇总测试结果，生成性能测试报告，包括各项性能指标的数据和分析、性能瓶颈的定位和建议。

设计思路:
资源模拟: 虽然测试环境资源只有生产环境的1/5，但通过模拟真实用户行为和逐步增加负载的方式，可以观察系统在资源受限的情况下的性能表现。
场景模拟: 设计场景需符合真实用户行为，包括登录、浏览、搜索、提交等操作，以确保测试结果具有实际参考价值。
监控与分析: 使用监控工具实时监控系统性能，根据性能指标和日志数据进行深入分析，找到性能瓶颈和潜在问题。
持续优化: 根据性能测试结果提出系统优化建议，包括代码优化、数据库索引优化等，以提高系统在受限资源下的性能。
```

### 测试数据收集(python)

1. 现需要收集中国⼤陆境内的100个经纬度坐标及其对应的省份城市，请提交可执⾏的python脚本及坐标csv⽂件


2. 如果在规定时间内要求收集10000个这样的坐标呢，你会如何加快脚本执⾏速度？请简述优化思路或提交可执⾏的python脚本

使用并发请求库asyncio来并行发起多个逆地理编码请求，以减少等待时间。这样可以在同一时间内处理多个请求，提高效率

## e签宝测试

### 选择题

1. 下列代码的时间复杂度：D
```
int f(unsigned int n) {
    if (n == 0 || n == 1) return 1;
    else return n * f(n - 1);
}
A. O(n!)  B.O(n^2)  C.O(1)  D.O(n)

这是个递归函数，n 的阶乘。
时间复杂度取决于递归调用的次数，即函数 f 被调用的次数。
在这个递归函数中，每次递归调用都会使 n 减少 1，直到 n 减少到 0 或 1 时返回 1。
因此，函数 f 将被调用 n 次，时间复杂度为 O(n)。

```
2. 两个线程运行在双核机器上，每个线程主线程如下，线程1：x=1;r1=y;线程2：y=1;r2=x;x和y是全局变量，初始为0，r1和r2可能是多少：A，C

```
A. r1=0;r2=0  B.r1=0;r2=1  C.r1=1;r2=1  D.r1=1;r2=0

```

3. 一个栈的入栈ABCDE，不可能的出栈顺序是 A
```
A.ABCDE B.DCEBA C.DECBA D.ECDBA
```

4. 关于cpp和java类中的static成员和对象成员 的说法正确的是 B
```
A.static成员变量在对象构造时生成
B.虚成员函数不可能是static成员函数
C.static成员函数不能访问static成员变量
D.static成员函数在对象成员函数中无法使用
```
5. n从1开始，每个操作可以选择对n加1或者对n加倍，若想获得整数2013，最少需要几个操作：
```
A.18 B.24 C.不可能 D.21
```
6. 一个洗牌程序的功能是将n张牌的顺序打乱，以下关于洗牌程序的功能定义说法最恰当的是：A

```
A.n张牌的任何两个不同排列出现的概率相等
B.任何连续位置上的两张牌的内容独立
C.每张牌出现在n个位置上的概率独立
D.每张牌出现在n个位置上的概率相等

洗牌程序的目的是打乱牌的顺序，使得每一种可能的排列出现的概率相等
```

7. 袋中有红球，黄球，白球各一个，每次任意取一个放回，如此连续3次，则下列事件中概率是8/9的是A

```
A.颜色不全相同
B.颜色全不相同
C.颜色全相同
D.颜色无红色

总共有 3^3 = 27 种可能的事件，其中：

颜色不全相同的事件有：RRY, RYR, YRR, RWW, WRW, WWR，共6种情况。
颜色全不相同的事件有：RYW, RYW, YWR, YRW, WRY, WYR，共6种情况。
颜色全相同的事件有：RRR, YYY, WWW，共3种情况。
颜色无红色的事件有：WWW，1种情况。
```
8. 测试ATM的取款功能，已知取款数只能输入正整数，每次取款要求是100的倍数且不大于500，正确的无效等价类是 B
```
A.(500,+∞),任意大于0小于500的非100倍数的整数
B.(-∞,100)、(100,500)、(500,+∞)
C.(0,100)、(100,500)、(500,+∞)
D.(500,+∞)
```
9. 下列排序算法中，初始数据集的排列顺序对算法性能无影响的是 C
```
A.冒泡
B.插入
C.堆排序
D.快速排序
```

### 问答题
1.进程与线程的区别

```
定义：
进程是程序在执行过程中分配和管理资源的基本单位。每个进程都有独立的内存空间，包括代码、数据和堆栈等。
线程是进程中的一个执行单元，是操作系统能够进行运算调度的最小单位。一个进程可以包含多个线程，它们共享进程的资源。

资源分配：
进程是独立的资源分配单位，拥有独立的地址空间和系统资源（如内存、文件句柄等）。
线程是进程内的实体，共享进程的资源，包括内存、文件和网络连接等。

切换开销：
由于进程拥有独立的地址空间，进程间的切换开销相对较大，需要保存和恢复大量的状态信息。
线程切换的开销相对较小，因为线程共享进程的地址空间，切换时只需要保存和恢复线程的部分状态即可。

并发性：
进程之间是独立的，互相之间不直接共享资源，通信需要通过进程间通信（IPC）机制来实现。
线程之间共享进程的资源，可以直接进行通信和共享数据，但也需要注意同步和互斥问题。

系统调度：
进程是操作系统进行资源分配和调度的基本单位，不同进程之间由操作系统进行调度和管理。
线程是操作系统进行调度和管理的基本单位，不同线程之间的调度由操作系统的线程调度器完成。
```

2.为产品“百度搜索”设计测试用例

```
基本搜索功能测试：
输入关键词进行搜索，确保搜索结果正确显示。
搜索不同类型的内容（网页、图片、视频、新闻等），验证搜索结果是否符合预期。

搜索建议测试：
在搜索框中输入部分关键词，观察是否有搜索建议出现。
点击搜索建议，检查是否能够正确跳转到相关搜索结果页面。

搜索结果页面测试：
确保搜索结果页面的布局、样式和功能正常。
验证搜索结果的排序是否准确，相关性是否高。

搜索结果过滤测试：
测试搜索结果页面的过滤功能，如按时间、按来源、按地点等。
确保过滤结果的准确性和实用性。

相关搜索测试：
在搜索结果页面底部查看相关搜索，验证相关搜索的准确性和实用性。

搜索设置测试：
进入搜索设置页面，测试不同设置选项的功能和影响。
确保用户对搜索结果的个性化设置能够正确生效。

移动端搜索测试：
在移动设备上测试搜索功能，确保在不同分辨率和浏览器下的兼容性。
验证移动端特有的搜索功能和体验。

搜索性能测试：
测试搜索功能的响应速度和性能，包括搜索速度、结果加载时间等指标。
模拟高并发场景，测试搜索服务的稳定性和可靠性。

安全性测试：
检查搜索功能是否存在安全漏洞，如SQL注入、XSS攻击等。
确保用户输入的搜索内容得到有效过滤和处理，防止恶意攻击。

多语言测试：
测试不同语言环境下的搜索功能，确保搜索结果的多语言支持和准确性。
```

3.已知有两个表A，B，其中A为用户表包含(id,name),B为权限表包含(用户id，role，app)，找出其中拥有应用最多用户极其应用，写出正确查找的sql语句

```
可以使用 SQL 的聚合函数和联合查询来实现

SELECT B.app, COUNT(DISTINCT B.用户id) AS 用户数
FROM B
GROUP BY B.app
ORDER BY 用户数 DESC
LIMIT 1;

这个查询首先按应用分组，并统计每个应用拥有的唯一用户数。然后按用户数降序排序，并只取第一行结果，即拥有应用最多用户的应用以及对应的用户数。

假设 用户id 在表 A 中称为 id，在表 B 中称为 用户id，应用在表 B 中称为 app。
```

4.描述TCP/IP建立连接的过程

```
客户端向服务器发起连接请求：

客户端首先向服务器发送一个 SYN（同步）标志的数据包，表示客户端要求建立连接。这个数据包中会包含客户端的初始序列号（Seq=X）。
服务器确认连接请求：

服务器收到客户端发来的 SYN 数据包后，如果同意建立连接，则会向客户端发送一个 SYN ACK（同步确认）标志的数据包，表示服务器接受客户端的连接请求，并为该连接分配资源。这个数据包中会包含服务器的初始序列号（Seq=Y），以及确认号（Ack=X+1）。
客户端确认连接：

客户端收到服务器发来的 SYN ACK 数据包后，会向服务器发送一个确认数据包，表示客户端也接受服务器的连接请求。这个数据包中会包含确认号（Ack=Y+1）。
通过这个过程，客户端和服务器建立起了双向的可靠连接，可以开始进行数据传输了。
```

5.测试用例的设计方法有哪些

```
等价类划分法：将输入数据划分为若干个等价类，然后从每个等价类中选择代表性的数据作为测试用例。这种方法能够有效地覆盖输入数据的不同情况，同时减少了测试用例的数量。

边界值分析法：针对输入数据的边界条件进行测试设计，通常会选择边界值、边界值的前一个值和后一个值作为测试用例。这种方法可以有效地发现由于边界条件引起的错误。

错误推测法：根据经验或者对系统的理解，推测出可能存在的错误，并设计相应的测试用例来验证这些错误情况。这种方法可以帮助发现一些隐藏的、不易察觉的错误。

正交试验法：通过正交表设计测试用例，使得各种可能的输入组合都能够被覆盖到，从而在相对较少的测试用例数量下达到较好的测试覆盖率。

状态转换法：针对有状态的系统，根据系统的状态转换图设计测试用例，覆盖不同的状态转换路径，以确保系统在不同状态下的行为正确性。

决策表法：根据系统的输入条件和动作规则，设计出决策表，然后根据决策表中的条件组合来设计测试用例，以验证系统对不同条件组合的处理是否正确。
```

### 编程题

1.给你一个包含n个整数的数组nums，判断nums中是否存在三个元素a,b,c使得a+b+c=0？请你找出所有和为0且不重复的三元组

输入nums=[-1，0，1，2，-1，-4] 输出：[[-1,-1,2],[-1,0,1]]

输入输入nums=[] 输出:[]

输入[0] 输出:[]

```py
def threeSum(nums):
    res = []
    nums.sort()
    for i in range(len(nums)):
        if nums[i] > 0:
            break
        if i > 0 and nums[i] == nums[i-1]:
            continue
        l, r = i+1, len(nums)-1
        while l < r:
            if nums[i] + nums[l] + nums[r] == 0:
                res.append([nums[i], nums[l], nums[r]])
                while l < r and nums[l] == nums[l+1]:
                    l += 1
                while l < r and nums[r] == nums[r-1]:
                    r -= 1
                l += 1
                r -= 1
            elif nums[i] + nums[l] + nums[r] < 0:
                l += 1
            else:
                r -= 1
    return res

这道题目要求在给定的整数数组 nums 中找到所有不重复的三元组，使得三元组中的元素之和等于 0。算法的关键在于先对数组进行排序，然后使用双指针技巧来遍历数组，寻找和为 0 的三元组。

下面是对算法的解释：

首先对数组 nums 进行排序，这样相同的数字会相邻排列，方便后续的去重操作。

然后遍历数组 nums，以每个元素作为第一个元素 nums[i]，初始化左右指针 l 和 r，分别指向 i+1 和数组最后一个元素。

在遍历过程中，若 nums[i] > 0，说明后面的元素都大于 0，不可能存在和为 0 的三元组，直接跳出循环。

若 nums[i] 与前一个元素相同（即重复元素），则跳过这次循环，避免产生重复的三元组。

在内层循环中，通过双指针 l 和 r，不断向中间靠拢，查找和为 0 的三元组。若和等于 0，则将三元组 [nums[i], nums[l], nums[r]] 加入结果列表，并移动指针 l 和 r。

注意在内层循环中，还需要判断重复元素的情况，如果指针移动后的元素与前一个元素相同，则继续移动指针直到遇到不同的元素为止。

最终返回结果列表 res。

这个算法的时间复杂度为 O(n^2)，其中 n 是数组 nums 的长度。因为排序的时间复杂度为 O(nlogn)，而双指针遍历的时间复杂度为 O(n^2)。
```

2.复制某个目录及其包含的文件到指定的目录，比如：复制/root/tms/tmsdoc到/root/document

## 360测开

1 多选题：关于自动化测试的目的，下面表述正确的是(bcd)
```
提高交付能力
提高测试效率
提高准确性、稳定性
模拟人工测试无法实现的复杂场景
```

2 单选题：下面哪个操作是selenium不支持的d
```
截图
页面跳转
点击按钮
setCookie
```

3 多选题:关于自动化测试的特点，以下哪些描述是正确的(bd)
```
能够覆盖软件所有功能
准确度、精确度高
自动化测试可以取代手工测试
速度快、效率高
```

4 单选题：DataOutputStream的直接父类为(c)
ObjectOutputStream
FileOutputStream
FilterOutputStream
OutputStream

5 单选题：下列关于泛型类的说法，正确的是a
```
方法传递时不必进行类型转换
不能直接使用类型参数代替参数类型
不能直接使用类型参数代替返回值类型
实例化泛型类时可以用boolean型替换类型参数
```

6 单选题:有下列类定义
```cpp
class T{
    int t1;
protected:
    int t2;
public:
    void show(){}
};
class D:public T{
    int d1;
protected:
    int d2;
public:
    void show(){}
};
派生类D中的public成员个数是(d)
3
2
4
1
```

7 单选题：下列程序的运行结果是(d)
```cpp
class T{
public:
    T(int i=0):x(i){ c++;show();}
    ~T(){c--;show();}
    static void show(){cout<<c;}
private:
    static int c;
    int x;
};
int T::c=0;
int main(){
    T* i= new T[3];
    T* j= new T(8);
    delete []i;
    delete j;
    return 0;
}

无输出结果
1234567891011109876543210
1210
12343210
```

8 单选题:下列(c)是istream类的对象
```
clog
cerr
cin
cout
```

9 存在如下代码，访问http://serverName/index.php/Home/lndex/index时输出结果为(c)
```php
<?php
    namespace Home\Controller;
    use Think\Controller;
    class IndexController extends Controller{
        public function_before_index(){
            echo 'before<br/>';
        }
        public function index(){
            echo 'index<br/>';
        }
        public function_after_index(){
            echo 'after<br/>';
        }
    }
?>

index

before
index

before
index
after

before
after
```

10 单选题：ANR通常在下面哪种情况下会发生(c)
```
当应用程序接收到新的通知时
当应用程序收到网络请求时
当应用程序运行时间过长而没有响应用户操作时
当应用程序进行导航操作时
```

11 单选题：关于 Android 的 ContentProvider 用途描述正确的是(a)
```
ContentProvider 用于管理应用程序的数据存储和共享,
ContentProvider 用于加载和更新应用程序的布局文件。
ContentProvider 用于处理应用程序的用户界面事件。
ContentProvider 用于发送和接收网络请求
```

12 单选题:下列关于Android网络编程框架的说法中，正确的是(a)
```
Retrofit是一个用于Android网络请求的开源框架，底层基于OkHttp实现。
Android网络编程框架只能用于同步的网络操作，不能进行异步操作。Volley是Android官方推荐的网络编程框架，具有高性能和稳定性,
Android网络编程框架不能处理网络请求的结果，需要手动解析数据
```

13 单选题：为IPv6数据包提供数据完整性、数据验证、数据机密性和重放保护服务的协议是(d)
```
SHA
SSL
DAS
IPSec
```

14 单选题:下面(b)用于动态多态的实现
```
重载
虚函数
隐藏
模板
```
15 单选题:代码输出（b）
```cpp
#include <iostream>
using namespace std;
template <class T>
void fun(T temp){
    cout<<temp<<endl;
    cout << "A" << endl;
}
void fun(int temp){
    cout << temp << endl;
    cout << "B" << endl;
}
int main(){
    fun(30);
    return 0;
}

30
A

30
B

30
A
B

30
B
A
```

16 单选题:下面说法正确的是(a)
```
int *p1 = new int[5];
int *p2 = new int[5]();
p1申请的空间里的值是随机值，p2申请的空间里的值已经初始化
p1和p2申请的空间里面的值都是随机值
p1申请的空间里的值已经初始化，p2申请的空间里的值是随机值
p1和p2申请的空间里的值都已经初始化
```

17 在电子商务中https协议进行数据加密传输,创建一次性会话密钥的节点是(d)
```
switch
router
client
server
```
18 单选题：在数据库中有两张表:商品表(商品号，商品名，商品类别，成本价)和销售表(商品号，销售时间，销售数量，销售单价)。用户黑统计指定年份每类商品的销售总数是和销售总利润，要求只列出销售总利润最多的前三类商品的商品类别、销售总数量和销售总利润。为了完成该统计操作，以下存储过程PSUM正确的语句是:(a)
```sql
PROCEDURE `P_SUM`
(IN `year` int)
BEGIN
SELECT 商品类别，SUM(销售数量) AS 销售总数量,
SUM(销售数量*(销售单价-成本价))AS 销售总利润
FROM 商品表 JOIN 销售表 ON 商品表,商品号=销售表,商品号
WHERE 销售时间 =year GROUP BY 商品类别 ORDER BY 销售总利润 DESC limit 0,3;
END

PROCEDURE `P_SUM`
BEGIN
SELECT 商品类别,SUM(销售数量) AS 销售总数量,
SUM(销售数量*(销售单价-成本价))AS 销售总利润
FROM 商品表 JOIN 销售表 ON 商品表,商品号=销售表,商品号
WHERE 销售时间 =year GROUP BY 商品类别 ORDER BY 销售总利润 DESC limit 0,3;
END

PROCEDURE `P_SUM`
(IN `year` int)
BEGIN
SELECT 商品类别，SUM(销售数量)AS 销售总数量,
SUM(销售数量*(销售单价-成本价))AS 销售总利润
FROM 商品表 JOIN 销售表 ON 商品表,商品号=销售表,商品号
WHERE 销售时问 =year GROUP BY 商品类别 ORDER BY 销售总利润 limit 0.3;
END

PROCEDURE `P_SUM`
(IN `year` int)
BEGIN
SELECT 商品类别，SUM(销售数量)AS 销售总数量,
SUM(销售数量*(销售单价-成本价))AS 销售总利润
FROM 商品表 JOIN 销售表 ON 商品表,商品号=销售表,商品号
WHERE 销售时问 =year ORDER BY 销售总利润 DESC limit 0,3;
END
```

19 单选题:以下关于离线存储说法不正确的是b
```
服务器对离线资源更新后必须更新manifest文件新资源才能被浏览器重新下载
站点离线存储的容量限制是5K
当更新资源失败，浏览器依旧采用原来的资源
离线的情况下，浏览器会直接使用离线存储的资源
```

20 单选题:使用二维数组存放无向图(无自环)，若二维数组的行数、列数、非零元素个数分别为4、4、8，则该无向图的顶点数和边数分别是d
```
8和8
8和4
4和8
4和4
```

21 单选题:Linux系统中/etc/fstab文件每一行由六列组成,列的分隔符为空格,现査看/etc/fstab每行记录是否完整 b
```
awk 'NF' /etc/fstab
awk '{print NF}' /etc/fstab
awk '{print NR}' /etc/fstab
awk '{print FS}' /etc/fstab
```

22 单选题：遍历方式可以得到一个图的最短路径 b
```
中序遍历
BFS
后序遍历
DFS
```

23 下列有关图的生成树说法错误的是(b)
```
非连通图的生成树可能不唯一
在生成树中加一条边一定构成回路
生成树中任意两顶点间的路径是不唯一的
图的生成树包含图的全部顶点
```

24 单选题:下面代码执行后的结果为(a)
```php
<?php
$a1=array("a"= >"red","b"=>"green","c"=>"blue","d"=>"yellow");$a2=array("e"= >"red","f"= >"green","g"=>"blue");
$result=array_diff($a1,$a2);
print_r($result);
?>

Array{
    [d] => yellow
}

Array{
    [a]=> red
    [b] => green
    [c] => blue
    [d] => yellow
}

Array{
    [e] => red
    [f] => green
    [g] => blue
}

Array{
    [a] => red
    [b]=> green
    [c]=> blue
}
```

25 单选题:ThinkPHP URL的默认模式为(c)
```
普通模式
REWRITE
PATHINFO
兼容模式
```

26 单选题:vim编辑器可以对shell程序进行加密,vim加密指定文件时使用的参数是(b)
```
-o
-x
-s
-e
```

27 单选题:一个学生表(student)，包含学号(id)，姓名(name)，年龄(age)，性别(gender)4个列;
一个课程表(course)，包含课程号(id)，课程名(name)，教师编号(t id);
一个成绩表(scores)，包含学号(s id)，成绩(score)，课程号(cid);
一个教师表(teacher)，包含教师编号(id)教师姓名(name)。
如果要删除student表中id为10的学生记录，同时删除scores表中的该学生信息，下面SQL语句错误的是:(a)
```sql
DELETE S, SC
FROM student s
INNER JOlN scores sc ON s.id = sc.s_id
WHERE s.id = 10;

DELETE FROM student s
USING scores sc
WHERE s.id = sc.s_id AND s.id = 10;

DELETE FROM s, sc
USING student s,scores sc
WHERE s.id = sc.s_id
AND s.id = 10;

DELETE S, SC
FROM student s
JOlN scores sc ON s.id = sc.s_id
WHERE s.id = 10;
```

28 单选题:创建订单表(orders)，记录了订单编号、订单日期和订单金额等信息，SQL语句如下()
```sql
CREATE TABLE orders(
order_id INT,
order_date DATE,
amount DECIMAL(10, 2)
);
INSERT INTO orders VALUES
(1,'2023-01-01'.100.00),
(2,'2023-01-02',200.00),
(3,'2023-02-01'150.00),
(4,'2023-02-02'250.00),
(5,'2023-03-01'300.00),
(6,'2023-03-02'350.00);
若要查询2023年3月的所有订单的订单金额总和和平均值。下面SQL语句不正确的是:(a)
SELECT sum(amount),avg(amount)
FROM orders
WHERE order_date ='2023-03-01'and order_date ='2023-03-02';

SELECT sum(amount),avg(amount)
FROM orders
WHERE order_date ='2023-03-01'or order_date ='2023-03-02';

SELECT sum(amount),avg(amount)
FROM orders
WHERE order_date >='2023-03' and order_date <'2023-04';

SELECT sum(amount),avg(amount)
FROM orders
WHERE DATE_FORMAT(order_date,'%Y-%m')='2023-03';
```


29 单选题:Fragment通过add或者replace的添加Fragment，对Fragment的生命周期影响描述错误的是d
```
如果当前 Activity 同一个布局id存在了Fragment,replace传递的Fragment实例和已经存在的Fragment实例不一致，旧Fragment生命周期执行 onPause ->onStop ->onDestroyView ->onDestroy->onDetach，新fragment走正常生命周期

如果当前 Activity 同一个布局id还未添加Fragment，add和replace操作相同，两者生命周期变化是onAttach->onCreate->onCreateView->onActivityCreated->onStart-onResume

如果当前 Activity 同一个布局id存在了Fragment，add传递的Fragment实例和已经存在的Fragment实例不一致,旧fragment生命周期执行 onPause ->onStop ->onDestroyView ->onDestroy->onDetach，新fragment走正常生命周期

如果当前 Activity 同一个布局id存在了Fragment，add传递的Fragment实例和已经存在的Fragment实例不一致，旧Fragment生命周期无变化，新Fragment走正常生命周期。
```

30 单选题：调用后可以使线程从新建状态进入就绪状态的方法是a
```
start()
onCreate()
run()
create()
```

31 单选题:onSavelnstanceState()的调用时机是(d)
```
在onstop()之后
在onDestroy()之前
在onDestroy()之后
在onStop()之前
```

32 单选题:分析以下代码
```html
<canvas id="myCanvas" width="100" height="100"></canvas>
<script>
var canvas= document.getElementByld("myCanvas");
var ctx= canvas.getContext("2d");
ctx.beginPath();
ctx.fillStyle = "red";
ctx.strokeStyle="green";
ctx.fillRect(0,0, canvas.width, canvas.height);
ctx.stroke();
</script>
下列说法正确的是a
代码是一个绿色边框，红色背景的矩形图
代码是一个红色边框的矩形图
代码是一个绿色边框矩形图
代码是一个红色实心的矩形图
```

33 单选题:下列JavaScript对象中，(b)可以被web worker访问
```
Window
Location
Document
Parent
```

34 单选题:存在以下xml文件，使用Node.getChildNodes()获取到元素的个数为()
```
<?xml version="1.0"encoding="utf-8" ?>
<class>
    <student>
        <firstname>cxx1</firstname>
        <lastname>Bob1</lastname>
        <nickname>stars1</nickname>
        <marks>85</marks>
    </student>
</class>
```

35 单选题:在 JDK9 开始，输入流的API中新增了 transferTo0 方法，下面选项中使用正确的是(c)
```java
try {
    InputStream inputStream = new FilelnputStream("c:/src.txt");
    File file = new File("d:/dest.txt");
    inputStream.transferTo(file);
} catch (Exception e) {
    throw new RuntimeException(e);
}

try {
    OutputStream outputStream = new QutputStream("c:/src.txt");
    File file = new File("d:/dest.txt");
    outputStream.transferTo(file);
}catch(Exception e){
    throw new RuntimeException(e);
}

try {
    InputStream inputStream = new FilelnputStream("c:/src.txt");
    OutputStream outputStream = new FileGtputStream("d:/dest.txt");
    inputStream.transferTo(outputStream);
} catch (Exception e){
    throw new RuntimeException(e);
}

try {
    InputStream inputStream = new FilelnputStream("c:/src.txt");
    OutputStream outputStream = new FileQutputStream("d:/dest.txt");
    outputStream.transferlo(inputStream);
} catch(Exception e){
    throw new RuntimeException(e);
}
```

36 单选题:使用甲、乙两种方法求解同一问题，2乙方法的消耗时间明显低于甲方法，则甲、乙不可能是c
```
甲:递归法 乙:动态规划法
甲:暴力枚举法 乙:回溯法
甲:动态规划法 乙:回溯法
甲:暴力枚举法 乙:动态规划法
```

37 单选题:在串的简单模式匹配中，当模式串位j与目标串位i比较时，两字符不相等，则i的位移方式是(b)
```
i=j+1
i=i-j+1
i++
i=j-i+1
```

38 单选题 KMP算法中，next数组的含义是(b)
```
模式串中每个字符的最长公共前后缀的长度
模式串中每个字符的最长公共前缀的长度
目标串中每个字符的最长公共前缀的长度
目标串中每个字符的最长公共前后缀的长
```

39 单选题：快速排序算法是基于(d)策略的一种算法
```
动态规划
递归
贪心
分治
```

40 单选题:在线性规划中，目标函数的要求是(c)
```
必须是凸函数
必须是单调函数
必须是线性函数
可以是任意函数
```

41 算法：中心位置
```python
题目描述：你有一个长度为m的正整数序列，下标从1开始。
对于这个序列，你需要统计整个序列的数字种类数，记种类数为m。
之后对于每一种数字x，记其出现次数为Cx，
设函数f(x)表示在序列中从左往右数[Cx/2]个数字x对应的下标([y]表示对y向上取整，例如[0.5]=1，[2]=2)。
最后你需要将所有f(x)按从小到大的顺序输出出来。

输入描述:
第一行一个正整数n(1≤n≤10^5)，表示序列长度。
第二行n个由空格隔开的正整数a1,a2…an,(1≤a,≤10^4)，表示该序列。
输出描述:
第一行输出一个正整数m，表示序列中的数字种类数。
第二行输出m个由空格隔开的正整数，分别表示按从小到大的顺序排序之后的函数值。末尾不要输出多余空格。

样例输入
9
3 4 5 5 3 4 4 5 3
样例输出
3
4 5 6
提示
一共有 3,4,5 三种数字，其中f(3)=5，f(4)=6，f(5)=4，排序后就是 4,5,6。

class Solution:
    def center(self, n, nums):
        count={}
        for i,num in enumerate(nums):
            if num not in count:
                count[num]=[]
            count[num].append(i+1)
        res=[]
        for num in sorted(count.keys()):
            mid_index=(len(count[num])+1)//2
            res.append(count[num][mid_index-1])
        res=sorted(res)
        return len(count),res
def main():
    n=input()
    nums=list(map(int,input().split()))
    s=Solution()
    m,res=s.center(n,nums)
    print(m)
    print(*res)
main()
```

42 算法：加密算法
```python
小X和小Y正在进行加密算法有关的研究
小X提出了一种简单的加密算法:对于一个只包含小写英文字母的字符串，将'a'替换成1,'b'替换成2...'z'替换成26，比如一个字符串'abcyz',加密后变成'1232526'。
但是小Y觉得对于一个加密后的数字串可能对应很多原串:比如'1232526'可能表示’abcyz’,也可能表示'abcbebf',也可能表示'lcyz'...
为了说服小X，小Y希望能计算出某个加密后的数字串可能对应的原串个数，由于答案可能很大，请输出答案对1000000007(10^9+7)取模后的结果。

输入描述:
第一行一个正整数n,表示加密后的数字串的长度。
接下来一行字符串长度为n,表示加密后的数字串。
对于所有数据，n<=50000，保证输入字符串所有字符是’1’到’9’的数字

输出描述:
输出一个正整数表示可能的原串个数

样例输入:
6
114514
样例输出:
6
提示
分别是'aadead','kdead','kden','aaden','anead','anen'六种可能的原串

class Solution:
    def encrypt(self, n, s):
        mod=1000000007
        dp=[0]*(n+1)
        dp[0]=1
        dp[1]=1
        for i in range(2,n+1):
            if s[i-1]=='0':
                if s[i-2]=='2' or s[i-2]=='0':
                    dp[i]=dp[i-2]
                else:
                    return 0
            else:
                if s[i-2]=='1' or (s[i-2]=='2' and s[i-1]<='6'):
                    dp[i]=(dp[i]+dp[i-2])%mod
                else:
                    dp[i]=dp[i-1]
        return dp[n]

def main():
    n=int(input())
    s=input()
    solution=Solution()
    res=solution.encrypt(n,s)
    print(res)
main()
```

## 小米测试

### 拉票

```
题目描述:
小明正在参加一次选举。这次选举一共有n个人参与投票，每个人都最多投1票目必须投1 票。
每一个人都隶属于且仅隶属于一个阵营，第i个人隶屈的阵营编号为a。
小明隶厘的阵营编号为x，所有隶厘于该阵营的人(允许没有人屈于该阵营的情况出现)都一定会投票给小明，其他阵营的人都不会主动投票给小明。
小明自己不参与投票，这 n个人当中也不包含小明。
在最终投票前，小明决定再游说 1个阵营投票给自己，如果小明游说成功，则隶属于被游说阵营的所有人都会投票给小明。
小明会选择1个阵营进行游说，使得最后获得的总票数尽可能多。
如果在小明游说前这n 个人已经全部隶属于小明所在的阵营,则小明不会展开游说。
假设小明游说一定成功，则小明在最终投票中最多可以获得多少票?

输入描述
输入第一行包含两个墪数n(1≤n≤10^5)和x(1≤x≤n)，分别表示参与投票的总人数和小明隶属的阵营编号。
输入第二行包含n个墪数，其中第i个整数ai(1≤ai≤n) 表示了第i个人隶属的阵营编号

输出描述
输出一行，一个整数，表示小明在最终投票中最多可以获得的票数。

样例揄入
6 2
1 2 2 2 3 3

样例输出
5
小明隶属于编号为2的阵营，获得3票;小明选择游说编号为3的阵营，再获得2票，一共获得5票
```
```python
class Solution:
    def vote(self, n, x, a):
        count = [0] * (n + 1)
        for i in a:
            count[i] += 1
        res = count[x]
        for i in range(1, n + 1):
            if i != x:
                if count[i]>0:
                    res += 1
        return res
def main():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    solution = Solution()
    res = solution.vote(n, x, a)
    print(res)
main()
```

### 字符转换
```
题目描述:
小李天生偏爱一些字符，对于一个字符串，他总是想把字符串中的字符变成他偏爱的那些字符。
如果字符串中某个字符不是他所偏爱的字符，称为非偏爱字符，
那么他会将该非偏爱字符替换为字符串中距离该字符最近的一个偏爱的字符。
这里的距离定义即为字符在字符串中的对应下标之差的绝对值。
如果有不止一个偏爱的字符距离非偏爱字符最近，
那么小李会选择最左边的那个偏爱字符来替换该非偏爱字符，这样就保证了替换后的字符事是唯一的。小李的所有替换操作是同时进行的。
假定小李有m个偏爱的字符，依次为c1,c2...cm,当小李看到一个长度为n的字符串s时，请你输出小李在进行全部替换撬作后形成的字符串。

输入描述
第一行输入两个正整数n，m。
接下来一行输入m个字符c1,c2...cm，每两个字符之间用空格隔开，表示小李偏爱的字符.
接下来一行输入一个字符串s。
1≤n≤100000，1≤m≤26，保证题目中所有的字符均为大写字符，小李偏的字符互不相同，且偏爱字符至少出现一次。

输出描述
输出一行字符串，表示小李将给定的字符串s替换后形成的字符串。

样例输入
12 4
Z G B A
ZQWEGRTBYAAI
羊例输出
ZZZGGGBBBAAA
公
字符Q为非偏爱字符，且偏爱字符z距离它最近，所以营换成Z:同理E距离G最近，替换成G:对于字符w，偏爱字符z和G与其距离相同，所以替换为左边的Z;
对于字符I，右边没有偏爱字符，左边第一个偏爱字符是A，所以替换成字符A.
同一个傧爱字符可能会在字符串中出现多次。
```

```python
class Solution:
    def replce(self, n, m, c, s):
        res=[]
        path=[]
        self.back(n, m, c, s, 0, path, res)
        return res[0]
    def back(self, n, m, c, s, index, path, res):
        for i in range(n):
            if s[i] not in c:
                left=i
                right=i
                while left>=0 and s[left] not in c:
                    left-=1
                while right<n and s[right] not in c:
                    right+=1
                if left<0:
                    path.append(s[right])
                elif right==n:
                    path.append(s[left])
                else:
                    if abs(i-left)<=abs(i-right):
                        path.append(s[left])
                    else:
                        path.append(s[right])
            else:
                path.append(s[i])
        res.append(''.join(path))
```

## 维谛测试

纯硬件电路，不写此栏

## 美团测开

![](https://pic.xhcheats.cn/assets/2024/04/03/231831.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/231831_1.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/231831_2.png)


## 东软测试

![](https://pic.xhcheats.cn/assets/2024/04/03/230405_1.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230405_2.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230405_3.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230405_4.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230405_5.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230405_6.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230405_7.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230405_8.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230404.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230404_1.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230404_2.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230404_3.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230404_4.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230404_5.png)

![](https://pic.xhcheats.cn/assets/2024/04/03/230405.png)


## 吉比特测试

## 诺瓦星云测试

## 美团saas测试实习

## funplus游戏测试

## 大华技术支持

## 勇仕游戏测试

## 云杉测试

## 完美世界

## 尚游游戏