# HTML

> 能够说出常⻅的html标签的作⽤

## html介绍

### 前端三⼤核⼼
    html:超⽂本标记语⾔，由⼀套标记标签组成
    标签：
    单标签： <标签名 />
    双标签: <标签名></标签名>
    属性： 描述某⼀特征 示例:<a 属性名="属性值">

### html⻣架标签
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>⽹⻚标题</title>
    </head>
    <body>
        ⽹⻚内容
    </body>
</html>
```

    html:根标签，所有的内容都应该放到html标签中
    head：头部标签
    body:身体标签（代码编写区域）

### 注释
    作⽤：描述的内容不会被浏览器执⾏
    说明：解析程序给程序员看
    快捷键：ctrl+/ <!--注释区域-->
    测试点： 前端⻚⾯上线之前检查注释描述或去除注释

### 标签
    标题： h1~h6
        说明：h1最⼤，h6最⼩
        示例：
            <h1>我是h1</h1>
            <h6>我是h6</h6>
    段落： p
        特点：语义化、独占⼀块（换⾏）
        示例：<p>我是段落</p>
    超链接a
        说明： 点击⽂本跳转到指定⻚⾯
        语法： <a href="https://www.baidu.com">⽂本</a>
    属性：
        href：跳转的地址或⽂件
        target:打开窗⼝模式。新窗⼝：target="_blank"
    图⽚
        说明： 在⻚⾯中插⼊⼀张图⽚
        测试点：必须有title属性（悬停和未加载显示）
    示例：<img src="011.jpg" title="希望在⽥野" width="100px" height="200px" alt="此处有⼀张⽥野照⽚"/>
    空格与换⾏
        空格： &nbsp; &->shift+7
        换⾏： <br />
    布局标签
        布局：设置⻚⾯布局，便于排版
        ⼤盒⼦：div、独占⼀⾏
        ⼩盒⼦：span、⼀⾏可以放多个
    列表：ul、ol、li
        ul：无序列表
        ol：有序列表
        li：列表项
        示例：
            <ul>
                <li>我是列表项</li>
                <li>我是列表项</li>
            </ul>
            <ol>
                <li>我是列表项</li>
                <li>我是列表项</li>
            </ol>

    script:js标签
    style:css标签
    link:外部加载css标签

    input标签
        ⽂本框： <input type="text" />
        密码框： <input type="password" />
        单选按钮：
        复选框：
        按钮：
            普通：type=button
            提交：type=submit
            重置: type=reset
    form标签
        作⽤：提交⻚⾯输⼊的数据到指定⻚⾯或后台