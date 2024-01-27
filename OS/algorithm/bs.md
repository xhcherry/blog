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

10.某程序的段表如下，则逻辑地址 (1,262) 对应的物理地址为 ()\
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


## 三道算法题

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