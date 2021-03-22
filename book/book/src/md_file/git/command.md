# Git 常用操作命令



### 常用命令   

**`<localBranchName>` ->  本地分支名**   
**`<localNewBranchName>` -> 本地新分支名**   
**`<remoteBranchName>` -> 远程仓库分支名**   



* 首次提交:    
    
    > `git init`      
    > `git add ./`      
    > `git push -u origin master`      


* 日常提交:    
    
    > `git add ./`                          -> 变更加入到暂存区域等待提交
    > `git commit -m "xxxx"`                -> 对变更进行说明      
    > `git pull origin <remoteBranchName>`  -> 同步远程仓库代码        
    > `git push origin <remoteBranchName>`  -> 将此次变更提交到远程仓      


* 切换分支：   

    > `git checkout <localBranchName>`   
    > `git checkout -b <localNewBranchName>`   （新建并切换到新建分支）    
    > `git checkout -b <localNewBranchName> <remoteBranchName> `（新建并拉取，本地分支名与远程之间用空格隔开）      

    

* 暂存：   

    > `git add ./`        
    > `git stash`      
    > `git stash pop` (弹出暂存文件)        



* 在本地目录下关联远程repository :      

    > `git remote add origin git@github.com:git_username/repository_name.git`       


* 删除已有的GitHub远程库：   

    > `git remote remove origin`    


* 查看远程库信息：      

    > `git remote -v`       



* 查看一下本地的分支状态:       

    > `git branch`   
    > `git branch -a`  (包括本地和远程)   
    > `git branch -vv`  (本地分支对应哪个远程分支)   
    > `git branch -d <localBranchName>`  删除分支 (不能删除当前所指向的分支)   
    


* 本地分支push到远程指定分支：   

    > `git push origin <localBranchName> : <remoteBranchName>`    
    > `git push origin --delete <remoteBranchName>` (删除指定的远程分支，谨慎操作！！)          

    


* 查看当前分支历史提交记录：    

    > `git log`   
    > `git reflog` (所有记录，包含删除操作)     


* 对当前分支版本回退：  

    > `git reset –hard 版本号`   
    > `git push -f ` （远程仓库同步回退，紧接上面命令）     



* 拉取远程分支(不会进行分支切换):   

    > `git fetch origin <remoteBranchName> : <localNewBranchName>`      


* 合并远程分支到当前本地分支:       

    > `git merge origin/<remoteBranchName>`     
    


* 仓库迁移:     

    > `git clone --bare [old remote]`      
    > `cd [project].git`      
    > `git push --mirror [new remote]`          


* 自定义彩色log     

    - 输出   
    > `git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --` 
    - 配置命令（git lg）   
    > `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`