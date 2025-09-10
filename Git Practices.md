# **Git 实践**

## 一、修改commit的信息（Modify the commit information）

### （一）修改最近一次的提交信息

直接使用`--amend`参数修改最近的提交信息，只适用于还未推送到远程仓库的提交

```bash
git commit --amend -m <"nerw message">
# 例如
git commit --amend -m "Modify the commit message"
```

### （二）修改之前的提交信息

1. 使用交互式 rebase

   ```bash
   git rebase -i HEAD~n
   # 其中n为阿拉伯数字，表示要修改最新的几次提交
   # 例如
   git rebase -i HEAD~2
   ```

2. 在打开的`vi`编辑器中将会出现一下类似内容，各提交按照是时间先后顺序自上而下排列

   ```
   pick 1234567 CommitMessage1
   pick 89abcde CommitMessage2
   ```

3. 将需要修改的提交前的`pick`改为`reward`或者`r`

   ```
   reward 1234567 CommitMessage1
   pick 89abcde CommitMessage2
   ```

4. 保存退出`vi`编辑器后，`Git`会依次打开每个需要修改的提交的信息，继续使用`vi`编辑器进行修改保存退出即可

5. 修改后的信息的提交需要使用强制推送

   ```bash
   git push origin <branch name> --force
   # 例如
   git push origan main --force
   ```

   

