# **Git 实践**

## 一、修改commit的信息（Modify the commit information）

### （一）修改最近一次的提交信息

直接使用`--amend`参数修改最近的提交信息，只适用于还未推送到远程仓库的提交

```bash
git commit --amend -m <"nerw message">
# 例如
git commit --amend -m "Modify the commit message"
```

