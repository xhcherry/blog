# GitHub进行fork后与原仓库同步

进入从你自己GitHub账号克隆的本地某仓库根目录

执行命令 ```git remote -v``` 查看远程仓库的路径

如果出现(fatch)和(push)结尾的两句话，说明你未设置 upstream （上游代码库）。一般情况下，设置好一次 upstream 后就无需重复设置

执行命令```git remote add upstream https://github.com/***.git```将源仓库设为upstream

然后再执行命令 ```git remote -v```就会多出两条upstream语句

执行命令```git fetch upstream```抓取源仓库的更新

执行命令 ```git checkout master``` 切换到 master 分支,若已在无需切换

执行命令 ```git merge upstream/master``` 合并远程的master分支

执行命令 git push 把本地仓库向github仓库（你fork到自己名下的仓库）推送修改

