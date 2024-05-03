
启动docker实例
```shell
docker run --name testdiskEnv --hostname testdiskEnv --interactive --tty --detach ubuntu:22.04
#docker start testdiskEnv
```

进入docker实例终端
```
docker exec --interactive --tty testdiskEnv  /bin/bash
```

在docker实例终端执行
```shell
apt update
```