![image](https://github.com/TMeatlazijiding/On_the_way/assets/62745028/1d06a668-fc43-4a24-b1c1-797f9e50efcc)
# git fetch但是不merge会有什么后果
~~~
git fetch 是将远程仓库的最新内容拉到本地，但并不会自动合并到本地的分支上。
如果你不合并，你的本地分支就会落后于远程分支，可能会产生冲突。
你可以先用 git fetch 拉取远程分支，然后用 git merge 或者 git rebase 来合并到本地分支34。
如果有冲突，你需要手动解决。
~~~

# merge和rebase的区别
~~~
merge 和 rebase 都是用来合并两个分支的方法，但是有一些区别，
merge 会生成一个新的提交，包含两个分支的所有历史，保留了时间顺序和分支结构。
rebase 会把一个分支的提交重新应用到另一个分支上，形成一个线性的历史，不会增加额外的提交。
使用 merge 的优点是可以保持完整的历史记录，方便追溯和回滚。使用 rebase 的优点是可以得到一个清晰的历史，方便阅读和审查。
~~~
# git中遇到的问题———回退已经push的commit，并重新提交
~~~
如标题所述，我正在开发一个比较难的功能，已经完成了一部分，由于难度较高进度很慢，手上还有其他issues需要处理，遂决定先
处理issue。这里我先用add到stage中，并commit，想回头继续开发时不至于被覆盖（！！！其实这里完全不用，这是一个很傻的操作，
只需在当前工作分支输入 git stash 来隐藏当前改动，然后新建一个分支来工作就好）

  这里插一个题外话，你新建的分支为了保持工作环境的干净，不能与你当前正在开发的功能混在一起，这时你需要切换到主工作目录
  来新建你的分支，使你的新建分支与主分支一样。当前分支为dev_nx
  git stash
  git co dev
  git co -b dev_issue (你的分支名字)

当建好dev_issue分支后，并debug完成后并正常push，随后发现push的是dev_nx，即我的开发分支，远端分支是我未写完的功能代码，
这时需要将远端分支代码回退一下，大致以下几种方法：
1.暴力删除你的远端分支，并重建一个，和远端主目录保持一致，因为git特性，远端和本地是独立的。（慎点）
2.将你之前修改好的issue分支（未受污染，只是在原有基础上debug了），强制pus并覆盖远程分支。
即在dev_issue分支上
  git add
  git commit -m "fix issue xx"
  git push origin dev_issue:dev_nx
这样就让远端分支在不受第一次push影响下再次push了issue修改

总结：这个事故发生的根本原因是git push origin dev_issue:dev_nx这一步，题主之前写成了
  git push origin dev_nx
将dev_nx push上去而不是push dev_issue分支

git push用法：
git push <远程主机名> <本地分支名>:<远程分支名>
在本地分支和远程分支同名时可以忽略‘：’

~~~

# git强制同步之坑
~~~
在开发环境中我期望有一个分支dev来保持和最新代码同步，用另一个分支dev_1来rebase dev保持自己开发分支的纯净同时在必要时获取最新代码，
当dev分支不慎被本地改动污染后，我们需要强制同步dev分支为远端最新。
这里有两种方法，
方法一：
git fetch --all
git reset --hard origin/<branch_name>
这里，<branch_name> 是你要更新的分支名称。这些命令将从远程仓库获取最新的代码，并强制覆盖本地的代码。

方法二：
git pull --force

注：方法二会将你本地的所有分支强制同步为远端最新，并忽略所有本地改动。如果你只想更新某个分支的代码，请使用方法1 。

假使你使用了方法2，且没有备份你的改动或者stash，那么你就可以复习一遍的代码了^0^...
~~~
