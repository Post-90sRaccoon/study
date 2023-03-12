# git 提示文件被修改

* 提示文件被修改，但是没有修改，可能因为文件权限改变导致的。windows 下和 linux 下保存文件权限的方式不同，会损失信息。

```BASH
git config --global core.filemode false
```

