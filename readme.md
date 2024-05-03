
**分析 testdisk/qphotorec 大致流程**

1. [编译](http://giteaz:3000/frida_analyze_app_src/app_testdisk#1-%E7%BC%96%E8%AF%91)
2. [运行](http://giteaz:3000/frida_analyze_app_src/app_testdisk#2-%E8%BF%90%E8%A1%8C)
3. [监控运行（产生日志）](http://giteaz:3000/frida_analyze_app_src/app_testdisk#3-%E7%9B%91%E6%8E%A7%E8%BF%90%E8%A1%8C%E4%BA%A7%E7%94%9F%E6%97%A5%E5%BF%97)
4. [日志处理](http://giteaz:3000/frida_analyze_app_src/app_testdisk#4-%E6%97%A5%E5%BF%97%E5%A4%84%E7%90%86)
5. [日志可视化](http://giteaz:3000/frida_analyze_app_src/app_testdisk#5-%E6%97%A5%E5%BF%97%E5%8F%AF%E8%A7%86%E5%8C%96)

# 1. 编译

#### 1.0 编译环境(docker)准备

##### 1.0.a ubuntu22.04下的docker安装

[wiki.git/ubuntu22.04下的docker安装](http://giteaz:3000/wiki/wiki/src/branch/main/computer/ubuntu22.04.3/docker_install_at_ubuntu_22.04.3.md#1-ubuntu2204%E4%B8%8B%E7%9A%84docker%E5%AE%89%E8%A3%85)

##### 1.0.b 编译环境启动

停止、删除 docker实例
```shell
docker stop testdiskEnv
docker rm testdiskEnv
```

启动docker实例
```shell
docker run --name testdiskEnv --hostname testdiskEnv  --volume /fridaAnlzAp:/fridaAnlzAp --interactive --tty --detach ubuntu:22.04
#docker start testdiskEnv
```

复制lazygit到docker实例 或 下载lazygit
```shell
docker cp /fridaAnlzAp/bin/lazygit testdiskEnv:/bin/lazygit
# wget https://github.com/jesseduffield/lazygit/releases/download/v0.41.0/lazygit_0.41.0_Linux_x86_64.tar.gz -output-document  /bin/lazygit
```

进入docker实例终端
```
docker exec --interactive --tty testdiskEnv  /bin/bash
```


1.1 、 1.2 、 1.3 都在docker实例终端执行

#### 1.1 编译环境搭建


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

#### 1.2 编译testdisk

代码仓库git
```shell
apt install --yes git
```

拉取testdisk代码
```shell
rm -fr /fridaAnlzAp/cgsecurity--testdisk
git clone -b v7.3 https://gitee.com/disk_recovery/cgsecurity--testdisk.git  /fridaAnlzAp/cgsecurity--testdisk
#或者 -b release

#人工查看testdisk代码仓库
#cd /fridaAnlzAp/cgsecurity--testdisk
#lazygit
```
编译testdisk, 参考[cgsecurity--testdisk.git/tag_release/build.sh](https://gitee.com/disk_recovery/cgsecurity--testdisk/blob/tag_release/build.sh)
```shell
cd /fridaAnlzAp/cgsecurity--testdisk

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

#### 1.3 编译产物
```shell
ls -lh  src/testdisk  src/qphotorec
file src/testdisk  src/qphotorec
```

# 2. 运行

#### 2.1 准备磁盘

请跳过，该u盘已准备好了

对u盘创建6个分区, [cgsecurity--testdisk.git/u_disk_create.sh](https://gitee.com/disk_recovery/cgsecurity--testdisk/blob/fridaAnlzAp/qphotorec/u_disk_create.sh)

#### 2.2 使用testdisk
回到宿主机 （理由是 docker实例下图形化界面较麻烦） 

插入事先准备好的磁盘（存储卡，有小尺寸分区）

用testdisk列出磁盘的各分区
```
sudo /fridaAnlzAp/cgsecurity--testdisk/src/testdisk  /list /dev/sda
```

用qphotorec恢复磁盘的某分区中已删除的文件
```shell
sudo /fridaAnlzAp/cgsecurity--testdisk/src/qphotorec    /dev/sda
```

# 3. 监控运行（产生日志）

#### 3.1 frida_js监视testdisk以产生函数调用日志

frida_js生成 函数进出日志、进出时刻点日志,[frids_js/f8d80/fridaJs_runApp.sh](http://giteaz:3000/frida_analyze_app_src/frida_js/src/commit/f8d80c10899042cd7d660d93dc5c2b107db01d2f/fridaJs_runApp.sh),  [frids_js/ok/qphotorec_01/fridaJs_runApp.sh](http://giteaz:3000/frida_analyze_app_src/frida_js/src/tag/ok/qphotorec_01/fridaJs_runApp.sh)

# 4. 日志处理

# 5. 日志可视化

#### 5.1 analyze_by_graph分析函数调用日志

[cytoscape可视化应用程序qphotorec函数调用日志半成品](http://giteaz:3000/frida_analyze_app_src/analyze_by_graph/src/commit/aed2f1cbe736f3f42e6a3a9db3075f50571f2589/visual/cytoscape__testdisk_qphotorec/readme.md)

# 参考
1. http://giteaz:3000/frida_analyze_app_src/fridaAnlzAp_env/src/commit/534efcedcb81f71f3963480ca74c3ecac73d1269/testdisk.md


# 术语

- 【术语】 qphotorec==带qt的testdisk
