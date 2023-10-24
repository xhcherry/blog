# VS Code远程免密登录

## 本地操作

生成公钥  ```ssh-keygen```

## 远程操作

进入home目录（cd ~）

打开.ssh目录，没有则创建

复制本机id_rsa.pub内容，放置到远程authorized_keys文件，没有则创建