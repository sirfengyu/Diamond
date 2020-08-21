git合并不同库的分支
--------------------------------------------------------------

*合并开源库的方法*

1. 首先拉取码云代码库

   git clone https://gitee.com/apulis/apulis_platform.git

    * 配置过滤条件
    
1. merge driver:

   `git config --global merge.ours.driver true`

2. 新建一个 .gitattributes 用于指定非文本文件的对比合并方式。
   ```
   # 假如你要避免这个文件，config.xml
   vim .gitattributes

   config.xml merge=ours

   ```

2. 创建新分支如：release-0.1.3-beta
   ```
   git branch release-0.1.3-beta
   git checkout release-0.1.3-beta 
   ```
3. 拉取远端代码库

   git remote -v

   git remote add dlws https://github.com/apulis/apulis_platform.git -b <public> (这步视情况变更)

   git pull dlws <public> --allow-unrelated-histories

4. 合并分支
   ```
   git branch -a 

   git merge dlws/public –no-ff 
   # 如果冲突太多，计划以新代码为主
   git  cherry-pick ..<master>  # 预先查看冲突文件并处理
   # git reset HEAD               # 撤销add
   # 如果远端冲突太多，必须要用远端代码
    git merge origin/master -s theirs –no-ff
   ```
5. 上传到开放repo
   
   `git push origin release-0.1.3-beta  <--force>`


---

* git将某分支的某次提交合并到另一分支

1. 代码开发的时候，有时需要把某分支（比如develop分支）的某一次提交合并到另一分支（比如master分支），这就需要用到git cherry-pick命令。

2. 切换到develop分支 <br>
   git log 命令，查找需要合并的commit记录，比如commitID：7fcb3defff；

3. 切换到master分支<br>
   git cherry-pick 7fcb3defff  就把该条commit记录合并到了master分支，这只是在本地合并到了master分支；

4. git push 提交到master远程 
 
至此，就把develop分支的这条commit所涉及的更改合并到了master分支

**参考**

1. [git 合并策略](https://blog.walterlv.com/post/git-merge-strategy.html#recursive)