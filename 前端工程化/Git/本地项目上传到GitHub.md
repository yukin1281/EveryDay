# 本地项目上传GitHub

git下载地址：https://git-scm.com/downloads

## 一、流程

- 首先先在Github中创建远程仓库（项目）
- 然后准备好本地项目
- 最后通过Git命令进行本地项目和远程仓库关联、推送等操作

## 二、创建远程仓库

GitHub创建远程仓库，名字最好和项目名称一致。

复制远程仓库地址（SSH或HTTPS）

SSH和HTTPS的区别在于SSH需要配置SSH Key（无需账号密码验证），HTTPS则无需配置SSH KEY（拉取推送时需要用户和密码验证）

## 三、本地项目上传到GitHub

### 1.初始化

初始化时，确保本地分支和远程仓库分支名称一致。

```shell
git init
```

### 2.关联远程仓库

如果使用`https`地址，需要先配置用户名和邮箱，`ssh`地址不需要配置，但需要配置`ssh key`

```shell
git config --global user.name "username"
git config --global user.email  useremail@qq.com
```

```shell
git remote add origin 远程仓库地址(SSH/HTTPS)
```

### 3.拉取远程仓库文件到本地

```shell
git pull origin main
```

### 4.将本地文件添加到暂存区

```shell
git add .
```

### 5.暂存区提交到仓库区

```shell
git commit -am "提交时的描述信息，如提交了哪些内容"
```

### 6.文件推送到远程仓库

```shell
git push origin main
```

如果本地分支名和远程仓库分支名不一致，如远程分支`main`，本地分支`master`，修改本地分支名。

```shell
git branch -m master main   #master分支名改为main
```

## 常见问题

控制面板（Win + R 输入control）--用户账户--凭证管理器--普通凭证（添加github账户和密码）
