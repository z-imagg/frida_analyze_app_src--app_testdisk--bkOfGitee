
#### docker实例
停止、删除 docker实例
```shell
docker stop testdiskEnv
docker rm testdiskEnv
```

启动docker实例
```shell
docker run --name testdiskEnv --hostname testdiskEnv  --volume /app:/app --interactive --tty --detach ubuntu:22.04
#docker start testdiskEnv
```

复制lazygit到docker实例 或 下载lazygit
```shell
docker cp /app/bin/lazygit testdiskEnv:/bin/lazygit
# wget https://github.com/jesseduffield/lazygit/releases/download/v0.41.0/lazygit_0.41.0_Linux_x86_64.tar.gz -output-document  /bin/lazygit
```

进入docker实例终端
```
docker exec --interactive --tty testdiskEnv  /bin/bash
```

#### '编译testdisk'所需依赖


在docker实例终端执行
```shell
apt update

```


编译工具
```shell
apt install --yes build-essential autoconf pkg-config automake
```

其他工具
```shell
apt install --yes file
```

testdisk依赖的库
```shell
apt install --yes qtbase5-dev qttools5-dev-tools libncurses5-dev 
#若询问时区，请选 : '6. Asia'  --> '70.Shanghai'

#检查是否有qt库
pkg-config  --list-all | grep -i qt
#检查是否有curses库
pkg-config  --list-all | grep -i curses
```

#### 编译testdisk

代码仓库git
```shell
apt install --yes git
```

拉取testdisk代码
```shell
rm -fr /app/cgsecurity--testdisk
git clone -b v7.3 https://gitee.com/disk_recovery/cgsecurity--testdisk.git  /app/cgsecurity--testdisk
#或者 -b release

#人工查看testdisk代码仓库
#cd /app/cgsecurity--testdisk
#lazygit
```
编译testdisk
```shell
cd /app/cgsecurity--testdisk

#删除目录config
rm -frv config
#清理目录src内的编译产物
( cd src; make clean; )
#清理编译产物
make clean; make distclean  ;  make maintainer-clean ;
#生产编译配置(编译配置主要是Makefile)
./autogen.sh
#编译
./compile.sh
```

查看编译产物
```shell
ls -lh  src/testdisk  src/qphotorec
file src/testdisk  src/qphotorec
```

#### 使用testdisk
回到宿主机 （理由是 docker实例下图形化界面较麻烦） 

插入事先准备好的磁盘（有小尺寸分区）

```
#列出分区
/app/cgsecurity--testdisk/src/testdisk /list   hd.img 
```

#### 参考
1. http://giteaz:3000/frida_analyze_app_src/app_env/src/commit/534efcedcb81f71f3963480ca74c3ecac73d1269/testdisk.md