# 私有库开启GitHub pages

在看这篇文章之前，需要知道如何使用 Git 以及 GitHub 的一些基本操作。

创建公开仓库

创建一个用于存放 GitHub Pages 静态资源文件的仓库。.

创建完仓库后，并不需要自己手动开启 GitHub Pages。

在公开仓库开启 GitHub Pages

当一个仓库创建 gh-pages 后，GitHub 默认为这个仓库开启 GitHub Pages。

有两种方式维护公开仓库的 GitHub Pages。
```
1.手动维护

每次把源代码构建成网站的静态资源后，手动执行 git push 把静态资源上传到公开仓库的 gh-pages 分支。

2.GitHub Actions

直接在私有仓库配置 Actions，每次 git push 后自动执行构建，并把构建得到的静态资源 git push 到公开仓库的 gh-pages 分支。

强烈推荐使用 GitHub Actions 。GitHub Actions 的配置文件使用 YAML 语法，配置项简单，一次配置，自动发布。
```

GitHub Actions

在私有仓库中配置 Actions，把源代码构建成静态资源文件后 push 到公开仓库的 gh-pages 分支。

Action 配置文件参考：

```<xxx>``` 代表必填项且需要按自己实际情况更改。

```
name: Deploy Site
 ​
 on:
   push:
     branches: [ master ]
 ​
 jobs:
   build:
     runs-on: ubuntu-latest
     steps:
       - name: Checkout
         uses: actions/checkout@v3
         with:
           persist-credentials: fasle # false 是用 personal token，true 是使用 GitHub token
           fetch-depth: 0
 ​
      # ... TODO Build your site
 ​
      # <work-dir> 静态资源所在的目录
      # <domain> 需要 github.io 绑定的域名
      # <username> git commit 用户名
      # <mail> git commit 邮箱
      # <message> git commit 的 message
      # <secrets_name> Secrets 名称
      # <github_username> github 的用户名（公开仓库）
      # <repo> 公开仓库名
       - name: Commit and Push
         working-directory: <work-dir>
         run: |
           echo "<domain>" > CNAME
           git init
           git checkout -b gh-pages
           git add -A
           git -c user.name='<username>' -c user.email='<mail>' commit -m '<message>' 
           git remote add origin https://${{secrets.<secrets_name>}}@github.com/<github_username>/<repo>.git
           git push origin gh-pages -f -q
```

参考这个配置需要注意几点：

如果你的公开仓库与私有仓库在同一个账号下：

persist-credentials: fasle的 false 改为 true

第 35 行的 url 不需要 ${{secrets.<secrets_name>}}@ 这一段

如果你不需要 GitHub Pages 绑定域名，需要把第 30 行删除。

使用 Secrets 的好处是可以随时更换 PAT，且不会在配置文件中泄露敏感信息。

使用 Cloudflare 加速 + 隐藏真实地址(可选)

由于某些不可抗拒因素，在国内访问 xxx.github.io 是一个很魔幻的事情，有时很慢，甚至无法打开。

对于这种情况，可以使用 Cloudflare 加速访问。

同时，如果你不愿公开 GitHub Pages 的静态资源文件，但开启 GitHub Pages 后，使用 xxx.github.io 的方式访问的话，新建 GitHub 账号也将毫无意义。即使配置域名后，虽然在一定程度上隐藏了真实地址，但仍可以通过扫描 DNS 记录，找到真实的 xxx.github.io 的地址。

不过Cloudflare 在加速访问的同时，也帮你隐藏了真实的地址。