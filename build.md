
启动docker实例
```shell
docker run --name testdiskEnv --hostname testdiskEnv --interactive --tty --detach ubuntu:22.04
#docker start testdiskEnv
```

复制lazygit到docker实例 或 下载lazygit
```shell
docker cp Download/lazygit testdiskEnv:/bin/lazygit
# wget https://github.com/jesseduffield/lazygit/releases/download/v0.41.0/lazygit_0.41.0_Linux_x86_64.tar.gz -o /bin/lazygit
```

进入docker实例终端
```
docker exec --interactive --tty testdiskEnv  /bin/bash
```

在docker实例终端执行
```shell
apt update

```

```shell
apt install -y git
```

```shell
apt install -y build-essential autoconf pkg-config 
```

```shell
apt install -y qtbase5-dev
apt install  -y qttools5-dev-tools

#检查是否有qt库
pkg-config  --list-all | grep -i qt
```

```shell
git clone -b release https://gitee.com/disk_recovery/cgsecurity--testdisk.git

```


参考
1. http://giteaz:3000/frida_analyze_app_src/app_env/src/commit/534efcedcb81f71f3963480ca74c3ecac73d1269/testdisk.md