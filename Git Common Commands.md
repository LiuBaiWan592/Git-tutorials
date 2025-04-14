## 一、基本配置

1. 配置用户信息

   ```bash
   git config --global user.name "username"
   git config --global user.email "name@email.com"
   ```

2. 查看配置信息

   ```bash
   git config --list
   ```




## 二、基本命令

1. 初始化仓库

   ```bash
   git init
   ```

2. 添加到暂存区

   ```bash
   # 添加一个或多个文件
   git add <filename1> [filename2] [filename3] ...
   
   # 添加指定目录及其子目录
   git add <dir>
   
   # 添加当前目录下的所有文件
   git add .
   ```

3. 提交到本地仓库

   ```bash
   git commit -m <"message">
   ```

4. 查看仓库状态

   ```bash
   git status
   ```

5. 回退文件至在仓库中已知的状态

   ```bash
   # 将工作区指定文件 <filename> 恢复到最近一次提交时的状态，丢弃所有未提交的更改
   git checkout -- <filename>
   
   # 撤销工作区中对文件的修改
   git restore <filename>
   ```

6. 查看提交日志

   ```bash
   # 查看完整日志
   git log
   
   # 单行简略形式的树状图日志
   git log --graph --pretty=oneline --abbrev-commit
   ```

7. 记录执行的命令日志

   ```bash
   git reflog
   ```

8. 版本回退命令

   ```bash
   # 使用日志相关命令查询commit的ID，执行以下命令，另外因为当前分支当前版本的指针为HEAD，可是使用HEAD^表示上一次提交的ID
   git reset --hard <commitID>
   # 上述命令的 --hard 参数为硬退回，舍弃未暂存的更改， --soft 为软退回，保留所有更改
   git reset --soft <commitID>
   ```

9. 撤销在暂存区提交的文件

   ```bash
   # 查看暂存区的文件
   git ls-files
   
   # 撤销在暂存区提交的文件
   git restore --staged <filename>
   
   # 撤销工作区中对文件的修改，而不仅仅是暂存的修改
   git restore <filename>
   ```

10. 取消跟踪文件与删除文件

    ```bash
    # 停止跟踪某个文件，但并不想将其删除：执行这个命令后，文件将从Git的暂存区（Stage）中移除，但仍然保留在磁盘上。
    git rm --cached <filename>
    
    # 彻底删除Git代码库中的某个文件：这个命令不仅会从代码库中移除该文件，还会将其从磁盘中删除。
    git rm <filename>
    ```

11. 提交文件一次，但忽略后续更改

    ```bash
    # 将文件<filename>标记为假定未更改。从此之后，无论我们对该文件进行任何更改，Git都不会将这些更改视为需要跟踪和提交的修改。
    git update-index --assume-unchanged <filename>
    
    # 如果我们想要对文件进行后续更改的跟踪和提交，可以使用--no-assume-unchanged选项
    git update-index --no-assume-unchanged <filename>
    
    # 以上命令只对单个文件有效，若对某目录下所有文件都执行忽略后续更改则可使用以下命令
    git ls-files <目录路径>/* | xargs -I {} git update-index --assume-unchanged {}
    
    git ls-files <目录路径>/* | xargs -I {} git update-index --no-assume-unchanged {}
    
    
    # 如果忽略的文件多了，可以使用以下命令查看忽略列表
    git ls-files -v | grep '^h '
    
    # 提取文件路径，方法如下
    git ls-files -v | grep '^h ' | awk '{print $2}'
    
    # 所有被忽略的文件，取消忽略的方法，如下
    git ls-files -v | grep '^h' | awk '{print $2}' |xargs git update-index --no-assume-unchanged
    ```
    
    

## 三、分支命令

1. 查看分支

   ```bash
   git branch [-v]
   ```

2. 创建分支

   ```bash
   git branch <branchname>	
   
   # 创建分支后立即切换到新分支
   git checkout -b <branchname>
   ```

3. 切换分支

   ```bash
   git checkout <branchname>
   ```

4. 合并分支到当前分支

   ```bash
   git merge <branchname>
   ```

   **冲突问题：**

   * 无冲突：两个分支没有修改同一个文件

     > 执行`git merge <branchname>`命令进行快速合并，但是被合并的分支`<branchname>`的状态不会被保存
     >
     > 执行`git merge <branchname> --no-ff -m <"message">`命令，相当于解决冲突后进行一次新的提交，只不过并没有冲突

   * 有冲突：两个分支都对同一个文件进行了修改

     > 执行`git merge <branchname>`命令合并后，会将冲突部分在文件中展现，修改文件后再进行提交即可

5. 删除分支

   ```bash
   git branch -d <branchname>
   ```




## 四、远程仓库

1. 生成 SSH Key

   ```bash
   ssh-keygen -t rsa -C "name@email.com"
   ```

2. 关联远程仓库

   ```bash
   git remote add <remoteshortname> <url>
   
   # 例如：
   git remote add origin git@gitee.com:LiuBaiWan-Runner/git-demo.git
   ```

3. 推送到远程仓库

   ```bash
   # 第一次推送使用，使用-u参数将本地分支与远程分支关联
   git push [-u] <remoteshortname> <remoterepositorybranch>
   
   # 例如：
   git push origin main
   git push -u origin dev
   
   # 后续使用以下命令即可
   git push
   
   # 如果需要回退远程的版本，可在本地版本回退后，重新推送到远程仓库，但要添加 --force 强制参数
   git push origin main --force
   ```

   **冲突问题：**

   * 无冲突：本地仓库是在远程仓库的基础上进行的修改，然后进行的新的`commit`

     > 使用`git push`命令即可成功推送，更新远程仓库的内容

   * 有冲突：远程仓库有比本地更新的`commit`，则需要合并冲突

     > 先执行`git pull`命令将远程仓库下载到本地合并后，会将冲突部分在文件中展现，修改文件冲突后再进行`commit`,然后再次执行`git push`进行推送

4. 从远程仓库克隆

   ```bash
   # 第一次从远程仓库获取仓库执行克隆，将远程仓库完整的复制到本地
   git clone <url>
   
   # 例如：
   git clone git@gitee.com:LiuBaiWan-Runner/git-demo.git
   ```

5. 查看远程仓库

   ```bash
   # 查看详细信息
   git remote -v
   
   # 查看分支关联信息
   git branch -vv
   ```

6. 从远程仓库拉取

   拉取（`git pull`）就是把远程仓库最新的内容下载（`git fetch`）并合并（`git merge`）到本地，如果存在冲突需要合并冲突

   ```bash
   # 第一次拉取使用
   git pull <remoteshortname> <remoterepositorybranch>
   
   # 例如：
   git pull origin main
   
   # 后续使用以下命令即可
   git pull
   
   # git pull = git fetch + git merge
   ```

   **冲突问题：**

   * 无冲突：本地仓库当前的状态已经提交并推送到远程仓库，远程仓库有更新的状态

     > 直接执行`git pull`命令进行可以直接下载合并，当前的commit状态为远程仓库的状态

   * 有冲突：两个仓库都对同一个文件进行了修改

     > 执行`git pull`命令合并后，会将冲突部分在文件中展现，修改文件后再进行提交即可完成本地仓库的更新