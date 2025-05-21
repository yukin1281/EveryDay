# SSH Key生成

```
SSH KEY的配置是为了使用SSH方式推送拉取项目，配置后无需验证账号密码
如果仅使用HTTPS方式推送拉取就不需要配置SSH KEY，只需要验证账号密码即可
```

进入git bash窗口

**检查SSH Key**

输入命令，检查本机是否有SSH Key

```shell
cd ~/.ssh
```

如显示：`No such file or directory`，表示没有ssh key

**生成SSH**

输入命令创建ssh，`-C`参数后面跟上`github`邮箱账号

```shell
ssh-keygen -t rsa -C "xxx@163.com"
```

接下来三次回车：第一次确生成key的存储位置，第二次确认密码（直接回车默认无密码），第三次再次确认密码。

在`C:\用户\admin\.ssh下`查看

`id_rsa`是私钥
`id_rsa.pub`是公钥，如github绑定SSH key时使用的就是这个公钥中的内容

# Git绑定Github

打开`id_rsa.pub`公钥

```tex
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDCbJMfHki0XOpyUS0x6VNXKlksLK8oydAjpJIR/NPBXy3qxx7jmdtSInDmRApa5nUduRPpAaVoyvd/AuNXLgPU836TwhYiv+cgmsBJf0GkDIEbsmpnLlbIoa7gjVIfC683U8gCoPn6hm8AXI0flVTDH7+wRQueKACZFogT/JJaXg8SWnwaeQHddh7gVDK7mCOnOzpgJQc58iS4+szdgUOfOB6BpQe/9h0Qf5namM2FSmfbPwnXD8TXNgBn7rhWif4afyRYYGwZcd88Hwr8jzJRHPd9ZXt7R+LRkOdf28/Bl17hBM3h+hqpPnGmHKvrStbREhRGoVAGWseOJCjZOuJiECuqE25NDsoKXymHZ3NcpcpQUixALfdxmIgWsHyYkI7f64pka6QOVmeNpzVDQDDtHmq93T5ka+RrZmugrixsf0AeGJOQskYQBJPznxewqvqz4NY2s2wKWWTXwW/iJHn2OkQpylbB797JiuEwj9ubBQoYsBW4pWCF+8YSpZk/ARs= 1409640444@qq.com
```

github设置打开`SSH and GPG keys`，添加SSH Key

添加完成后验证

```shell
ssh -T git@github.com
```



