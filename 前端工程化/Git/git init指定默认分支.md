# git init时指定默认分支

## 1.修改默认分支

在初始化时指定默认分支`main`

```shell
git init -b main
```

## 2.初始化之后修改

已经初始化，修改本地分支，如果原来默认分支为`master`，修改分支为`main`

```shell
git branch -m master main
```

## 3.全局修改

全局修改默认分支

```shell
git config --global init.defaultBranch main
```