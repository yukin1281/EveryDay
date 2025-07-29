# 进程和服务管理

## 常用命令

- 查看当前系统进程

```bash
ps -ef | grep nginx
```

- `kill`/`killall`/`pkill` 终止进程

```bash
kill -9 pid      # 强制杀死（SIGKILL）
killall nginx     # 杀死所有名为 nginx 的进程
pkill -f "java"       # 根据完整命令行匹配
pkill -u user1        # 杀掉用户 user1 的进程
```

- `nohup` / `&` / `disown`：后台运行进程

```>& 
nohup command > output.log 2>&1 &
nohup java -jar xxxx.jar >/dev/null 2>&1 &
```

- `lsof` / `fuser`：查看进程占用的资源或端口

```bash
lsof -i :80           # 查看占用 80 端口的进程
fuser -n tcp 80       # 同上，显示 PID
```

