# git log

## 查询指定条数

```bash
git log -number
```

## 查询指定作者

```bash
git log –author=‘TIm’
```

## 查指定时间段的日志

```bash
git log –after=2022-1-1 –before=2022-1-5
```

```bash
git log –until=1.year.ago  
#查询一年前的记录 还有 since 关键词
```

## 查询包含指定描述内容的提交记录

```bash
git log –grep=‘fix’
```

## 查询指定分支的提交记录

* 默认是当前分支。

```bash
git log branchName
```

## 查询指定 commit 之间的提交记录

```bash
git log commit1..commit2
git log feature..dev #可以查看 merge 之前合并到 dev 的 commit 有哪些
```

## 查看指定文件的提交记录

```bash
git log branch -- a.txt  # -- 前面是分支名 后面是文件名
```

## 显示单行信息

```bash
git log --online
```

## 显示每条记录中文件修改的具体行数和行体统计

```bash
git log --stat
```

