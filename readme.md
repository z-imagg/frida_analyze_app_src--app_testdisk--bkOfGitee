
**依赖安装**
1. [依赖安装:nodejs](http://giteaz:3000/frida_analyze_app_src/app_testdisk#3-%E4%BE%9D%E8%B5%96%E5%AE%89%E8%A3%85nodejs)
2. [依赖安装:Miniconda3](http://giteaz:3000/frida_analyze_app_src/app_testdisk#4-%E4%BE%9D%E8%B5%96%E5%AE%89%E8%A3%85miniconda3)
3. [依赖安装:neo4j](http://giteaz:3000/frida_analyze_app_src/app_testdisk#5-%E4%BE%9D%E8%B5%96%E5%AE%89%E8%A3%85-neo4j)
4. [依赖安装: cytoscape-unix-3.10.2](http://giteaz:3000/frida_analyze_app_src/app_testdisk#6-%E4%BE%9D%E8%B5%96%E5%AE%89%E8%A3%85-cytoscape-unix-3102)

**可视化 testdisk 流程概要**
0. [克隆本仓库](http://giteaz:3000/frida_analyze_app_src/app_testdisk#0-%E5%85%8B%E9%9A%86%E6%9C%AC%E4%BB%93%E5%BA%93)
1. [编译](http://giteaz:3000/frida_analyze_app_src/app_testdisk#1-%E7%BC%96%E8%AF%91)
2. [运行](http://giteaz:3000/frida_analyze_app_src/app_testdisk#2-%E8%BF%90%E8%A1%8C)
3. [监控运行(产生日志)](http://giteaz:3000/frida_analyze_app_src/app_testdisk#7-%E7%9B%91%E6%8E%A7%E8%BF%90%E8%A1%8C%E4%BA%A7%E7%94%9F%E6%97%A5%E5%BF%97)
4. [日志格式转换](http://giteaz:3000/frida_analyze_app_src/app_testdisk#8-%E6%97%A5%E5%BF%97%E5%A4%84%E7%90%86)
5. [日志可视化](http://giteaz:3000/frida_analyze_app_src/app_testdisk#9-%E6%97%A5%E5%BF%97%E5%8F%AF%E8%A7%86%E5%8C%96)

# 依赖安装
## 1. 依赖安装:nodejs

参考, [wiki.git/nvm_install_nodejs.md.sh](http://giteaz:3000/wiki/wiki/src/branch/main/computer/nvm_install_nodejs.md.sh)

```shell
sudo mkdir /app; sudo chown z.z /app

git clone  -b v0.39.5 https://gitclone.com/github.com/nvm-sh/nvm.git /app/nvm
#原始仓库为: https://github.com/nvm-sh/nvm.git

#导入nvm等命令
source /app/nvm/nvm.sh
export NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node/
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node/
#将以上三行  写入 文件 ~/.bashrc 的最末尾   ， 以命令 'gedit ~/.bashrc' 打开该文件,  拖动滚动条 到文件最末尾， 粘贴以上两行

#以nvm命令安装nodejs-v18.19.1
NVM_NODEJS_ORG_MIRROR=http://nodejs.org/dist nvm install v18.19.1
#nodejs中有node、npx等命令
which node 
#/app/nvm/versions/node/v18.19.1/bin/node
which npx 
#/app/nvm/versions/node/v18.19.1/bin/npx

#nodejs镜像设置为国内淘宝镜像
npm config -g set registry=https://registry.npmmirror.com 

#查看已安装nodejs
nvm list
#查看远端nodejs各版本
nvm ls-remote

```

重新登陆当前终端，以迫使  ```~/.bashrc``` 中新增的内容被执行



## 2. 依赖安装:miniconda3
参考, [bash-simplify.git/miniconda3install.sh](http://giteaz:3000/bal/bash-simplify/src/branch/release/miniconda3install.sh)

```shell
cd /tmp/
#制作Miniconda3安装包的数字摘要
#  数字摘要 == 数字签名
cat << 'EOF' > Miniconda3-py310_22.11.1-1-Linux-x86_64.sh.md5sum.txt
e01420f221a7c4c6cde57d8ae61d24b5  Miniconda3-py310_22.11.1-1-Linux-x86_64.sh
EOF

#若 数字摘要 验证不过, 则下载
md5sum --check Miniconda3-py310_22.11.1-1-Linux-x86_64.sh.md5sum.txt || \
wget  https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py310_22.11.1-1-Linux-x86_64.sh  --output-document=Miniconda3-py310_22.11.1-1-Linux-x86_64.sh 

hm=/app/Miniconda3-py310_22.11.1-1
#当不存在activate文件时,
[[ ! -f $hm/bin/activate ]] && \
sudo mkdir -p $hm && \
sudo chown -R $(id -gn).$(whoami) $hm && \
#安装Miniconda3
bash Miniconda3-py310_22.11.1-1-Linux-x86_64.sh -b -u -p $hm

```


## 3. 依赖安装: neo4j
neo4j-4.4.32-community 安装 (以docker运行), [analyze_by_graph.git/release_qphotorec/neo4j-4.4.32-community--env-docker.md](http://giteaz:3000/frida_analyze_app_src/analyze_by_graph/src/tag/release_qphotorec/neo4j-4.4.32-community--env-docker.md)


## 4. 依赖安装: cytoscape-unix-3.10.2

```shell
bash -x <(curl http://giteaz:3000/frida_analyze_app_src/analyze_by_graph/raw/tag/release_qphotorec/doc/cytoscape_unix_dl_install.sh)
```

即 http://giteaz:3000/frida_analyze_app_src/analyze_by_graph/src/tag/release_qphotorec/doc/cytoscape_unix_dl_install.sh

其中 [github.com/.../cytoscape-unix-3.10.2.tar.gz](https://github.com/cytoscape/cytoscape/releases/download/3.10.2/cytoscape-unix-3.10.2.tar.gz) 国内直接下载很慢，请从别处复制，或爬墙下载

# 可视化 testdisk  流程概要

## 0. 克隆本仓库

添加代码仓库gitea私服域名
```shell
echo '10.0.4.9 westgw giteaz g'|sudo tee -a /etc/hosts
```

安装git
```shell
sudo apt install --yes git
```

克隆仓库app_testdisk
```shell
sudo mkdir /fridaAnlzAp
sudo chown z.z /fridaAnlzAp
git clone http://giteaz:3000/frida_analyze_app_src/app_testdisk.git /fridaAnlzAp/app_testdisk

cd /fridaAnlzAp/app_testdisk;  git  submodule    update --recursive --init 
```
## 1. 编译

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

下载lazygit 并 复制lazygit到docker实例
```shell
#国内访问一般github很慢
wget https://github.com/jesseduffield/lazygit/releases/download/v0.41.0/lazygit_0.41.0_Linux_x86_64.tar.gz -output-document  ~/lazygit

docker cp ~/lazygit testdiskEnv:/bin/lazygit
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


以下 2. 在 宿主机 中运行，  （理由是 docker实例下图形化界面较麻烦） 


## 2. 运行

#### 2.1 准备磁盘

请跳过，该u盘已准备好了

对u盘创建6个分区, [cgsecurity--testdisk.git/u_disk_create.sh](https://gitee.com/disk_recovery/cgsecurity--testdisk/blob/fridaAnlzAp/qphotorec/u_disk_create.sh)

插入事先准备好的u盘

运行```gnome-disks```，观察u盘的设备文件 比如可能是```/dev/mmcblk0```


#### 2.2 使用testdisk


用testdisk列出u盘的各分区
```
sudo /fridaAnlzAp/cgsecurity--testdisk/src/testdisk  /list /dev/mmcblk0
```

理论上 testdisk 也可以被分析， 但 这里暂时不分析 testdisk ，转而 分析 qphotorec

#### 2.3  安装 运行qphotorec所需库

运行qphotorec需要qtbase5-dev
```shell
sudo apt install --yes qtbase5-dev
```
qttools5-dev-tools ： 编译qphotorec时需要， 但运行qphotorec时不需要


#### 2.4 使用qphotorec

用qphotorec恢复磁盘的某分区中已删除的文件
```shell
sudo /fridaAnlzAp/cgsecurity--testdisk/src/qphotorec    /dev/mmcblk0
```



# 3. 监控运行（产生日志）


#### 3.1 克隆代码仓库frida_js
frida_js： 监控qphotorec
```shell
git clone -b release_qphotorec http://giteaz:3000/frida_analyze_app_src/frida_js.git  /fridaAnlzAp/frida_js
```


#### 3.2 frida_js监视qphotorec以产生函数调用日志

frida_js监控地运行qphotorec 以生成 函数进出日志、进出时刻点日志
```shell
bash -x /fridaAnlzAp/frida_js/fridaJs_runApp.sh
```

qphotorec被frida_js监控地运行 ， 会感觉很卡，这是正常的。 界面操作， 卡住时应该等待， 卡住时不要多次点击。

qphotorec的图形化界面出来后：
  - 顶部 "Please select a media to recover from" --> 下拉列表中 选择 u盘。  若默认选择的已是u盘，则此步不需要。
  - 选择最小的分区（这样耗时短, 该分区尺寸越30MB）
  - "Please select a destination to save the recovered files to."  --> 点击 右下角"Browse"  -->  弹出目录选择器,比如选```/home/z``` --> 点击右上角"Open"
  - 点击 底部"Search"  即开始恢复该分区中已删除的文件  
  - 进入恢复文件进度条界面， 等待进度条走完， 点击 底部"Quit"
  - 本次监控运行完毕

  本次监控运行 耗时约 1小时,  产生的日志文件 尺寸约350MB、 行数约100万行


# 4. 日志格式转换

#### 4.1 克隆代码仓库analyze_by_graph

analyze_by_graph：  日志处理

```shell
git clone -b release_qphotorec http://giteaz:3000/frida_analyze_app_src/analyze_by_graph.git  /fridaAnlzAp/analyze_by_graph
```



#### 4.2 analyze_by_graph 日志格式转换： 从 *.log文件 到 sqlite3表 到 neo4j表V_FnCallLog

```shell
source /app/Miniconda3-py310_22.11.1-1/bin/activate
bash -x /fridaAnlzAp/analyze_by_graph/_main.sh
```

粗略估计 需要运行1到2个小时

# 5. 日志可视化

#### 用词解释
- chainBegin_fnCallId : 链条的开始fnCallId
- chainBegin_fnCallId : 链条的结束fnCallId
- beginW : 开始宽度
- w1BeginD : 宽度大于等于1时 的 开始深度


#### 5.1 修改 过滤条件: query__链条_宽_宽1深.cypher
[/fridaAnlzAp/analyze_by_graph/cypher_src/query__链条_宽_宽1深.cypher](http://giteaz:3000/frida_analyze_app_src/analyze_by_graph/src/tag/release_qphotorec/cypher_src/query__%E9%93%BE%E6%9D%A1_%E5%AE%BD_%E5%AE%BD1%E6%B7%B1.cypher) 

neo4j的web控制台 http://localhost:7474/browser/  ，  用户名 neo4j 、密码 123456  

##### 填写最长链条的 开始、结束 fnCallId 到 chainBegin_fnCallId 、chainEnd_fnCallId
```c++
// 1. 查看 各 链条 开始、结束 fnCallId， 
MATCH (v:V_Chain__BzWriteDeepth)   RETURN v.root_fnCallId as chainBegin_fnCallId, v.end_fnCallId as chainEnd_fnCallId 
```
填充 [/fridaAnlzAp/analyze_by_graph/cypher_src/query__链条_宽_宽1深.cypher](http://giteaz:3000/frida_analyze_app_src/analyze_by_graph/src/tag/release_qphotorec/cypher_src/query__%E9%93%BE%E6%9D%A1_%E5%AE%BD_%E5%AE%BD1%E6%B7%B1.cypher)  中的 chainBegin_fnCallId 为 把最长的链条 的 chainBegin_fnCallId 、 chainEnd_fnCallId 为 把最长的链条 的 chainEnd_fnCallId

##### 人工尝试不同的 beginW 、 w1BeginD 使得 '点数' 约大几千个

将 末尾的注释```// return count(v) as 点数 ``` 放开,  为了 人工尝试不同的 beginW 、 w1BeginD :
（以下是拍脑袋的经历，称不上经验）
在目前电脑配置下， 约1万个点， cytoscape能迅速展示出，几万个点会慢一些、几百万个点展示不出来

当'点数'比千还少，可能会出现许多孤立点群，这样也许不是想要的。

当 返回的'点数'5千到1万之间  cytoscape能正常显示 且 孤立点群 少， 这也许是想看的。

beginW 从2到 、 w1BeginD [/fridaAnlzAp/analyze_by_graph/cypher_src/query__链条_宽_宽1深.cypher](http://giteaz:3000/frida_analyze_app_src/analyze_by_graph/src/tag/release_qphotorec/cypher_src/query__%E9%93%BE%E6%9D%A1_%E5%AE%BD_%E5%AE%BD1%E6%B7%B1.cypher)

#### 5.2 根据过滤条件 将 日志表V_FnCallLog 转为 可视化表V_FnCallLog_Analz

修改好 query__链条_宽_宽1深.cypher 后， 执行 _main_create_neo4jTable_for_cytoscape
```shell
bash -x /fridaAnlzAp/analyze_by_graph/visual/_main_create_neo4jTable_for_cytoscape.sh
```
即可获得 可视化表V_FnCallLog_Analz

#### 5.3 将neo4j的 点V_FnCallLog_Analz、边E_P2S 可视化到 cytoscape

 [cytoscape可视化应用程序qphotorec函数调用日志半成品(参考样例)](http://giteaz:3000/frida_analyze_app_src/analyze_by_graph/src/commit/aed2f1cbe736f3f42e6a3a9db3075f50571f2589/visual/cytoscape__testdisk_qphotorec/readme.md)



# 参考
1. http://giteaz:3000/frida_analyze_app_src/fridaAnlzAp_env/src/commit/534efcedcb81f71f3963480ca74c3ecac73d1269/testdisk.md


# 术语

- 【术语】 qphotorec==带qt的testdisk
