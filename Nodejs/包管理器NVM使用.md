# nvm

官网：https://nvm.p6p.net/

安装教程：https://nvm.p6p.net/install.html

特点：多版本管理、简便安装和切换、独立环境、跨平台支持。

## 命令

### 安装node.js版本

```shell
nvm list available    #显示可下载版本的部分列表
nvm install latest    #安装最新版本
nvm install 版本号     #例：nvm install 8.12.0
```

### 查看可用版本

```shell
nvm ls 或 nvm list
```

### node版本切换和卸载

切换指定node版本：`nvm use <version>`

卸载指定node版本：`nvm uninstall <version>`

### nvm切换国内镜像

1、修改配置文件 

在 [nvm](https://nvm.p6p.net/) 的安装路径下，找到 `settings.txt`文件，设置`node_mirror`与`npm_mirror`为国内镜像地址。在文件末尾加入：

```bash
#阿里云镜像
node_mirror: https://npmmirror.com/mirrors/node/
npm_mirror: https://npmmirror.com/mirrors/npm/
```

```bash
#腾讯云镜像
node_mirror: http://mirrors.cloud.tencent.com/npm/
npm_mirror: http://mirrors.cloud.tencent.com/nodejs-release/
```

2、命令行切换（注意请切换国内镜像后再安装 node 版本，否则会很慢）

设置`Node.Js`镜像： `nvm node_mirror [url]`

设置`npm`镜像： `nvm npm_mirror [url]`

```bash
#阿里云镜像
nvm npm_mirror https://npmmirror.com/mirrors/npm/
nvm node_mirror https://npmmirror.com/mirrors/node/
```

```bash
#腾讯云镜像
nvm npm_mirror http://mirrors.cloud.tencent.com/npm/
nvm node_mirror http://mirrors.cloud.tencent.com/nodejs-release/
```

### nvm卸载/删除

要手动删除 NVM，请执行以下操作：

首先，使用 `nvm unload` 从终端会话中移除 nvm 命令，并删除安装目录：

```bash
$ nvm_dir="${NVM_DIR:-~/.nvm}"
$ nvm unload
$ rm -rf "$nvm_dir"
```

编辑 `~/.bashrc`（或其他 shell 资源配置文件），并删除以下行：

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[[ -r $NVM_DIR/bash_completion ]] && \. $NVM_DIR/bash_completion
```

通过以上步骤，你可以手动卸载 NVM。

### 其他命令

参考：https://nvm.p6p.net/use/command.html

